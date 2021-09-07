<template>
  <div class="left1">
    <!--采购订单-->
    <div class="productOrder">
      <div class="title">
        <p><img src="@/assets/img/personal/首页/产品订单.png" alt=""/>采购订单</p>
      </div>
      <div class="content">
        <div class="section" @click="toPage('./myOrder2?cdType=1&statusCd=1')">
          <div class="partA">
            <img src="@/assets/img/personal/首页/待付款.png" alt=""/>
            <span v-if="this.number != 0" class="span">{{number}}</span>
          </div>
          <p>待付款</p>
        </div>
        <div class="section" @click="toPage('./myOrder2?cdType=1&statusCd=2')">
          <div class="partA">
            <img src="@/assets/img/personal/首页/待发货.png" alt=""/>
            <span v-if="this.number != 0" class="span">{{number}}</span>
          </div>
          <p>待发货</p>
        </div>
        <div class="section" @click="toPage('./myOrder2?cdType=1&statusCd=3')">
          <div class="partA">
            <img src="@/assets/img/personal/首页/待收货.png" alt=""/>
            <span v-if="this.number != 0"  class="span">{{number}}</span>
          </div>
          <p>待收货</p>
        </div>
        <div class="section" @click="toPage('./myOrder2?cdType=1&statusCd=4')">
          <div class="partA">
            <img src="@/assets/img/personal/首页/已完成.png" alt=""/>
            <span v-if="this.number != 0"  class="span">{{number}}</span>
          </div>
          <p>已完成</p>
        </div>
        <div class="section" @click="toPage('./myOrder2?cdType=1')">
          <div class="partA">
            <img src="@/assets/img/personal/首页/全部订单.png" alt=""/>
            <span v-if="this.number != 0"  class="span">{{number}}</span>
          </div>
          <p>全部订单</p>
        </div>
      </div>
    </div>
    <!--设计订单-->
    <div class="productOrder">
      <div class="title">
        <p><img src="@/assets/img/personal/首页/产品订单.png" alt=""/>建造订单</p>
      </div>
      <div class="content">
        <div class="section" @click="toPage('./myOrder2?cdType=3&statusCd=1')">
          <div class="partA">
            <img src="@/assets/img/personal/首页/待付款.png" alt=""/>
            <span v-if="this.number != 0"  class="span">{{number}}</span>
          </div>
          <p>待付款</p>
        </div>
        <div class="section" @click="toPage('./myOrder2?cdType=3&statusCd=2')">
          <div class="partA">
            <!--图片需更换-->
            <img src="@/assets/img/personal/首页/建造中.png" alt=""/>
            <span v-if="this.number != 0" class="span">{{number}}</span>
          </div>
          <p>建造中</p>
        </div>
        <div class="section" @click="toPage('./myOrder2?cdType=3&statusCd=4')">
          <div class="partA">
            <img src="@/assets/img/personal/首页/已完成.png" alt=""/>
            <span v-if="this.number != 0" class="span">{{number}}</span>
          </div>
          <p>已完成</p>
        </div>
        <div class="section" @click="toPage('./myOrder2?cdType=3')">
          <div class="partA">
            <img src="@/assets/img/personal/首页/全部订单.png" alt=""/>
            <span v-if="this.number != 0"  class="span">{{number}}</span>
          </div>
          <p>全部订单</p>
        </div>
      </div>
    </div>
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
              <button class="button3" :class="{disable:scope.row.disable == true}" v-if="scope.row.statusCd == 'BID_OFFER_STATUS.40'" @click="openOffer(scope.row)">报价</button>
              <button class="button3" :class="{disable:scope.row.disable == true}" v-if="!scope.row.statusCd" @click="bidApply(scope.row)">报名</button>
              <button class="button2" @click="toOpen(scope.row)">下载附件</button>
              <button class="button4" @click="toDetail(scope.row)" v-if="scope.row.statusCd == 'BID_OFFER_STATUS.50' ||scope.row.statusCd == 'BID_OFFER_STATUS.60' || scope.row.statusCd == 'BID_OFFER_STATUS.70'">查看详情</button>
            </div>
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
        number: 0,
        offer: false,
        download1: false,
        applyInfo:{},
        bidList:[],//投标管理列表
        bidData:{
          orgEnterId:'',
          identityCd:'',
        }
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
      },
      //获取投标
      async getBidData(){
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

        //根据报名时间添加disable属性
        for(var i=0; i<this.bidList.length;i++){ // 添加disable属性并设置为false
          this.$set(this.bidList[i], 'disable', 'false')
        }
        this.bidList.forEach(item => {
          if(this.until.TimeStep(item.completeTm) >= 0){
            item.disable = true;
          }
        });

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
      indexMethod(index) {
        return index +1;
      },
      //报价弹窗
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

      //投标报名
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
      close(data){
        this.offer = false;
        if(data=='submit'){
          this.getBidData()
        }
      },
      //下载弹窗
      toOpen(row){
        this.download1 = true;
        this.applyInfo = row
      },
      //关闭下载
      toClose(){
        this.download1 = false;
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
    //产品订单
    .productOrder{
      width: 100%;
      /*height: 200px;*/
      background-color: #FFFFFF;
      margin-bottom: 24px;
      .title{
        width: 100%;
        height: 56px;
        border-bottom: 1px solid #F3F3F3;
        >p{
          font-size: 16px;
          color: #333333;
          line-height: 56px;
          margin-left: 20px;

        }
        img{
          padding-right: 10px;
          height: 19px;
          vertical-align: middle;
        }
      }
      //内容
      .content{
        width: 100%;
        display: inline-flex;
        .section{
          flex: 1;
          display: flex;
          flex-direction: column;
          justify-content: center;
          align-items: center;
          height: 162px;
          .partA{
            position: relative;
            .span{
              position: absolute;
              width: 14px;
              height: 14px;
              background-color: #E4393C;
              border-radius: 50%;
              left: 26px;
              color: #FFFFFF;
              font-size: 12px;
              text-align: center;
              line-height: 14px;
            }
            >img{
              cursor: pointer;
              width: 35px;
              padding-bottom: 10px;
            }
          }
          >p{
            font-size: 12px;
            color: #333333;
            cursor: pointer;
          }
        }
      }
    }

    //投标管理
    .message{
      width: 100%;
      background-color: #FFFFFF;
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
      .content1{
        height: 100%;
        min-height: 413px;
      }
    }

  }

</style>
