<template>
<div id="app">
  <offer :applyInfo="applyInfo" :offer="offer" @close="close"></offer>
  <download :download1="download1" @toClose="toClose" :applyInfo="applyInfo"></download>
  <div class="header">
    <p><span class="lineC"></span>投标管理</p>
    <span @click="back">< 返回</span>
  </div>
  <div class="body">
    <div class="search">
      <span>时间：</span>
      <div class="block">
        <el-date-picker
          v-model="value1"
          type="date"
          @change="select"
          placeholder="选择发布日期">
        </el-date-picker>
      </div>
      <span>状态：</span>
      <el-select v-model="value" clearable placeholder="请选择" class="select" @change="select"
      >
        <el-option
          v-for="item in list"
          :key="item.cd"
          :label="item.nm"
          :value="item.cd">
        </el-option>
      </el-select>
      <div class="shipNm" >
        <input type="text" placeholder="搜索项目名称" v-model="bidNm">
        <span class="point" @click="search"><i class="iconfont icon-ziyuan"></i></span>
      </div>
    </div>
    <div class="table">
      <el-table
        :data="bidList"
        :cell-style="status"
        style="width: 100%">
        <el-table-column
          type="index"
          :index="indexMethod"
          align="center"
          width="110"
          label="序号">
        </el-table-column>
        <el-table-column
          prop="nm"
          width="280"
          label="项目名称">
        </el-table-column>
        <el-table-column
          width="170"
          prop="publishTm"
          align="center"
          label="发布时间">
        </el-table-column>
        <el-table-column
          width="170"
          prop="completeTm"
          align="center"
          label="结束时间">
        </el-table-column>
        <el-table-column
          prop="statusNm"
          width = "110"
          align="center"
          label="状态">
        </el-table-column>
        <el-table-column
          align="center"
          width="110"
          fixed="right"
          prop="operations"
          label="操作">
          <div class="btnList" slot-scope="scope">
            <button class="button3" :class="{disable:scope.row.disable == true}" v-if="scope.row.statusCd == 'BID_OFFER_STATUS.40'" @click="openOffer(scope.row)">报价</button>
            <button class="button3" :class="{disable:scope.row.disable == true}" v-if="!scope.row.statusCd && nowDate"  @click="bidApply(scope.row)">报名</button>
            <button class="button2"  @click="toOpen(scope.row)">下载附件</button>
            <button class="button4" @click="toDetail(scope.row)" v-if="scope.row.statusCd == 'BID_OFFER_STATUS.50' ||scope.row.statusCd == 'BID_OFFER_STATUS.60' || scope.row.statusCd == 'BID_OFFER_STATUS.70'">查看详情</button>
          </div>
        </el-table-column>
      </el-table>
    </div>
    <div class="foot">
      <el-pagination
        background
        @current-change="handleCurrentChange"
        :current-page.sync="pageNum"
        :page-size="pageSize"
        layout="prev, pager, next, jumper"
        :total= "total">
      </el-pagination>
    </div>
  </div>
</div>
</template>

