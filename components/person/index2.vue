<template>
  <div class="left1">
    <!--报价-->
    <offer  :applyInfo="applyInfo" :offer="offer" @close="close"></offer>
    <!--附件下载-->
    <download :download1="download1" @toClose="toClose" :applyInfo="applyInfo"></download>
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
        <div class="section"  @click="toPage('./myOrder2?cdType=1&statusCd=2')">
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
        <div class="section"  @click="toPage('./myOrder2?cdType=1')">
          <div class="partA">
            <img src="@/assets/img/personal/首页/全部订单.png" alt=""/>
            <span v-if="this.number != 0"  class="span">{{number}}</span>
          </div>
          <p>全部订单</p>
        </div>
      </div>
    </div>
    <!--设计销售订单-->
    <div class="shipOrder">
      <div class="title">
        <p><img src="@/assets/img/personal/首页/产品订单.png" alt=""/>设计销售订单</p>
      </div>
      <div class="content">
        <div class="section"  @click="toPage('./myOrder2?cdType=21&statusCd=1')">
          <div class="partA">
            <img src="@/assets/img/personal/首页/待付款.png" alt=""/>
            <span v-if="this.number != 0"  class="span">{{number}}</span>
          </div>
          <p>待付款</p>
        </div>
        <div class="section" @click="toPage('./myOrder2?cdType=21&statusCd=2')">
          <div class="partA">
            <!--图片需更换-->
            <img src="@/assets/img/personal/首页/设计中.png" alt=""/>
            <span v-if="this.number != 0" class="span">{{number}}</span>
          </div>
          <p>进行中</p>
        </div>
        <div class="section"  @click="toPage('./myOrder2?cdType=21&statusCd=4')">
          <div class="partA">
            <img src="@/assets/img/personal/首页/已完成.png" alt=""/>
            <span v-if="this.number != 0" class="span">{{number}}</span>
          </div>
          <p>已完成</p>
        </div>
        <div class="section"  @click="toPage('./myOrder2?cdType=21')">
          <div class="partA">
            <img src="@/assets/img/personal/首页/全部订单.png" alt=""/>
            <span v-if="this.number != 0"  class="span">{{number}}</span>
          </div>
          <p>全部订单</p>
        </div>
      </div>
    </div>
    <!--设计采购订单-->
    <div class="shipOrder">
      <div class="title">
        <p><img src="@/assets/img/personal/首页/产品订单.png" alt=""/>设计采购订单</p>
      </div>
      <div class="content">
        <div class="section"  @click="toPage('./myOrder2?cdType=22&statusCd=1')">
          <div class="partA">
            <img src="@/assets/img/personal/首页/待付款.png" alt=""/>
            <span v-if="this.number != 0"  class="span">{{number}}</span>
          </div>
          <p>待付款</p>
        </div>
        <div class="section" @click="toPage('./myOrder2?cdType=22&statusCd=2')">
          <div class="partA">
            <!--图片需更换-->
            <img src="@/assets/img/personal/首页/设计中.png" alt=""/>
            <span v-if="this.number != 0" class="span">{{number}}</span>
          </div>
          <p>进行中</p>
        </div>
        <div class="section"  @click="toPage('./myOrder2?cdType=22&statusCd=4')">
          <div class="partA">
            <img src="@/assets/img/personal/首页/已完成.png" alt=""/>
            <span v-if="this.number != 0" class="span">{{number}}</span>
          </div>
          <p>已完成</p>
        </div>
        <div class="section"  @click="toPage('./myOrder2?cdType=22')">
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
              <button class="button2"  @click="toOpen(scope.row)">下载附件</button>
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
      Offer, },
    data(){
      return {
        number: 0,
        offer: false,
        download1: false,
        applyInfo:{},

        bidApplyInfo:{  //报名参数
          orgId: "5024282848367616",
          orgNm: "中创海洋科技股份有限公司",
          bidId: "5031217130673152",
          bidNm: "测试一个招标项目",
          rmks: "",
          attachment: "https://sinovat.oss-cn-shanghai.aliyuncs.com/c6178ce400fc4c048a9b4328cc92a878.pdf"
        },
        bidList:[],//投标管理列表
        bidData:{  //投标管理列表请求参数
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

      this.getBidData()
    },
    methods:{

      //投标列表
      async getBidData(){
        this.bidData.orgEnterId = this.currentRole.id;
        this.bidData.identityCd = this.currentRole.identityCd
        let qry = this.query.new()
        this.query.toP(qry,'1','1')
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


        // this.bidList.splice(3)
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
      download(row){
        console.log(row)

        let url = `/general/oss/aliDownload?urls=${row.attachment}&zipName=${row.nm}`
        window.open(url)
      },
      //投标报名
      bidApply(row){
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
      //附件下载关闭
      toClose(){
        this.download1 = false;
      },

    },
  }
</script>

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
          width: 20%;
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
    //船舶订单
    .shipOrder{
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
          height: 21px;
          vertical-align: middle;
        }
      }
      .content{
        width: 100%;
        display: inline-flex;
        justify-content: space-between;
        .section{
          width: 20%;
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
              color: #FFFFFF;
              left: 26px;
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
    /*我的产品*/
    .myProduct{
      width: 100%;
      background-color: #FFFFFF;
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
        .sectionA{
          width: 25%;
          display: flex;
          flex-direction: column;
          justify-content: center;
          align-items: center;
          height: 162px;
          >img{
            width: 84px;
          }
          >p{
            margin-top: 16px;
            font-size: 16px;
            color: #666666;
            cursor: pointer;
          }
        }
      }
    }
    /*经销商 客户档案*/
    .document{
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

      }
    }
    /*经销商 收款记录 */
    .money{
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

      }
    }
    /*跟进记录*/
    .record{
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
      .content2{
      }
    }
    //在线留言板块
    .message1{
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
        padding: 20px 110px 30px 80px;
      }
    }
    //在线留言板块
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
        min-height: 180px;
        .format{
          width: 100%;
          display: inline-flex;
          justify-content: center;
          flex-direction: row;
          align-items: baseline;
          text-align: center;
          cursor: pointer;
          >p{
            font-size: 15px;
            color: #666666;
            width: 93px;
          }
          input{
            width: 348px;
            height: 42px;
            border: 1px solid #DDDDDD;
            margin-top: 20px;
            padding-left: 21px;
            color: #999999;
          }
          textarea{
            width: 348px;
            height: 92px;
            border: 1px solid #DDDDDD;
            margin-top: 20px;
            resize: none;
            padding: 21px;
            color: #999999;
          }
        }
        .button1{
          margin-top: 30px;
          width: 100%;
          padding-left: 92px;
          display: inline-flex;
          justify-content: center;
          padding-bottom: 30px;
          button:nth-child(1){
            width: 159px;
            height: 42px;
            border: 1px solid #DDDDDD;
            color: #666666;
            font-size: 16px;
            background-color: #FFFFFF;
            cursor: pointer;
          }
          button:nth-child(2){
            width: 159px;
            height: 42px;
            border: 1px solid #2778BE;
            color: #FFFFFF;
            font-size: 16px;
            background-color: #2778BE;
            margin-left: 31px;
            cursor: pointer;
          }
        }
        .disable{
          background-color: #999999;
        }
      }
    }
    /*监理监造机构  管理船舶*/
    .manage{
      width: 100%;
      background-color: #FFFFFF;
      margin-top: 24px;
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

      }
    }
  }


</style>
