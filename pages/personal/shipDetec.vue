<template>
<!--  检测船舶(检验检测)-->
<div >
  <ship-notice ref="notice"></ship-notice>
  <div class="head">
    <p><span class="lineC"></span>检测船舶</p>
    <span @click="back" class="point">< 返回</span>
  </div>
  <div class="search">
    <div class="endTm">
      <span>完成时间 :</span>
      <el-date-picker
        v-model="finishTm"
        type="datetimerange"
        format="yyyy-MM-dd"
        range-separator="至"
        start-placeholder="开始日期"
        end-placeholder="结束日期">
      </el-date-picker>
    </div>
    <div class="status">
      <span>状态 :</span>
      <el-select v-model="value">
          <el-option
            v-for="item in options"
            :key="item.value"
            :label="item.label"
            :value="item.value">
          </el-option>
        </el-select>
    </div>
    <div class="shipNm">
      <input type="text" placeholder="搜索船舶名称" v-model="shipNm">
      <span class="point" @click="search"><i class="iconfont icon-ziyuan"></i></span>
    </div>
  </div>
  <div class="mainFile">
     <div class="tableFile">
       <el-table
        :data="tableData"
        style="width: 100%">
          <el-table-column
            prop="cd"
            align="center"
            width="100"
            label="船舶编号">
          </el-table-column>
		  <el-table-column
		    prop="number"
		    align="center"
		    width="130"
		    label="船号">
		  </el-table-column>
          <el-table-column
            prop="nm"
            width="240"
            label="船舶名称">
            <template slot-scope="scope">
              <div style="display: flex;align-items: center;">
                <span>{{scope.row.nm}}</span>
                <!-- <img
                  :src="VRImg"
                  style="cursor: pointer"
                  v-if="scope.row.vrUrl"
                  @click="toVR(scope.row.id)"
                /> -->
              </div>
            </template>
          </el-table-column>
          <el-table-column
            width="170"
            prop="productName"
            label="客户名称">
          </el-table-column>
          <el-table-column
            width="100"
            prop="statusNm"
            label="船舶状态">
          </el-table-column>
          <el-table-column
            prop="deliveryTm"
            width="110"
            align="center"
            label="完成时间">
          </el-table-column>
          <el-table-column
            fixed="right"
            align="center"
            width="140"
            prop="operations"
            label="操作">
            <template slot-scope="scope">
              <el-button @click="toDetail(scope.row,'1')" v-if="scope.row.statusNm!='已交船'" type="text" size="small" class="addBtn">新增</el-button>
              <el-button @click="toDetail(scope.row,'2')" type="text" size="small" class="detailBtn">查看详情</el-button>
              <!--船厂、设计院、监理监造、检验检测-->
              <el-button v-if="currentRole.identityCd=='identity30' || currentRole.identityCd=='identity20' || currentRole.identityCd=='identity50' || currentRole.identityCd=='identity40'" type="text" size="small" class="detailBtn" @click="toNotice(scope.row)">节点通知</el-button>
            </template>
          </el-table-column>
       </el-table>
     </div>
  </div>
  <div class="footPage">
      <!-- <page-ination :total="total" :pageSize="pageSize" @changeP='pageChange'></page-ination> -->
      <el-pagination
        background
        @current-change="handleCurrentChange"
        :current-page.sync="currentPage1"
        :page-size="pageSize"
        layout="prev, pager, next, jumper"
        :total="total">
      </el-pagination>
  </div>
</div>
</template>

<script>
    import {mapState} from "vuex";
    import pageInation from "@/components/pageInation.vue"
    import shipNotice from "@/components/person/shipNotice"
    import VRImg from "../../assets/img/personal/VR.png";
    export default {
      layout:'person',
        name: "shipowner",
      components: {
        pageInation,
        shipNotice
      },
      data(){
          return{
            VRImg,
            orderNo:'',
            cd:'',
            shipNm:'',
            tabId:1,
            statusL:'全部',
            currentPage1: 1,      //前往哪页
            pageSize:8,       //每页数量
            total:0,         //总数
            options: [{
                value: '',
                label: '全部'
              }, {
                value: '设计中',
                label: '设计中'
              }, {
                value: '建造中',
                label: '建造中'
              }],
            value: '',
            tableData: [],
            finishTm:'',
          }
      },
      computed:{
        ...mapState([
          'bWidth',
          'width'
        ])
      },
      mounted() {
        this.tabId = this.until.getQueryString('cdType')
        this.getInfo()
      },
      watch:{
        $route(){
          this.cd = this.until.getQueryString('cdType')
        },
        value(){
          this.getInfo()
        },
        finishTm(){
          this.getInfo()
        }
      },
      methods: {
        //节点通知
        toNotice(item){
          this.$refs.notice.toNotice(item)
        },
        async getInfo(){
          this.tableData=[]
          let qry = this.query.new()
          this.currentRole=JSON.parse(this.until.seGet('currentRole'))
          this.query.toP(qry,this.currentPage1,this.pageSize)
          if(this.value){
            this.query.toW(qry,'statusNm',this.value,'LK')
          }
          if(this.finishTm){
            let timeS=this.finishTm[0]
            let timeF=this.finishTm[1]
            let timeS1=this.until.formatDate(timeS)
            timeS=timeS1.year+'-'+timeS1.month+'-'+timeS1.day+' '+timeS1.hour+':'+timeS1.minite+':'+timeS1.second
            let timeF1=this.until.formatDate(timeF)
            timeF=timeF1.year+'-'+timeF1.month+'-'+timeF1.day+' '+timeF1.hour+':'+timeF1.minite+':'+timeF1.second
            this.query.toW(qry,'deliveryTm',timeS+','+timeF,'BT')
          }
          let param={
            keyWord:this.shipNm,
            orgEnterId:this.currentRole.id
          }
		  this.query.toO(qry,'cd','asc')
          let data=await this.api.allShip(this.query.toEncode(qry),param)
          this.total=data.page.total
          data.data.list.forEach(item=>{
            item.deliveryTm=item.deliveryTm.split(' ')[0]
            this.tableData.push(item)
          })
          console.log(this.tableData)
        },
        search(){
          console.log(this.finishTm)
          this.getInfo()
        },
        // pageChange(pageNo) {
        //   console.log('页码改变'+JSON.parse(pageNo));
        //   this.currentPage1=JSON.parse(pageNo)
        //   this.tableData=[]
        //   this.getInfo()
        // },
        handleCurrentChange(val){
          this.currentPage1=val
          this.tableData=[]
          this.getInfo()
        },
        toDetail(item,type){
          if(type=='1'){
            this.$router.push('./shipFileDetail?id='+item.id+"&cdType=3&formW=jiance&cdType2=3&shipCd="+item.cd+"&shipStatus="+item.statusNm+"&fileNm="+item.nm)
          }else if(type=='2'){
            this.$router.push('./archives?id='+item.id+"&formW=jiance&shipCd="+item.cd+"&shipStatus="+item.statusNm)
          }
          console.log(item.id)

        },
        back(){//返回
			this.$router.back()
        },
        toVR(id) {
          window.open("./mframe?typeCd=3&id=" + id);
        },
      },
    }
