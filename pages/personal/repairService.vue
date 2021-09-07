<template>
<!--  维保服务-->
    <div class="main">
      <div style="padding:0 30px">
        <!--派单-->
        <dispatch  :show="show" @toClose="show=false" :info="currentInfo" :manList="manList" @confirm="dispatchConfirm"></dispatch>
        <!--确认完成-->
        <service-order  :show="show2" @close="show2=false" :id="currentInfo.id" @confirm="serviceOrderConfirm"></service-order>
        <service-order-detail  :show="show3" @close="show3=false" :id="currentInfo.id"></service-order-detail>
        <tobbar :title="title" :show-all="false"></tobbar>
      </div>
      <p style="border-bottom: 1px solid rgba(0,0,0,0.1);"></p>

      <div class="line1">
        <p>
          维保服务单：<span>{{total}}</span>
        </p>
         工单状态：<el-select v-model="status" placeholder="请选择">
          <el-option
            v-for="item in statusList"
            :key="item.cd"
            :label="item.nm"
            :value="item.cd">
          </el-option>
        </el-select>
          <el-input v-model="keyWord" placeholder="请输入船舶编号"></el-input>
          <button @click="toSearch">查询</button>

      </div>
      <div class="tableRepair">
        <el-table
         :data="list"
         style="width: 100%">

           <el-table-column
             prop="cd"
             width="150"
             label="服务单号">
           </el-table-column>
           <el-table-column
             width="150"
             prop="appointmentTm"
             label="预约时间">
           </el-table-column>
           <el-table-column
             width="160"
             prop="addr"
             label="服务地点">
           </el-table-column>
           <el-table-column
             prop="linkman"
             width="140"
             align="center"
             label="联系人姓名">
           </el-table-column>
           <el-table-column
             prop="docsCd"
             width="110"
             label="船舶编号">
           </el-table-column>
          <el-table-column
            prop="serviceItemNm"
            width="110"
            label="服务内容">
          </el-table-column>
           <el-table-column
             prop="statusNm"
             width = "100"
             label="状态">
           </el-table-column>
           <el-table-column
             fixed="right"
             align="center"
             width="120"
             prop="operations"
             label="操作">
             <template slot-scope="scope">
                <el-button type="text" @click="toDetail(scope.row)" size="small" class="toEva">查看详情</el-button>
               <button v-if="scope.row.statusCd=='TASK_NOTE_STATUS.40' || scope.row.statusCd=='TASK_NOTE_STATUS.50' " size="small" plain class="evalutaed" @click="toDispatch(scope.row)">接单</button>
               <button v-if="scope.row.statusCd=='TASK_NOTE_STATUS.60'" size="small" plain class="evalutaed" @click="toFinish(scope.row)">确认完成</button>
             </template>
           </el-table-column>
        </el-table>
      </div>
      <div class="line5">
        <el-pagination
          @current-change="handleCurrentChange"
          :current-page="currentPage3"
          :page-size="pageSize"
          prev-text="上一页"
          next-text="下一页"
          layout="total, prev, pager, next, jumper"
          :total="total">
        </el-pagination>
      </div>


    </div>
</template>

