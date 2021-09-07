<template>
  <div class="left1">
    <offer  :applyInfo="applyInfo" :offer="offer" @close="close"></offer>
    <download :download1="download1" @toClose="toClose" :applyInfo="applyInfo"></download>
    <!--投标管理-->
    <div class="message" >
      <div class="title">
        <p><img src="@/assets/img/personal/首页/投标管理.png" alt=""/>投标管理<span class="more" @click="toPage('./bid')">查看更多<i class="el-icon-arrow-right"></i></span></p>
      </div>
      <div class="content1">
        <el-table
          :data="bidList"
          :cell-style="status"
          style="width: 100%">
          <el-table-column
            type="index"
            :index="indexMethod"
            align="center"
            width="60"
            label="序号">
          </el-table-column>
          <el-table-column
            prop="nm"
            width="200"
            label="项目名称">
          </el-table-column>
          <el-table-column
            width="110"
            prop="publishTm"
            align="center"
            label="发布时间">
          </el-table-column>
          <el-table-column
            width="110"
            prop="completeTm"
            align="center"
            label="结束时间">
          </el-table-column>
          <el-table-column
            prop="statusNm"
            width = "85"
            align="center"
            label="状态">
          </el-table-column>
          <el-table-column
            align="center"
            width="90"
            fixed="right"
            prop="operations"
            label="操作">
            <div class="btnList" slot-scope="scope">
              <button class="button3"  :class="{disable:scope.row.disable == true}" v-if="scope.row.statusCd == 'BID_OFFER_STATUS.40'" @click="openOffer(scope.row)">报价</button>
              <button class="button3"  :class="{disable:scope.row.disable == true}" v-if="!scope.row.statusCd" @click="bidApply(scope.row)">报名</button>
              <button class="button2"  @click="toOpen(scope.row)">下载附件</button>
              <button class="button4" @click="toDetail(scope.row)" v-if="scope.row.statusCd == 'BID_OFFER_STATUS.50' ||scope.row.statusCd == 'BID_OFFER_STATUS.60' || scope.row.statusCd == 'BID_OFFER_STATUS.70'">查看详情</button>
            </div>
          </el-table-column>
        </el-table>
      </div>
    </div>
    <!--管理船舶-->
    <div class="manage">
      <div class="title">
        <p><img src="@/assets/img/personal/首页/管控船舶.png" alt=""/>管控船舶<span class="more" @click="toPage('./shipControl')">查看更多<i class="el-icon-arrow-right"></i></span></p>
      </div>
      <div class="content1">
        <el-table
          :data="manageList"
          style="width: 100%">
          <el-table-column
            prop="cd"
            width="160"
            label="船舶编号">
          </el-table-column>
          <el-table-column
            prop="nm"
            width="200"
            label="船舶名称">
          </el-table-column>
          <el-table-column
            width="100"
            prop="productName"
            align="center"
            label="客户名称">
          </el-table-column>
          <el-table-column
            prop="statusNm"
            width = "85"
            align="center"
            label="船舶状态">
          </el-table-column>
          <el-table-column
            align="center"
            width="90"
            fixed="right"
            prop="operations"
            label="操作">
              <template slot-scope="scope">
                <div class="btnList">

                <button class="button3" @click="toDetail2(scope.row,'1')" v-if="scope.row.statusNm!='已交船'">新增</button>
                <button class="button2" @click="toDetail2(scope.row,'2')">查看详情</button>
                </div>

              </template>
          </el-table-column>
        </el-table>
      </div>
    </div>
  </div>
</template>