</script>
<style lang="less">
  .endTm {  //时间选择
    .el-date-editor.el-input{
      border-radius: 5px;
      border: 1px solid rgba(223, 223, 223, 1);
      width: 81.9%;
      height: 100%;
      cursor:pointer;
    }
    .el-input__inner{
      height: 100%;
      cursor:pointer;
      border:1px solid rgba(223, 223, 223, 1);
    }
    .el-date-editor .el-range-separator{
      width: auto;
    }
  }

  .status .el-input__inner{     //下拉框
          height: 36px;
          line-height: 36px;
  }
  .tableFile{
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
    .addBtn{
      background:rgba(39,120,190,1);
      padding: 6px 27px;
      border:none;
      border-radius:3px;
      font-size:15px;
      color:#FFFFFF;
    }
    .detailBtn{
      border:none;
      color:rgba(80,80,80,1);
    }
  }
</style>
<style lang="less" scoped>
  @import url("../../assets/css/init.less");
  .point{
    cursor:pointer;
  }
  .head{
    display: flex;
    justify-content: space-between;
    align-items: center;
    height: 59px;
    width: 100%;
    padding: 0 24px 0 30px;
    border-bottom: 1px solid rgba(0,0,0,0.1);;
    >p{
      display: flex;
      align-items: center;
      font-size:20px;
      font-family:Microsoft YaHei;
      font-weight:400;
      color:#333333;
      height: 18px;
      .lineC{
        display: block;
        height: 18px;
        width: 3px;
        background-color: #2778BE;
        margin-right: 8px;
      }
    }
    >span{
      display: block;
      height:12px;
      font-size:14px;
      font-family:Microsoft YaHei;
      font-weight:400;
      color: #666666;
    }
  }
  .search{
    width: 100%;
    height: 78px;
    padding: 20px 22px;
    display: flex;
    justify-content: space-between;
    align-items: center;
    >div{
      display: flex;
      align-items: center;
      height: 38px;
      box-sizing: content-box;
      >span{
        white-space: nowrap;
        display: inline-block;
        height: 12px;
        font-size: 14px;
        margin-right: 20px;
        font-family: Microsoft YaHei;
        font-weight: 400;
        line-height: 12px;
        color: #333333;
      }
    }
    .endTm{
      width: 34%;
    }
    .status{
      width: 31.5%;
      .el-select{
        width: 81.9%;
        // height: 100%;
        background:rgba(255,255,255,1);
        border:1px solid rgba(223, 223, 223, 1);
        border-radius:5px;
        height: 100%;

      }
    }
    .shipNm{
      position: relative;
      width: 30.6%;
      background:rgba(255,255,255,1);
      border:1px solid rgba(223, 223, 223, 1);
      border-radius:5px;
      padding-left: 12px;
      >input{
        width: 77.6%;
        height: 100%;
        border: none;
      }
      >span{
        position: absolute;
        display: flex;
        justify-content: center;
        align-items: center;
        margin: 0;
        right: 0;
        width: 18.3%;
        height:100%;
        background:rgba(39,120,190,1);
        border-bottom-right-radius: 5px;
        border-top-right-radius: 5px;
        .icon-ziyuan{
          color: #ffffff;
        }
      }
    }
  }
  .mainFile{
    width: 100%;
    .tableFile{
      width: 100%;
      .btnList{
        display: flex;
        flex-direction: column;
      }
    }
  }
  .footPage{
    display: flex;
    justify-content: center;
    margin: 44px 0 20px;
    .btn_confirm{
      margin-left: 43px;
    }
  }
</style>