<script>
  import {mapState} from "vuex";
  import Offer from "../../components/person/offer";
  import Download from "../../components/person/download";
  export default {
    components: {Download, Offer},
    layout:'person',
    name: "bid",
    data(){
      return{
        offer: false,
        download1: false,
        labelPosition: 'left',
        list:[],
        pageNum: 1,
        pageSize: 10,
        total: 0, //当前页码
        applyInfo:{},
        nowDate:true,
        value1: '',
        value: '',
        value2:'',
        bidNm:'',
        bidList:[],//投标管理列表
        bidData:{
          orgEnterId:'',
          identityCd:'',
          statusCd:'',
        },
        bidApplyInfo:{
          orgId: "5024282848367616",
          orgNm: "中创海洋科技股份有限公司",
          bidId: "5031217130673152",
          bidNm: "测试一个招标项目",
          rmks: "",
          attachment: "https://sinovat.oss-cn-shanghai.aliyuncs.com/c6178ce400fc4c048a9b4328cc92a878.pdf"
        },
      }
    },
      computed:{
        ...mapState([
          'bWidth',
          'width',
          "currentRole",
          'loading',
          'tel',
          'msgNum'
        ])
      },
    watch:{
      currentRole(){
        this.bidData.orgEnterId = this.currentRole.id;
        this.bidData.identityCd = this.currentRole.identityCd
      },
      offer(){
        this.getBidData()
      },

    },
    mounted() {
      if(this.currentRole){
        this.bidData.orgEnterId = this.currentRole.id;
        this.bidData.identityCd = this.currentRole.identityCd
      }
      this.getBidData()
      this.getData()

    },
    methods:{
      async getData(){
        this.list=await this.api.dataDictionary('BID_OFFER_STATUS')
      },
        back(){
          this.$router.go(-1)
        },
        indexMethod(index) {
          return index +1;
        },
      toDetail(row){
        this.$router.push({
          path:'./bidDetail',
          query:{
            bidId:row.bidId,
            orgId:row.orgId,
            statusCd:row.statusCd,
            statusNm:row.statusNm,
            publishTm:row.publishTm,
            completeTm:row.completeTm,
          }
        })
      },
      handleCurrentChange(val) {
        this.pageNum = val
        this.getBidData()
      },
      status(row){
          // console.log('row',row)
          if(row.row.statusCd ==''&&row.columnIndex =='4'){
            //未报名
            return 'color:#FF3C00;'
          }else if(row.row.statusCd == 'BID_OFFER_STATUS.20'&&row.columnIndex =='4'){
            //已报名
            return 'color:#333333;'
          }else if(row.row.statusCd == 'BID_OFFER_STATUS.50'&&row.columnIndex =='4' ||row.row.statusCd == 'BID_OFFER_STATUS.40'&&row.column =='4'){
            //已报价或待报价
            return 'color:#2778BE;'
          }else if(row.row.statusCd == 'BID_OFFER_STATUS.60'&&row.columnIndex =='4'){
            //已中标
            return 'color:#21AE2B;'
          }else if(row.row.statusCd == 'BID_OFFER_STATUS.70'&&row.columnIndex =='4'){
            //未中标
            return 'color:#FF3E3E;'
          }

      },
        openOffer(row){
          if(row.disable == true){
            this.$message({
              message: '已经过了截止时间',
              type: 'warning',
              duration: '1500',
              offset: '50'
            });
            return
          }else{
            this.offer = true
            this.applyInfo = row
          }

        },
        bidApply(row){
          // console.log(row)
          if(this.until.TimeStep2(row.completeTm+' 23:59:59') >= 0){
            this.$message({
              message: '已经过了截止时间',
              type: 'warning',
              duration: '1500',
              offset: '50'
            });
            return
          }
          this.bidApplyInfo.orgId = this.currentRole.id;
          this.bidApplyInfo.orgNm = this.currentRole.company;
          this.bidApplyInfo.bidId = row.id;
          this.bidApplyInfo.bidNm = row.nm;
          this.bidApplyInfo.attachment = row.attachment;
          this.api.bidApply(this.bidApplyInfo).then(res=> {
            this.$message({
              message: '报名成功',
              type: 'success',
              duration: '1500',
              offset: '50'
            });
            this.getBidData()
          })
        },
        close(){
          this.offer = false;
          this.getBidData()
        },
        toOpen(row){
          this.download1 = true;
          this.applyInfo = row
        },
        select(){
          this.bidData.statusCd =this.value
          this.pageNum = 1
          this.getBidData()
          // console.log('123',this.value,this.bidData.statusCd)
          },
        toClose(){
          this.download1 = false;
        },
        search(){
          this.pageNum = 1
          this.getBidData()
        },
        async getBidData(){
          let qry = this.query.new()
           this.query.toO(qry,'publishTm','desc')
          if(this.value1){
            let timeS = this.until.formatDate(this.value1)
            this.value2 = timeS.year+'-'+timeS.month+'-'+timeS.day
          }else{
            this.value2 = ''
          }
          this.query.toP(qry,this.pageNum,this.pageSize)
          this.query.toW(qry,'nm',this.bidNm,'LK')
          this.query.toW(qry,'publishTm',this.value2,'LK')
          let data = await this.api.bidManage(this.query.toEncode(qry),this.bidData)
          this.bidList = data.data.list
          this.bidList.forEach(item =>{
            item.publishTm = item.publishTm.split(' ')[0];
            item.completeTm = item.completeTm.split(' ')[0];
            if(!item.statusNm){
              item.statusNm = '待报名'
            }
          })
          this.total = data.page.total

          //根据报名时间添加disable属性
          for(var i=0; i<this.bidList.length;i++){ // 添加disable属性并设置为false
            this.$set(this.bidList[i], 'disable', 'false')
          }
          this.bidList.forEach(item => {
            if(this.until.TimeStep2(item.completeTm+' 23:59:59') >= 0){
              item.disable = true;
            }
          });

          console.log('bidlist',this.bidList)

        },
      },
    }
</script>
<style lang="less">
.search{
  .el-input__inner{
    height: 38px;
    line-height: 38px;
  }
}
.table{
  .btnList{
    display: flex;
    justify-content: space-around;
    flex-direction: column;
    .button3{
      width: 74px;
      height: 31px;
      background-color: #2778BE;
      color: #FFFFFF;
      border: none;
      border-radius: 3px;
      margin-bottom: 15px;
      cursor: pointer;
    }
    .disable{
      background-color: #999999;
    }
    .button4{
      border: none;
      width: 74px;
      height: 31px;
      color: #2778BE;
      font-size: 15px;
      background-color: #FFFFFF;
      margin-top: 5px;
      border-radius: 3px;
      cursor: pointer;
    }
    .button2{
      width: 74px;
      height: 31px;
      background-color: #FFFFFF;
      color: #2778BE;
      border-radius: 3px;
      border: 1px solid #2778BE;
      cursor: pointer;

    }
  }
  .el-table__header thead{
    color: #666666;
    font-size:14px;
  }
  .el-table__header th{
    background-color: #F7F7F7;

  }
  .el-table__body tr{
    color: #333333;
    font-size:15px;
    height: 117px;
  }
  .el-table__fixed-right{
    height: 100%!important;
  }
}
</style>
<style lang="less" scoped>
  @import url("../../assets/css/init.less");
#app{
  .header{
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
      /*font-family:Microsoft YaHei;*/
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
      /*font-family:Microsoft YaHei;*/
      font-weight:400;
      color: #666666;
      cursor: pointer;
    }
  }
  .body{
    .search{
      height: 78px;
      display: flex;
      align-items: center;
      border-bottom: 1px solid #DFDFDF;
      padding: 0 19px 0 26px;
      >span{
        font-size: 14px;
        color: #333;
        width: 60px;
        &:nth-of-type(2){
          margin-left: 30px;
        }
      }
      .select{
        margin-right: 30px;
      }
      .shipNm{
        display: flex;
        position: relative;
        height: 38px;
        width: 30.6%;
        background:rgba(255,255,255,1);
        border-radius:5px;
        border:1px solid rgba(223, 223, 223, 1);
        border-right: none;
        padding-left: 12px;
        cursor: pointer;
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
          cursor: pointer;
          .icon-ziyuan{
            color: #ffffff;
          }
        }
      }
    }
    .table{

    }
  }
}
  .foot{
    display: flex;
    width: 100%;
    justify-content: center;
  }
</style>
