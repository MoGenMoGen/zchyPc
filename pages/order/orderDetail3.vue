<template>
  <!-- 订单详情(供应商) -->
  <div id="home">
      <div class="cont">
        <div class="line1">
          <span class="point" @click="toPage('/')">首页&nbsp;&nbsp;>&nbsp;&nbsp;</span>
          <span class="point" @click="toPage('../personal/')">个人中心&nbsp;&nbsp;>&nbsp;&nbsp;</span>
          <span class="point" @click="toPage('../personal/myOrder3')">销售订单&nbsp;&nbsp;>&nbsp;&nbsp;</span>
          <span class="red">订单详情</span>
        </div>

        <div class="line2" style="justify-content: center;">
          <div class="line2-l" style="border-right: none;">
            <p >订单号：{{info.orderId}}</p>
            <p class="red">{{info.statusNm}}</p>
          </div>
          <!-- <div class="line2-r" v-if="form!='order3'">
            <el-steps :active="orderStatus">
              <el-step title="提交订单" icon="el-icon-document-checked" :description="info.crtTm"></el-step>
              <el-step title="付款成功" icon="el-icon-bank-card" :description="info.payTm"></el-step>
              <el-step title="待收货" icon="el-icon-truck" :description="info.logiTm"></el-step>
              <el-step title="交易完成" icon="el-icon-shopping-bag-2" :description="info.signTm"></el-step>
            </el-steps>
          </div> -->
        </div>
        <div class="line4">
          <div>
            <p>收货人信息</p>
            <p><span>收货人：</span><span>{{info.receNm}}</span></p>
            <p><span>收货地址：</span><span>{{info.addrNm}}</span></p>
            <p><span>手机号码：</span><span>{{info.receMob}}</span></p>
          </div>
          <div>
            <p>配送信息</p>
            <p><span>配送方式：</span><span>{{info.logiEntpNm}}</span></p>
            <p><span>运费：</span><span>￥{{fmoney(info.payShip)}}</span></p>
          </div>
          <!-- <div>
            <p>付款信息</p>
            <p><span>付款方式：</span><span>{{info.payType==0?'微信':'支付宝'}}</span></p>
            <p><span>付款时间：</span><span>{{info.payTm}}</span></p>
            <p><span>商品总额：</span><span>￥{{info.orderPrice}}</span></p>
          </div> -->
        </div>
<!--        产品信息-->

        <div class="line5">
          <p class="line5-t">
            <span>产品信息</span>
            <span>产品参数</span>
            <span>单价</span>
            <span>数量</span>
            <span>小计</span>
          </p>
          <div class="item">
             <div   v-for="(item,index) in goodList" :key="index">
               <p @click="toProDetail(item)">
                 <img :src="item.goodsImgUrl.split(',')[0]" >
                 <span>{{item.goodsNm}}</span>
               </p>
               <p>
                  {{item.goodsSkuAttrNm}}
               </p>
                <p v-if="item.unitPrice!=price">{{fmoney(item.unitPrice)}}</p>
               <p v-else>价格面议</p>
               <p>{{item.qty}}</p>
               <p v-if="item.unitPrice!=price">{{fmoney(item.totalPrice)}}</p>
               <p v-else>价格面议</p>
             </div>
          </div>

        </div>
<!--        总计-->
        <div class="line6">
          <p>
            <span></span>
            <span><span class="red">{{Number(num).toFixed(2)}}</span>件商品，总商品金额</span>
            <span v-if="goodList.length && goodList[0].unitPrice!=price">￥{{fmoney(moneySum)}}</span>
            <span v-if="goodList.length && goodList[0].unitPrice===price">价格面议</span>
          </p>
          <!-- <p>
            <span></span>
            <span>运费：</span>
            <span>￥{{info.payShip}}</span>
          </p> -->
          <p>
            <span></span>
            <span>应付总额：</span>
            <span class="red totalAmout"  v-if="goodList.length && goodList[0].unitPrice!=price">￥{{fmoney(moneySum+info.payShip)}}</span>
            <span  class="red totalAmout" v-if="goodList.length && goodList[0].unitPrice===price">价格面议</span>
          </p>
        </div>