<script>
  import {mapState} from "vuex";

  import Offer from "@/components/person/offer";
  import Download from "@/components/person/download";
  export default {
    name: "shipowner",
    components: {
      Download,
      Offer },
    data(){
      return {

        offer: false,
        download1: false,
        applyInfo:{},
        manageList: [{
          no: 'ZCHY20200515',
          nm: '新型 4000KW 中型休闲...',
          cl: '中创海洋',
          state: '建造中',
        }, {
          no: 'ZCHY20200515',
          nm: '新型 4000KW 中型休闲...',
          cl: '中创海洋',
          state: '建造中',
        }, {
          no: 'ZCHY20200515',
          nm: '新型 4000KW 中型休闲...',
          cl: '中创海洋',
          state: '已完成',
        }],
        bidApplyInfo:{
          orgId: "5024282848367616",
          orgNm: "中创海洋科技股份有限公司",
          bidId: "5031217130673152",
          bidNm: "测试一个招标项目",
          rmks: "",
          attachment: "https://sinovat.oss-cn-shanghai.aliyuncs.com/c6178ce400fc4c048a9b4328cc92a878.pdf"
        },
        bidList:[],//投标管理列表
        adertList:[],
        bidData:{
          orgEnterId:'',
          identityCd:'',
        }
      }

    },
    computed:{
      ...mapState([
        "currentRole",
      ])
    },
    watch:{
      currentRole(){
        this.getBidData()
      },
    },
    mounted(){
      if(this.currentRole){
        this.bidData.orgEnterId = this.currentRole.id;
        this.bidData.identityCd = this.currentRole.identityCd
      }
      this.getData()
    },
    methods:{
      async getData(){
        this.getBidData()
        this.getList()
      },
      //投标列表
      async getBidData(){
        this.bidData.orgEnterId = this.currentRole.id;
        this.bidData.identityCd = this.currentRole.identityCd
        let qry = this.query.new()
        this.query.toP(qry,'1','3')
        this.query.toO(qry,'publishTm','desc')
        let data = await this.api.bidManage(this.query.toEncode(qry),this.bidData)
        this.bidList = data.data.list
        this.bidList.forEach(item =>{
          item.publishTm = item.publishTm.split(' ')[0];
          item.completeTm = item.completeTm.split(' ')[0];
          if(!item.statusNm){
            item.statusNm = '待报名'
          }
        })

        //根据时间添加disable属性
        for(var i=0; i<this.bidList.length;i++){ // 添加disable属性并设置为false
          this.$set(this.bidList[i], 'disable', 'false')
        }
        this.bidList.forEach(item => {
          if(this.until.TimeStep(item.completeTm) >= 0){
            item.disable = true;
          }
        });
      },
      //管控船舶列表
      async getList(){
        let qry = this.query.new()
        this.query.toP(qry,1,3)

        let param={
          orgEnterId:this.currentRole.id
        }
        let data=await this.api.allShip(this.query.toEncode(qry),param)
        this.manageList=data.data.list
      },
      toPage(url){
        if(url){
          this.$router.push(url)
        }
      },
      status(row){
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
      //投标详情
      toDetail(row){
        console.log(row,'row')
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
      //管控船舶详情
      toDetail2(item,type){
        if(type=='1'){
          this.$router.push('./shipFileDetail?id='+item.id+"&cdType=3&formW=jianli&cdType2=2&shipCd="+item.cd+"&shipStatus="+item.statusNm)
        }else if(type=='2'){
          this.$router.push('./archives?id='+item.id+"&shipCd="+item.cd+"&shipStatus="+item.statusNm)
        }
      },
      indexMethod(index) {
        return index +1;
      },
      //报价弹窗打开
      openOffer(row){
        if(row.disable == true){
          this.$message({
            message: '已经过了截止时间',
            type: 'warning',
            duration: '1500',
            offset: '50'
          });
          return;
        }else{
          this.offer = true
          this.applyInfo = row
        }
      },

      //报名申请
      bidApply(row){
        console.log('111',row)
        if(this.until.TimeStep(row.completeTm) >= 0){
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
        // this.bidApplyInfo.rmks = row.rmks;
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
      //关闭报价弹窗
      close(data){
        this.offer = false;
        if(data=='submit'){
          this.getBidData()
        }
      },
      //附件下载弹窗打开
      toOpen(row){
        this.download1 = true;
        this.applyInfo = row
      },
      //附件下载 关闭
      toClose(){
        this.download1 = false;
      },
      //报价关闭
      submit() {
        // 确认弹窗回调
        this.show = false
        this.getBidData()
      },

    },
  }
</script>

<style lang="less">
  .content1{
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

  .message{
    input::-webkit-input-placeholder {
      /* Chrome/Opera/Safari */
      color: #999;
    }

    input::-moz-placeholder {
      /* Firefox 19+ */
      color: #999;
    }

    input:-ms-input-placeholder {
      /* IE 10+ */
      color: #999;
    }

    input:-moz-placeholder {
      /* Firefox 18- */
      color: #999;
    }
    textarea::-webkit-input-placeholder {
      /* Chrome/Opera/Safari */
      color: #999;
    }

    textarea::-moz-placeholder {
      /* Firefox 19+ */
      color: #999;
    }

    textarea:-ms-input-placeholder {
      /* IE 10+ */
      color: #999;
    }

    textarea:-moz-placeholder {
      /* Firefox 18- */
      color: #999;
    }
  }
    #resize{
      resize: none !important;
      height: 92px;
    }
    #btnBA{
      display: inline-flex;
      width: 100%;
      .el-form-item__content{
        display: inline-flex;
        justify-content: center;
        width: 100%;
      }
      #buttonA{
        width: 46%;
        height: 42px;
        border: 1px solid #2778BE;
        color: #FFFFFF;
        font-size: 16px;
        background-color: #2778BE;
        cursor: pointer;
        margin-left: 30px;
      }
      #buttonB{
        width: 46%;
        height: 42px;
        border: 1px solid #DDDDDD;
        color: #666666;
        font-size: 16px;
        background-color: #FFFFFF;
        cursor: pointer;
      }
    }

</style>
<style lang="less" scoped>
  @import url("../../assets/css/init.less");
  .left1{
    width: 67.3%;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    padding-top: 31px;

    .title{
      width: 100%;
      height: 56px;
      border-bottom: 1px solid #F3F3F3;
      .more{
        float: right;
        font-size: 12px;
        color: #666666;
        margin-right: 30px;
        cursor: pointer;

      }
      >p{
        font-size: 16px;
        color: #333333;
        line-height: 56px;
        margin-left: 20px;
      }
      img{
        padding-right: 10px;
        height: 19px;
        vertical-align: sub;
      }
    }
    //投标管理
    .message{
      width: 100%;
      background-color: #FFFFFF;
      min-height: 445px;
    }
    /*监理监造机构  管理船舶*/
    .manage{
      width: 100%;
      background-color: #FFFFFF;
      margin-top: 24px;
      min-height: 434px;
    }
  }
</style>
