<template>
<!--  我的订单-->
<div >
  <div class="head">
    <p><span class="lineC"></span>我的订单</p>
    <span @click="back" class="point">< 返回</span>
  </div>
  <div class="search">
    <div class="orderNo">
      <span>订单 :</span>
      <input type="text" placeholder="请输入订单编号" v-model="orderNo">
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
      <span class="point" @click="toSearch"><i class="iconfont icon-ziyuan"></i></span>
    </div>
  </div>
  <div class="mainOrder">
     <div class="tab">
       <p @click="tabId=1" :class="{clickP:tabId==1}" class="point">产品订单</p>
       <p @click="tabId=2" :class="{clickP:tabId==2}" class="point">船舶订单</p>
     </div>
     <div class="nav">
       <p v-for="item in options" @click="choose(item)" :class="{clickP:statusL==item.label}" class="point">
         {{item.label}}
       </p>
     </div>
     <div class="tableOrder">
       <el-table
        :data="tableData"
        style="width: 100%">
          <el-table-column
            prop="no"
            align="center"
            width="70"
            label="序号">
          </el-table-column>
          <el-table-column
            prop="orderNo"
            width="180"
            label="订单编号">
          </el-table-column>
          <el-table-column
            width="200"
            prop="productName"
            label="产品名称">
          </el-table-column>
          <el-table-column
            width="90"
            prop="productParam"
            label="产品参数">
          </el-table-column>
          <el-table-column
            prop="num"
            width="100"
            align="center"
            label="数量">
          </el-table-column>
          <el-table-column
            prop="realPayment"
            width="120"
            label="实付款">
          </el-table-column>
          <el-table-column
            prop="status"
            width = "100"
            label="交易状态">
          </el-table-column>
          <el-table-column
            fixed="right"
            align="center"
            width="120"
            prop="operations"
            label="交易操作">
            <div class="btnList">
              <el-button type="text" size="small">查看详情</el-button>
              <el-button type="danger" size="small" plain>去支付</el-button>
             <!-- <el-button type="text" size="small">取消订单</el-button>
              <el-button type="text" size="small">查看物流</el-button> -->
              <!-- <el-button type="primary"  size="small">确认收货</el-button>
              <el-button type="text" size="small">查看详情</el-button>
              <el-button type="text" size="small">删除订单</el-button> -->
            </div>
            <!-- <template slot-scope="scope">
              <el-button @click="handleClick(scope.row)" type="text" size="small">查看</el-button>
              <el-button  type="danger" size="small">编辑</el-button>
            </template> -->
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
    export default {
      layout:'person',
        name: "shipowner",
      components: {
        pageInation
      },
      data(){
          return{
            orderNo:'',
            cd:'',
            shipNm:'',
            tabId:1,
            statusL:'全部',
            currentPage1: 1,      //前往哪页
            pageSize:4,       //每页数量
            total:33,         //总数
            options: [{
                value: '0',
                label: '全部'
              }, {
                value: '1',
                label: '待付款'
              }, {
                value: '2',
                label: '待发货'
              }, {
                value: '3',
                label: '待收货'
              }, {
                value: '4',
                label: '已完成'
              }],
            value: '',
            tableData: [{
               no: '2016-05-02',
               orderNo: '王小虎',
               productName: '上海市普陀区金沙江路 1518 弄',
               productParam: '2016-05-02',
               num: '王小虎',
               realPayment: '上海市普陀区金沙江路 1518 弄',
               status: '2016-05-02',
               // operations: '王小虎',
             }],
             list:[{nm:11},{nm:11},{nm:11},{nm:11}]
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
        tabId(){
          this.$router.push('../personal/myOrder?cdType='+this.tabId)
          // this.until.href()
        }
      },
      methods: {
        getInfo(){
          this.tableData=[]
          this.list.forEach((item,index)=>{
            this.tableData.push({ no:index+1,
               orderNo: '202007210001',
               productName: '船舶设备名称',
               productParam: 'BL112 ',
               num: index+1,
               realPayment: '¥ 1600',
               status: '待付款'})

          })
        },
        handleSizeChange(val) {
          console.log(`每页 ${val} 条`);
        },
        handleCurrentChange(val) {
          console.log(`当前页: ${val}`);
        },

        // pageChange(pageNo) {

        //   this.currentPage1=JSON.parse(pageNo)
        //   this.tableData=[]
        //   this.getInfo()
        // },
        handleCurrentChange(val){
          this.currentPage1=val
          this.tableData=[]
          this.getInfo()
        },
        choose(item){//选择状态
            this.statusL=item.label
        },
        back(){//返回
			this.$router.back()
        },
      },
    }
</script>
<style lang="less">
  .status .el-input__inner{
          height: 36px;
          line-height: 36px;
  }
  .tableOrder{
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
    .orderNo{
      width: 27.1%;
      >input{
        width: 78.9%;
        background: #FFFFFF;
        border:1px solid #DFDFDF;
        border-radius:5px;
        height: 100%;
        font-size: 14px;
        padding-left: 10px;
      }
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
  .mainOrder{
    width: 100%;
    .tab{
      margin: 0 36px 0 30px;
      border-bottom: 1px solid #2778BE;
      height: 40px;
      display: flex;
      >p{
        display: flex;
        justify-content: center;
        align-items: center;
        width:14.1%;
        height: 100%;
        color: #666666;
        font-size:14px;
        font-family:Microsoft YaHei;
        font-weight:400;
      }
      .clickP{
        background-color: #2778BE;
        color: #FFFFFF;
      }
    }
    .nav{
      display: flex;
      margin: 17px 36px 30px 30px;
      height: 40px;
      border-bottom:  1px solid rgba(0,0,0,0.1);
      >p{
        display: flex;
        justify-content: center;
        align-items: center;
        width:14.3%;
        height: 100%;
        color: #666666;
        font-size:14px;
        font-family:Microsoft YaHei;
        font-weight:400;
      }
      .clickP{
        color: #2778BE;
        border-bottom:  2px solid #2778BE;
      }
    }
    .tableOrder{
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
    margin-top: 44px;
    .btn_confirm{
      margin-left: 43px;
    }
  }
</style>