<!--        底部边距-->
        <div class="line7">

        </div>
      </div>
  </div>
</template>

<script>

import {mapState} from "vuex";
import steps from "../../components/steps/steps";
export default {
  head () {
    return {
      title: '订单详情',
    }
  },
  data(){
    return{
      form:'',
      orderStatus:0,
      dataShow:false,
      activities: [{
        content: '活动按期开始活动按期开始666666666666666666666666666666666',
        timestamp: '2018-04-15'
      }],
      goodList:[],
      nm:'',
      imgUrl:'',
      id:"",
      info:{},
      num:0,
      moneySum:0,

    }
  },
  computed: {
    ...mapState([
      'bWidth',
      'width',
      'price'
    ])
  },
  components: {
      steps,

  },
  mounted(){
    this.id = this.until.getQueryString('id')
    this.getInfo()

  },
  methods:{
    fmoney(s, n) {
        n = n > 0 && n <= 20 ? n : 2;
        s = parseFloat((s + "").replace(/[^\d\.-]/g, "")).toFixed(n) + "";
        var l = s.split(".")[0].split("").reverse(), r = s.split(".")[1];
        var t = "";
        for (let i = 0; i < l.length; i++) {
            t += l[i] + ((i + 1) % 3 == 0 && (i + 1) != l.length ? "," : "");
        }
        return t.split("").reverse().join("") + "." + r;
    },
   async getInfo(){
     this.info=await this.api.shopBasicDetail(this.id)
        // if(this.info.payTm){
        //   this.orderStatus = 1
        // }
        // if(this.info.logiTm){
        //   this.orderStatus = 2
        // }
        // if(this.info.signTm){
        //   this.orderStatus = 3
        // }
        // this.goodList=this.info.itms
        this.dataShow=true

        if(this.info.status=='10'){
          this.info.statusNm='待支付'
        }else if(this.info.status=='20'){
          this.info.statusNm='待发货'
        }else if(this.info.status=='30'){
          this.info.statusNm='已发货'
        }else if(this.info.status=='40'){
          this.info.statusNm='未签收'
        }else if(this.info.status=='50'){
          this.info.statusNm='自动签收'
        }else if(this.info.status=='60'){
          this.info.statusNm='用户确认收货'
        }else if(this.info.status=='70'){
          this.info.statusNm='自动确认收货'
        }else if(this.info.status=='80'){
          this.info.statusNm='待评价'
        }else if(this.info.status=='90'){
          this.info.statusNm='已完成'
        }else if(this.info.status=='100'){
          this.info.statusNm='取消处理中'
        }else if(this.info.status=='110'){
          this.info.statusNm='取消中'
        }else if(this.info.status=='120'){
          this.info.statusNm='已取消'
        }else if(this.info.status=='160'){
          this.info.statusNm='删除订单'
        }
      console.log('基本信息')
      console.log(this.info)

      let qry = this.query.new()
      this.query.toW(qry,'orderId',this.id,'EQ')
      this.goodList=await this.api.shopProductDetail(this.query.toEncode(qry))
      console.log('产品信息')
      console.log(this.goodList)
      this.goodList.forEach(item=>{
        // item.total=item.goodsPrice*item.qty
        this.num+=item.qty
        this.moneySum+=item.totalPrice
      })






    },

    //页面跳转
    toPage(url){
      this.$router.push(url)
    },

    //跳转产品详情
    toProDetail(item) {
      console.log(item)
      // let url = '../sinovat/Trade/productDetail?id='+item.id + "&type=product"
      // window.open(url)
    },
  }
}
</script>
<style lang="less">


