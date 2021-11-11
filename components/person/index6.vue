<template>
  <div class="left1">
    <offer  :applyInfo="applyInfo" v-if="offer" @close="close"></offer>
    <download :download1="download1" @toClose="toClose" :applyInfo="applyInfo"></download>
    <!--销售订单-->
    <div class="productOrder">
      <div class="title">
        <p><img src="@/assets/img/personal/首页/销售订单.png" alt=""/>销售订单</p>
      </div>
      <div class="content">
        <div class="section"  @click="toPage('./myOrder3?statusCd=1')">
          <div class="partA">
            <img src="@/assets/img/personal/首页/待付款.png" alt=""/>
            <span v-if="number != 0" class="span">{{number}}</span>
          </div>
          <p>待付款</p>
        </div>
        <div class="section" @click="toPage('./myOrder3?statusCd=2')">
          <div class="partA">
            <img src="@/assets/img/personal/首页/待发货.png" alt=""/>
            <span v-if="number != 0" class="span">{{number}}</span>
          </div>
          <p>待发货</p>
        </div>
        <div class="section" @click="toPage('./myOrder3?statusCd=3')">
          <div class="partA">
            <img src="@/assets/img/personal/首页/待收货.png" alt=""/>
            <span v-if="number != 0"  class="span">{{number}}</span>
          </div>
          <p>待收货</p>
        </div>
        <div class="section" @click="toPage('./myOrder3?statusCd=4')">
          <div class="partA">
            <img src="@/assets/img/personal/首页/已完成.png" alt=""/>
            <span v-if="number != 0"  class="span">{{number}}</span>
          </div>
          <p>已完成</p>
        </div>
        <div class="section"  @click="toPage('./myOrder3?statusCd=0')">
          <div class="partA">
            <img src="@/assets/img/personal/首页/全部订单.png" alt=""/>
            <span v-if="number != 0"  class="span">{{number}}</span>
          </div>
          <p>全部订单</p>
        </div>
      </div>
    </div>
    <!--产品订单-->
    <div class="productOrder">
      <div class="title">
        <p><img src="@/assets/img/personal/首页/产品订单.png" alt=""/>产品订单</p>
      </div>
      <div class="content">
        <div class="section" @click="toPage('./myOrder1?cdType=1&statusCd=1')">
          <div class="partA">
            <img src="@/assets/img/personal/首页/待付款.png" alt=""/>
            <span v-if="number != 0" class="span">{{number}}</span>
          </div>
          <p>待付款</p>
        </div>
        <div class="section" @click="toPage('./myOrder1?cdType=1&statusCd=2')">
          <div class="partA">
            <img src="@/assets/img/personal/首页/待发货.png" alt=""/>
            <span v-if="number != 0" class="span">{{number}}</span>
          </div>
          <p>待发货</p>
        </div>
        <div class="section" @click="toPage('./myOrder1?cdType=1&statusCd=3')">
          <div class="partA">
            <img src="@/assets/img/personal/首页/待收货.png" alt=""/>
            <span v-if="number != 0"  class="span">{{number}}</span>
          </div>
          <p>待收货</p>
        </div>
        <div class="section" @click="toPage('./myOrder1?cdType=1&statusCd=4')">
          <div class="partA">
            <img src="@/assets/img/personal/首页/已完成.png" alt=""/>
            <span v-if="number != 0"  class="span">{{number}}</span>
          </div>
          <p>已完成</p>
        </div>
        <div class="section" @click="toPage('./myOrder1?cdType=1')">
          <div class="partA">
            <img src="@/assets/img/personal/首页/全部订单.png" alt=""/>
            <span v-if="number != 0"  class="span">{{number}}</span>
          </div>
          <p>全部订单</p>
        </div>
      </div>
    </div>
    <!--船舶订单-->
    <div class="productOrder">
      <div class="title">
        <p><img src="@/assets/img/personal/首页/船舶订单.png" alt=""/>船舶订单</p>
      </div>
      <div class="content">
        <div class="section" @click="toPage('./myOrder1?cdType=2&statusCd=1')">
          <div class="partA">
            <img src="@/assets/img/personal/首页/待付款.png" alt=""/>
            <span v-if="number != 0"  class="span">{{number}}</span>
          </div>
          <p>待付款</p>
        </div>
        <div class="section" @click="toPage('./myOrder1?cdType=2&statusCd=2')">
          <div class="partA">
            <img src="@/assets/img/personal/首页/待发货.png" alt=""/>
            <span v-if="number != 0" class="span">{{number}}</span>
          </div>
          <p>待发货</p>
        </div>
        <div class="section" @click="toPage('./myOrder1?cdType=2&statusCd=3')">
          <div class="partA">
            <img src="@/assets/img/personal/首页/待收货.png" alt=""/>
            <span v-if="number != 0" class="span">{{number}}</span>
          </div>
          <p>待收货</p>
        </div>
        <div class="section" @click="toPage('./myOrder1?cdType=2&statusCd=4')">
          <div class="partA">
            <img src="@/assets/img/personal/首页/已完成.png" alt=""/>
            <span v-if="number != 0" class="span">{{number}}</span>
          </div>
          <p>已完成</p>
        </div>
        <div class="section"  @click="toPage('./myOrder1?cdType=2')">
          <div class="partA">
            <img src="@/assets/img/personal/首页/全部订单.png" alt=""/>
            <span v-if="this.number != 0"  class="span">{{number}}</span>
          </div>
          <p>全部订单</p>
        </div>
      </div>
    </div>
    <!--我的产品-->
    <div class="myProduct">
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
  </div>
</template>

<script>
  import {mapState} from "vuex";
  import Offer from "@/components/person/offer";
  import Download from "@/components/person/download";
  import img1 from '@/assets/img/alter.png'

  export default {
    name: "shipowner",
    components: {Offer,Download
    },
    data(){
      return {
        number: 0,
        download1: false,
        offer:false,
        applyInfo:{},
        bidList:[],
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

    },
    mounted(){

      this.getData()
    },
    methods:{
      async getData(){
        this.getBidData()
      },
      //投标列表
      async getBidData(){
        this.bidData.orgEnterId = this.currentRole.id;
        this.bidData.identityCd = this.currentRole.identityCd
        let qry = this.query.new()
        this.query.toP(qry,'1','2')
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
      toPage(url){
        if(url){
          this.$router.push(url)
        }
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
          span{
            float: right;
            cursor: pointer;
          }
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






  }



</style>