<script>
    import tobbar from "../../components/person/tobbar";
    import searchIc from '../../assets/img/personal/search.png'
    import deleteIc from '../../assets/img/personal/delete.png'
    import successIc from '../../assets/img/success.png'
    import checked from '../../assets/img/personal/checked.png'
    import unchecked from '../../assets/img/personal/unchecked.png'
    import upIc from '../../assets/img/personal/上传图片.png'
    import del from '@/assets/img/personal/del.png'
   import Dispatch from "../../components/person/Dispatch";
   import ServiceOrder from "../../components/person/serviceOrder";
    import serviceOrderDetail from "../../components/person/serviceOrderDetail";
    export default {
        name: "allShip",
        components:{
          ServiceOrder,
          Dispatch,
            tobbar,
          serviceOrderDetail
        },

        layout:'person',
        data(){
            return{
                searchIc,
                deleteIc,
                successIc,
                checked,
                unchecked,
                upIc,
                del,
                manList:[],
                show:false,//派单
                title:'维保服务',
                showEvl:false,//评价弹框
                showAdd:false,//新增弹框
                total:0,
              currentPage3:1,
                pageSize:6,
                list:[],
                addForm:{},
                repairFrom:{
                  taskId:"",
                  speed: "非常满意",
                  attitude: "非常满意",
                  capacity: "非常满意",
                  price: "非常满意",
                  job: "",
                  moreEvel: "",
                },
                evaluateList:['非常满意','比较满意','一般','不满意'],
                currentRole:{},
                keyWord:'',
                status:'',
              currentInfo:{},
                show2:false, //确认完成
              show3:false,//详情
              statusList:[],
            }
        },
        watch:{
        },
        async mounted(){
          this.currentRole=JSON.parse(this.until.seGet('currentRole'))
          this.getList()
          this.statusList = await this.api.dataDictionary('TASK_NOTE_STATUS')
          this.manList = await this.api.toRepairMan(this.currentRole.id)
        },
        methods:{

          async getList(){
            let qry = this.query.new()
            console.log(this.currentRole)
            let param={
              orgId:this.currentRole.id
            }
            this.query.toW(qry,'statusCd',this.status,'EQ')
            this.query.toW(qry,'docsCd',this.keyWord,'LK')
            this.query.toO(qry,'crtTm','desc')
            this.query.toP(qry,this.currentPage3,this.pageSize)
            let data=await this.api.maintenR(this.query.toEncode(qry),param)
            this.total=data.page.total
            data.data.list.forEach((item,index)=>{
              item.no=index+1
              item.crtTm = item.crtTm.split(' ')[0]

            })
            this.list = data.data.list
          },
          toSearch(){
            this.currentPage3=1
            this.getList()
          },
          handleSizeChange(val) {
              console.log(`每页 ${val} 条`);
          },
          handleCurrentChange(val) {
              console.log(`当前页: ${val}`);
          },
          //派单
          toDispatch(item){
            this.currentInfo = item
            this.show=true
          },
          //完成确认
          toFinish(item){
            this.currentInfo = item
            this.show2=true
          },
          serviceOrderConfirm(){
            this.show2=false
            this.getList()
          },
          //派单确认
          dispatchConfirm(){
            this.show = false
            this.getList()
          },
          //查看详情
          toDetail(item){
            this.currentInfo = item
            this.show3=true
          },
          toClose(){
            this.show = false
          },
          close(){
            this.show2 = false
          },
          //新增维保单提交确定
          taskNoteSumbit(){
            this.showAdd = false
            this.getList()
          },
          //提交评价
          toSubmit(){
            this.store.commit('changeLoading',true)
            this.api.toRepair(this.repairFrom).then(res=>{
              console.log(res)
              this.$message({
                message: '评价成功',
                type: 'success',
                duration:'1500'
              });
              setTimeout(() => {
                this.showEvl=false
                this.getList()
              }, 1500);
            })
          },

        },

    }
</script>
<style lang="less">
  @import "../../assets/css/init.less";
  .tableRepair{
    .el-table__header thead{
      color: #666666;
      font-size:16px;
      font-weight:400;
    }
    .el-table__header th{
      background-color: #F7F7F7;
    }
    .el-table__body tr{
      color: #333333;
      font-size:15px;
    }
    .el-table__fixed-right{
      height: 100%!important;
    }
    .toEva{
      border:1px solid rgba(39, 120, 190, 1);
      border-radius:3px;
      color: #2778BE;
      padding: 9px 15px;
    }
    .evalutaed{
      background: @themeColor;
      width: 82px;
      height: 35px;
      line-height: 35px;
      text-align: center;
      color: #ffffff;
      border-radius: 2px;
      border: none;
      cursor: pointer;
    }
  }
</style>

<style scoped lang="less">
  .main{
    width: 100%;


    .line1{
      padding:0 18px 0 30px;
      width: 100%;
      height: 81px;
      position: relative;
      display: flex;
      align-items: center;
      >p{
        flex: 1;
        color: #333333;
        font-size: 18px;
        span{
          color: #E4393C;
        }

      }
      /deep/.el-select{
        width: 140px;
        /deep/.el-input{
          width: 100%;
          margin: 0;
        }
      }
      /deep/.el-input{
        width: 309px;
        margin: 0 10px;
      }
      >button{
        width: 100px;
        height: 38px;
        line-height: 38px;
        text-align: center;
        background: #E4393C;
        color: #ffffff;
        border-radius: 4px;
        border: none;
        cursor: pointer;
      }
    }
    .line5{
      padding: 30px 40px;
      width: 100%;
      display: flex;
      justify-content: flex-end;
    }
  }

  .active2{
    color: #2572B5;
    /*border-bottom: 2px solid #2572B5;*/
  }



</style>