</style>
<style lang="less" scoped>
  @import url("../../assets/css/init.less");
  #home{
    background: #F3F3F3;
    .point{
      cursor: pointer;
    }
    .cont{
      width: 1200px;
      margin: 0 auto;
      .line1{
        height: 52px;
        display: flex;
        align-items: center;
      }
      .line2{
        width: 100%;
        display: flex;
        height: 198px;
        background: white;
        .line2-l{
          width: 325px;
          height: 100%;
          border-right: 1px solid #F1F1F1;
          p{
            margin: 0 auto;
            text-align: center;
            width: fit-content;

            &:nth-child(1){
              color: #999999;
              margin-top: 30px;

            }
            &:nth-child(2){
              font-size: 28px;
              margin-top: 37px;

            }
            &:nth-child(3){
              font-size: 14px;
              color: #666666;
              margin-top: 30px;
              border-radius: 3px;
              border: 1px solid #DDDDDD;
              padding:9px 22px;
              background: #F5F5F5;

            }
          }
        }
        .line2-r{
          width: 874px;
          height: 100%;
          padding:54px 22px 40px 54px;


        }
      }

      .line3{
        margin-top: 20px;
        width: 100%;
        background: white;
        display: flex;
        .line3-l{
          display: flex;
          padding:31px;
          width: 325px;
          border-right: 1px solid #F1F1F1;
          img{
            width: 110px;
            height: 110px;
            margin-right: 20px;
          }
          p{
            margin-top: 13px;

          }
        }
        .line3-r{
          width: 874px;
          display: flex;
          align-items: center;
          padding: 30px 80px;
        }

      }
      .line4{
        margin-top: 20px;
        width: 100%;
        padding: 30px 0;
        display: flex;
        background: white;
        div{
          padding: 0 30px;
          flex:1;
          border-right: 1px solid #F1F1F1;
          /*标题*/
          p:first-of-type{
            font-size: 16px;
            color: #303030;
            font-weight: bold;
            margin-bottom: 24px;
          }
          p:not(first-of-type){
            display: flex;
            margin-bottom: 20px;
            span:nth-of-type(1){
              flex: 1;
            }
            span:nth-of-type(2){
              flex:3;
            }
          }
          p:last-of-type{
            margin-bottom: 0;
          }
          &:last-child{
            border-right: none;
          }
        }

      }
      .line5{
        width: 100%;
        background: white;
        padding: 30px;
        margin-top: 20px;
        .line5-t{
          width: 100%;
          display: flex;
          height: 50px;
          background: #F3F3F3;
          color: #666666;
          padding-left: 24px;
          padding-right: 24px;
          align-items: center;
          span:nth-of-type(1){
            flex: 3;
          }
          span:nth-of-type(2){
            flex: 1;
          }
          span:nth-of-type(3){
            flex: 1;
            text-align: right;
          }
          span:nth-of-type(4){
            flex: 1;
            text-align: right;
          }
          span:nth-of-type(5){
            flex: 1;
            text-align: right;
          }
        }
        .item{
          div{
            display: flex;
           padding: 19px 24px;
            border-bottom: 1px solid #E8E8E8;
            color: #333333;
            p:nth-of-type(1){
              flex: 3;
              display: flex;
              /*商品图标*/
              img{
                width: 110px;
                height: 110px;
              }
              /*商品名称*/
              span{
                margin-left: 10px;
                margin-right: 20px;
              }
            }
            p:nth-of-type(2){
              flex: 1;
              color: #999999;
            }
            p:nth-of-type(3){
              flex: 1;
              text-align: right;
            }
            p:nth-of-type(4){
              flex: 1;
              text-align: right;
            }
            p:nth-of-type(5){
              flex: 1;
              text-align: right;
            }

          }
        }
      }
      .line6{
        background: white;
        padding-left: 48px;
        padding-right: 48px;
        height: 138px;
        p{
          display: flex;
          margin-bottom: 25px;
          color: #666666;
          span:nth-of-type(1){
            flex: 5;
          }
          span:nth-of-type(2){
            flex: 1;
            text-align: right;
          }
          span:nth-of-type(3){
            flex: 1;
            text-align: right;
          }
        }


      }
      .line7{
        height: 129px;
      }

    }

  }
  .totalAmout{
    font-size: 24px;
  }
</style>
