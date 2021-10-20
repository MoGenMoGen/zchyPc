<template>
  <!--  去支付页面-->
  <div id="home" :style="{width:bWidth+'px'}">
    <div class="fenqiSuccess" v-show="fenqiShow">
       <div class="fenqiMain">
          <img :src="suc"/>
          <b>分期付款申请提交成功</b>
         <div class="btn">
           <p @click="toPage('../Trade/onlinetrading')">查看其他商品</p>
           <p @click="toPage('../personal/myOrder1?cdType=1')">查看申请状态</p>
         </div>
       </div>
    </div>
    <div class="qrCode" v-if="qrShow">
      <div>
        <img :src="close" @click="toClose" class="closeI">
        <p>
           <span>请在{{minutes}}分{{seconds}}秒内支付</span>
          <img class="qrImg" :src="qrImg">
          <img class="wxIcon" :src="wxPayIcon" alt="">
        </p>
      </div>
    </div>
    <div class="header">
      <div :style="{width:width+'px'}">
        <p><img :src="logo" @click="toPage('/')">收银台</p>
        <p><span @click="toOrder">我的订单</span><span class="fenge">|</span><span>使用帮助</span></p>
      </div>
    </div>

    <div class="cont" :style="{width:width+'px'}">
      <div class="line1">
        <div>
          <div>
            <p>订单提交成功，请尽快付款！订单号：{{ids}}</p>
            <p v-if="totalPrice!=price">应付金额 <span style="color: #E64347;font-size: 24px">{{totalPrice}}</span>元</p>
            <p v-else>应付金额 <span style="color: #E64347;font-size: 24px">价格面议</span></p>
          </div>
          <div>
            <!-- <p>请您在 <span><time-out v-model="time" @finish="timeOut"></time-out></span>内完成支付，否则订单会被自动取消</p> -->
            <p></p>
            <p style="color: #2778BE" @click="toDetail">查看订单详情</p>
          </div>
        </div>
      </div>
      <div class="line1" style="border: none">
        <div>
          <div>
            <p v-if="proList[0]">收货地址： {{proList[0].addrNm}}{{proList[0].addrDtl}} 收货人：{{proList[0].receNm}} {{proList[0].receMob}}</p>

          </div>
          <div>
            <p>商品名称：{{proNm}}</p>
          </div>
        </div>
      </div>
      <div class="line3">
        <div class="item" v-for="(item,index) in list" :key="item.id" :class="{active:selectId==item.id}" @click="toChoose(item)">
          <div class="itemCont">
            <div>
              <p>
                <img class="checked" :src="checked" v-show="item.id==selectId">
                <img class="checked" :src="unChecked" v-show="item.id!=selectId">
                <img class="type" :src="item.img">
                <span>{{item.nm}}</span>
              </p>
            </div>
            <div class="uploadImg"  v-show="item.id==4 && selectId==item.id ">
              <div class="payVoucher" v-for="(item,index) in img">
                <img :src="delImg" class="delImg" @click="deleteImg(index)"/>
                <img :src="item" @click="toLink(item)"/>
              </div>
              <div class="upload">
                <p>+</p>
                <span>上传付款凭证</span>
                <input ref="uploads" type="file" v-if="ifUpload"  multiple="multiple" @change="upImg($event)"/>
              </div>
            </div>
            <div v-if="item.id==selectId" class="money">
              支付<span>{{totalPrice}}</span>元
            </div>
          </div>
          <!--<div class="bankList" v-show="item.id==1 && selectId==item.id ">-->
            <!--<div class="list">-->
              <!--<el-radio v-model="brankId" :label="v.id" v-for="(v,i) in brandList" :key="item.id"><p><img :src="v.imgUrl"/></p></el-radio>-->
            <!--</div>-->
            <!--<button>到网上银行支付</button>-->
          <!--</div>-->

        </div>


        <!--<div class="jiaoyan">-->
          <!--<img :src="suc">-->
          <!--安全设置检测成功!无需短信校验-->
        <!--</div>-->
        <!--<div class="tip">-->
          <!--支付宝支付密码-->
        <!--</div>-->
        <!--        密码输入框-->

        <div class="paybox">

        </div>
        <div class="btn" @click="toPay">
          确认支付
        </div>

      </div>
    </div>
    <div style="height: 106px" class="bg">

    </div>
  </div>
</template>

<script>
  import checked from '../../assets/img/order/已勾选.png'
  import unChecked from '../../assets/img/order/未勾选.png'
  import zhifubao from '../../assets/img/order/zhifubao.png'
  import weixin from '../../assets/img/order/weixin.png'
  import xianxia from '../../assets/img/order/xianxia.png'
  import yinlian from '../../assets/img/order/yinlian.png'
  import fenqi from '../../assets/img/order/分期.png'
  import suc from '../../assets/img/success.png'
  import close from "@/assets/img/close.png"
  import wxPayIcon from '@/assets/img/wxPay.png'
  import timeOut from "../../components/timeOut/timeOut";
  import {mapState} from "vuex";
  import delImg from '@/assets/img/personal/del.png'
  const axios = require('axios');
  export default {
    name: "toPay",
    components: {
      timeOut,
    },
    data() {
      return {
        ifUpload:true,
        delImg,
        img:[],
        fenqiShow:false,
        // logo:'https://sinovat.oss-cn-shanghai.aliyuncs.com/0875a132ec924b17b73aae3d9c644a81_logo.png',//聚联logo
        logo:'https://sinovat.oss-cn-shanghai.aliyuncs.com/55b56e882f684d91b7fb7a0d0e6ded04_新logo.png',
        checked,
        fenqi,
        unChecked,
        zhifubao,
        weixin,
        xianxia,
        yinlian,
        wxPayIcon,
        close,
        suc,
        brankId: 1,
        brandList: [{
          id: 1,
          imgUrl: 'https://sinovat.oss-cn-shanghai.aliyuncs.com/ea6b01f89744454288c94b2d69a943bc-%E4%B8%AD%E4%BF%A1%E9%93%B6%E8%A1%8C.png',
        }, {
          id: 2,
          imgUrl: 'https://sinovat.oss-cn-shanghai.aliyuncs.com/ad7f521c4c6842d4a861c08ae29e32d8-%E4%B8%AD%E5%9B%BD%E9%93%B6%E8%A1%8C.png',
        }, {
          id: 3,
          imgUrl: 'https://sinovat.oss-cn-shanghai.aliyuncs.com/76dd67e91e434738bc233f4c50013be9-%E6%8B%9B%E5%95%86%E9%93%B6%E8%A1%8C.png',
        }, {
          id: 4,
          imgUrl: 'https://sinovat.oss-cn-shanghai.aliyuncs.com/833332c7489e49048fcb0ed6216984b4-%E9%82%AE%E6%94%BF.png',
        },{
          id:5,
          imgUrl:'https://sinovat.oss-cn-shanghai.aliyuncs.com/685e958a19984a3685b3f4e66616ab62-%E5%85%B4%E4%B8%9A%E9%93%B6%E8%A1%8C.png'
        },{
          id:6,
          imgUrl:'https://sinovat.oss-cn-shanghai.aliyuncs.com/0c6adffd42a24b6889e395dc4fa275e0-%E4%B8%8A%E6%B5%B7%E9%93%B6%E8%A1%8C.png'
        },{
          id:7,
          imgUrl:'https://sinovat.oss-cn-shanghai.aliyuncs.com/3b76c25c3b51436e9f9588dd08c787c8-%E6%B5%A6%E5%8F%91%E9%93%B6%E8%A1%8C.png'
        },{
          id:8,
          imgUrl:'https://sinovat.oss-cn-shanghai.aliyuncs.com/8026698744d2410dbe10aedb7108df26-%E5%86%9C%E4%B8%9A%E9%93%B6%E8%A1%8C.png'
        },{
          id:9,
          imgUrl:'https://sinovat.oss-cn-shanghai.aliyuncs.com/1dcb24eb45e34df09ff9892ab2d64f91-%E6%B0%91%E7%94%9F%E9%93%B6%E8%A1%8C.png'
        },{
          id:10,
          imgUrl:'https://sinovat.oss-cn-shanghai.aliyuncs.com/727d93b602054fc09a0044a05f5c16e4-%E4%BA%A4%E9%80%9A%E9%93%B6%E8%A1%8C.png'
        },{
          id:11,
          imgUrl:'https://sinovat.oss-cn-shanghai.aliyuncs.com/7445acc5a8a546e4b0669bdcbc2a754c-%E5%BB%BA%E8%AE%BE%E9%93%B6%E8%A1%8C.png'
        },{
          id:12,
          imgUrl:'https://sinovat.oss-cn-shanghai.aliyuncs.com/be6ec5743e0b4b65bcefa297a9ad531e-%E5%8D%8E%E5%A4%8F%E9%93%B6%E8%A1%8C.png'
        },{
          id:13,
          imgUrl:'https://sinovat.oss-cn-shanghai.aliyuncs.com/3694dcf965e84f9bbc941ccd80031471-%E5%B9%BF%E5%8F%91%E9%93%B6%E8%A1%8C.png'
        },{
          id:14,
          imgUrl:'https://sinovat.oss-cn-shanghai.aliyuncs.com/68d7a0576586489ab8b314ecf46aec8c-%E5%85%89%E5%A4%A7%E9%93%B6%E8%A1%8C.png'
        },{
          id:15,
          imgUrl:'https://sinovat.oss-cn-shanghai.aliyuncs.com/23044458506e4e8cbff92407f1640bcf-%E5%B7%A5%E5%95%86%E9%93%B6%E8%A1%8C.png'
        }],
        selectId: 0,
        time: 1200,
        info:{},
        totalPrice: 0,
        proList: [],
        proNm: '',
        ids: '',
        id: '',
        list: [
          {
            id: 1,
            nm: '网银支付',
            checked: false,
            up: false,//上传图片图标
            img: yinlian

          },
          {
            id: 2,
            nm: '支付宝支付',
            checked: false,
            up: false,//上传图片图标
            img: zhifubao
          },
          {
            id: 3,
            nm: '微信支付',
            checked: false,
            up: false,//上传图片图标
            img: weixin
          },
          {
            id: 4,
            nm: '线下支付',
            checked: false,
            up: true,//上传图片图标
            img: xianxia
          },{
            id: 5,
            nm: '申请分期付款',
            checked: false,
            img: fenqi
          }
        ],
        qrShow: false,
        qrImg: '',
        timeNum: 300,
        minutes:5,
        seconds:0,
        timer: '',
      }
    },
    computed: {
      ...mapState([
        'bWidth',
        'width',
        'price'
      ])
    },
    mounted() {
      //我的订单结算
      if (this.$route.query.id) {
          this.orderInfo(this.$route.query.id)
      } else {
        //购物车结算
        this.getInfo()
      }

    },
    methods: {
      getInfo() {
        this.proList = JSON.parse(this.until.seGet('orderPayList'))
        console.log(this.proList)
        this.info = this.proList[0]
        let nmArr = []
        let idArr = []
        let idArray = []
        this.proList.forEach(item => {
          if (item.items) {
            item.items.forEach(v => {
              nmArr.push(v.goodsNm)
            })
          } else {
            nmArr.push(item.goodsNm)
          }
          idArr.push(item.orderCd)
          idArray.push(item.id)
          this.totalPrice += item.orderAmt
          this.totalPrice += item.payShip
        })
        this.proNm = nmArr.join(',')
        this.ids = idArr.join(',')
        this.id = idArray.join(',')

      },
      async orderInfo(id) {
        let info = await this.api.orderDetail(id);
        this.info = info
        let nmArr = []
        info.itms.forEach(item => {
          if (item.items) {
            item.items.forEach(v => {
              nmArr.push(v.goodsNm)
            })
          } else {
            nmArr.push(item.goodsNm)
          }
        })
        this.totalPrice  = info.orderAmt;
        this.proNm = nmArr.join(',')
        this.ids = info.orderCd;
        this.id = info.id;

      },

      toPay() {
        if (this.selectId == 1) { //网银支付
          let param = {
            tradeNo: this.ids,
            date: this.info.crtTm.split('-').join('').split(':').join('').split(' ').join(''),
            totalAmount: this.totalPrice,
            payChannel: "0",
            bankChoiceType: "0",
            pageNotifyUrl: "https://www.baidu.com"
          }
          this.api.toPayBank(param).then(res=>{
              axios.post(res.reqUrl,res.reqParams)
            // this.api.toPayBank2('http://60.191.15.90:29082/B2BPAY/gateway',res.reqParams)
          })
          // console.log(this.id)
          // console.log(this.hostUrl)
          // this.api.orderCheck(this.id).then(() => {
          //   window.open(this.hostUrl + '/pay/aliPay/pcPay?ids=' + this.id+'&returnUrl='+window.location.origin+'/sinovat')
          // })
        }else if (this.selectId == 2) { //支付宝
          console.log(this.id)
          console.log(this.hostUrl)
          this.api.orderCheck(this.id).then(() => {
            window.open(this.hostUrl + '/pay/aliPay/pcPay?ids=' + this.id+'&returnUrl='+window.location.origin+'/sinovat')
          })
        } else if (this.selectId == 3) { //微信
          console.log(this.id)
          this.api.orderCheck(this.id).then((res) => {
            if (res.code == '0') {
              this.api.wxPay(this.id).then((res) => {
                if (res.code == '0') {
                  this.qrImg = res.data
                  this.timeNum = 300
                  this.minutes=5
                  this.seconds=0
                  this.qrShow = true
                  this.payStatus()
                  this.timer = setInterval(()=>{
                    this.timeNum--
                    this.minutes=parseInt(this.timeNum/60)
                    this.seconds=this.timeNum-this.minutes*60
                    if(this.timeNum==0){
                      this.$message({
                        message: '已超时，请重新获取二维码付款',
                        type: 'warning'
                      });
                      clearInterval(this.timer);
                      this.qrShow = false
                    }
                  }, 1000);
                }
              })

            }
          })
        }else if (this.selectId == 4) { //线下支付
          if(this.img.length==0){
            this.$message({
              message: '请上传付款凭证',
              type: 'warning'
            });
            return
          }
          let param = {
            id:this.id,
            img:this.img.join(','),
          }
          console.log(param)
          this.api.payXX(param).then(() => {
            this.$message({
              message: '提交成功',
              type: 'success'
            });
            let this_=this
            setTimeout(()=>{
                this_.$router.push('../order/orderDetail?id=' + this.id)
            },1500)
          })
        }else if (this.selectId == 5) { //分期
          this.api.applyStaging(this.id).then(()=>{
            this.fenqiShow = true
          })
          // this.api.orderCheck(this.id).then((res) => {
          //   if (res.code = '0') {
          //     this.api.wxPay(this.id).then((res) => {
          //       if (res.code = '0') {
          //         this.qrImg = res.data
          //         this.qrShow = true
          //         this.timer = setInterval(this.payStatus, 1000);
          //       }
          //     })
          //
          //   }
          // })
        }
      },
      //微信支付是否成功
      payStatus() {
        if(this.timeNum==0){
          return
        }
        this.api.wxPayStatus(this.id).then((res) => {

          if (res.data == 1) {
            clearInterval(this.timer);
            this.qrShow = false
            this.$message({
              message: '支付成功',
              type: 'success'
            });
            let this_=this
            setTimeout(() => {
              // window.location.href = './order/orderDetail?id=' + this.id
                this_.$router.push('../order/orderDetail?id=' + this.id)
            }, 1500);
          } else if (res.data == 1) {
            this.payStatus()
          }
        })
      },
      toClose() {
        clearInterval(this.timer);
        this.qrShow = false
        this.timeNum=0
        // this.minutes=5
        // this.seconds=0
      },
      timeOut() {
        window.console.log("计时结束")
        this.api.cancelPay(this.id)
        this.$message({
          message: '支付超时,已取消订单',
          type: 'warning'
        });
        setTimeout(() => {
          this.$router.push('../Trade/onlinetrading')
        }, 1500);
      },
      async upImg(e){
        this.ifUpload = false

        console.log('上传图片')
        let blob = e.target.files;
        for(let i=0;i<blob.length;i++){
          if(blob[i].type.indexOf('image')===-1){ //非图片格式
            this.$message({
              message: '请上传图片格式的文件',
              type: 'warning'
            });
            // this.$refs.upload.value = null
            // console.log(this.$refs.upload)
            // return
          }else {
            let img = await this.api.uploadImg2(blob[i])
            this.img.push(img)
          }
        }
        setTimeout(()=>{
          this.ifUpload = true

        },100)

      },
      //删除图片
      deleteImg(index){
        this.img.splice(index,1)
      },
      toLink(url){
        window.open(url)
      },
      toChoose(item) {
        if (item.id != this.selectId) {
          console.log('支付方式选择'+item.id)
          this.selectId = item.id
        }
      },
      toPage(url) {
        this.$router.push(url)
      },
      toOrder() {
        this.$router.push("../personal/myOrder1?cdType=1")
      },
      toDetail() {
        this.$router.push("./orderDetail?id=" + this.proList[0].id)
      },
    }
  }
</script>

<style scoped lang="less">
  @import url("../../assets/css/init.less");
  #home {
    background: #F3F3F3;
    background-image: url("https://sinovat.oss-cn-shanghai.aliyuncs.com/6070eb46a13e4d9488b6669e78324ab5-%E6%9C%AA%E6%A0%87%E9%A2%98-1%20(2).png");
    background-position: top center;
    background-repeat: no-repeat;
    background-size: contain;

    .uploadImg{
      display: flex;
      display: -webkit-flex;
      align-items: center;
      margin: 32px 0;
      // width: 85px;
      .payVoucher{
        width: 162px;
        // height: 85px;
        height: 156px;
        position: relative;
        margin-right: 5px;
        img{
          width: 100%;
          height: 100%;
        }
        .delImg{
          position: absolute;
          top: -7.5px;
          right: -7.5px;
          z-index: 2;
          width: 15px;
          height: 15px;
        }
      }
      .upload{
        display: block!important;
        color: #C6C6C6;
        // width: 85px;
        width: 162px;
        // height: 85px;
        height: 156px;
        border: 1px dashed #C6C6C6;
        position: relative;
        p{
          // width: 85px;
          width: 100%;
          text-align: center;
          font-size: 50px;
          font-weight: normal;
          height: 85px !important;
          line-height: 50px;
          display: block!important;
          padding-top: 30px;
        }
        span{
          display: block;
          color: #666666;
          font-size: 14px;
          font-weight: normal;
          text-align: center;
        }
        input{
          width: 100%;
          height: 100%;
          opacity: 0;
          position: absolute;
          left: 0;
          top: 0;
        }
      }
    }
    .fenqiSuccess{
      background: rgba(0,0,0,0.23);
      position: fixed;
      z-index: 99;
      left: 0;
      top:0;
      width: 100%;
      height: 100%;
      display: flex;
      display: -webkit-flex;
      .fenqiMain{
        width: 541px;
        height: 308px;
        border-radius: 8px;
        background: #ffffff;
        margin: auto;
        text-align: center;
        img{
          width: 38.9px;
          display: block;
          margin: 60px auto 15px;
        }
        b{
          font-size: 20px;
        }
        .btn{
          overflow: hidden;
          margin: 54px auto 0;
          p{
            width: 154px;
            height: 38px;
            text-align: center;
            line-height: 38px;
            cursor: pointer;
            font-size: 18px;
            box-sizing: border-box;
            border-radius: 18.5px;
            float: left;
            &:first-of-type{
              background: @themeColor;
              color: #ffffff;
              margin-right: 59px;
              margin-left: 95px;
            }
            &:last-of-type{
              background: #ffffff;
              color: @themeColor;
              border: 1px solid @themeColor;
            }
          }
        }
      }
    }
    .qrCode {
      position: fixed;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      background: rgba(0, 0, 0, .5);
      z-index: 100;
      display: flex;
      justify-content: center;
      align-items: center;

      > div {
        position: relative;
        width:678px;
        height:468px;
        background:rgba(255,255,255,1);
        border-radius:5px;
        display: flex;
        // height: auto;
        >p{
          display: flex;
          flex-direction: column;
          align-items: center;
          justify-content: center;
          margin: auto;
          // justify-content: center;
          >span{
            font-size:16px;
            color:rgba(230,67,71,1);
            margin-bottom: 23px;
          }
          .qrImg{
            width:226px;
            height:226px;
            border:1px solid rgba(75, 179, 22, 1);
            border-radius:5px;
            padding: 17px;
            margin-bottom: 26px;
          }
          .wxIcon{

          }
        }
        .closeI {
          cursor: pointer;
          position: absolute;
          right: 10px;
          top: 10px;
          width: 20px;
          height: 20px;
        }

      }
    }

    .header {
      background: white;
      height: 110px;
      width: 100%;
      box-shadow: 0px 6px 10px 0px rgba(13, 4, 8, 0.1);

      div {
        height: 100%;
        margin: 0 auto;
        align-items: center;
        justify-content: space-between;
        display: flex;

        p:nth-of-type(1) {
          display: flex;
          align-items: center;
          color: #404040;
          font-size: 36px;
          font-weight: bold;

          img {
            width: 450px;
            margin-right: 14px;
            cursor: pointer;
          }
        }

        p:nth-of-type(2) {
          margin-right: 86px;
          display: flex;
          align-items: center;
          color: #2778BE;

          .fenge {
            margin-left: 20px;
            margin-right: 20px;
            color: #D1D1D1;
            font-size: 12px;
          }

          span:nth-of-type(1), span:nth-of-type(3) {
            &:hover {
              cursor: pointer;
            }
          }

        }
      }
    }

    .cont {
      margin: 0 auto;

      .line1 {
        border-bottom: 1px solid #D8D8D8;
        padding: 30px 19px;
        color: #333333;

        div {
          div {

            display: flex;
            align-items: center;
            justify-content: space-between;
          }

          div:nth-of-type(1) {

          }

          div:nth-of-type(2) {
            margin-top: 15px;

            &:hover {
              cursor: pointer;
            }

          }
        }
      }

      .bankList {
        width: 100%;

        .list {
          width: 100%;
          overflow: hidden;

          /deep/ .el-radio {
            width: 20%;
            float: left;
            margin-right: 0;
            display: flex;
            display: -webkit-flex;
            align-items: center;
            height: 52px;
            padding-right: 50px;
            box-sizing: border-box;
            margin-bottom: 40px;

            .el-radio__label {
              flex: 1;

              p {
                width: 100%;
                height: 50px;
                border: 1px solid #DADADA;
                display: flex;
                display: -webkit-flex;

                img {
                  margin: auto;
                }
              }
            }
          }
        }

        > button {
          width: 240px;
          height: 46px;
          text-align: center;
          line-height: 46px;
          background: #E64347;
          color: #ffffff;
          border-radius: 5px;
          border: none;
          font-size: 18px;
          margin: 0 auto 60px;
          display: block;
        }
      }

      .line3 {
        background: white;
        padding: 20px 19px;
        padding-bottom: 91px;


        /*1px  从原点开始，沿x轴正方向的长度（倘若为负值，为沿x轴负方向的长度）；
　　　　　　　　　　　　　　　　　  2px  从原点开始，沿y轴正方向的长度；（倘若为负值，为沿y轴负方向的长度）
　　　　　　　　　　　　　　　　　  3px  阴影的模糊度，只允许为正值；
　　　　　　　　　　　　　　　　　  4px  阴影扩展半径；
　　　　　　　　　　　　　　　　　  #ccc  阴影颜色；
　　　　　　　　　　　　　　　　　  inset  设置为内阴影（如果不写这个值，默认为外阴影）；*/
        box-shadow: 0px -6px 10px 0px rgba(13, 4, 8, 0.05), /*上边阴影  */ -6px 0px 10px 0px rgba(13, 4, 8, 0.05), /*左边阴影  */ 6px 0px 10px 0px rgba(13, 4, 8, 0.05), /*右边阴影  */ 0px 6px 10px 0px rgba(13, 4, 8, 0.05); /*下边阴影  */

        /*item*/

        .item {

          padding-left: 20px;
          color: #333333;
          font-size: 18px;
          border-bottom: 1px solid #E8E8E8;

          .itemCont {
            display: flex;
            justify-content: space-between;

            div:nth-of-type(1) {
              display: flex;

              p {
                height: 115px;
                display: flex;
                align-items: center;

                .checked {
                  width: 13px;
                  margin-right: 31px;
                }

                .type {
                  margin-right: 23px;
                  width: 35px;
                }

                span {
                  margin-right: 29px;
                }
              }

              .up {
                display: block;
                margin: 32px 0 32px 29px;
                width: 162px;
                height: 156px;
                border: 1px dotted #D3D3D3;
                border-radius: 5px;
                position: relative;

                span {
                  display: block;
                  position: absolute;

                }

                span:nth-of-type(1) {

                  top: 40%;
                  left: 50%;
                  transform: translate(-50%, -50%);
                  font-size: 50px;
                  color: #999999;
                  font-weight: 100;
                }

                span:nth-of-type(2) {
                  width: 100%;
                  top: 70%;
                  left: 50%;
                  transform: translate(-50%, -50%);
                  position: absolute;
                  font-size: 14px;
                  color: #666666;
                  text-align: center;
                }
              }
            }

            .money {
              height: 115px;
              display: flex;
              align-items: center;
              padding-right: 60px;

              span {
                color: #E64347;
                font-weight: 400;
                font-size: 24px;
              }
            }
          }

          &:hover {
            cursor: pointer;
          }


        }

        /*校验*/

        .jiaoyan {
          padding-left: 20px;
          color: #666666;
          font-size: 16px;
          margin-top: 25px;
          display: flex;
          align-items: center;

          img {

            width: 21px;
            margin-right: 19px;
          }
        }

        /*提示*/

        .tip {
          margin-top: 25px;
          padding-left: 59px;
          color: #666666;
          font-size: 16px;
        }

        .paybox {
          margin-left: 59px;
          width: 30px;
        }

        /*按钮*/

        .btn {
          width: 240px;
          height: 46px;
          background: rgba(230, 67, 71, 1);
          border-radius: 5px;
          font-size: 18px;
          font-family: Microsoft YaHei;
          font-weight: 400;
          color: rgba(255, 255, 255, 1);
          line-height: 40px;
          display: flex;
          align-items: center;
          justify-content: center;
          margin-left: 59px;
          margin-top: 49px;

          &:hover {
            cursor: pointer;
          }


        }
      }

    }
  }

  /*选中的付款方式*/
  .active {
    box-shadow: 0px -3px 5px 0px rgba(13, 4, 8, 0.05), /*上边阴影  */ -3px 0px 5px 0px rgba(13, 4, 8, 0.05), /*左边阴影  */ 3px 0px 5px 0px rgba(13, 4, 8, 0.05), /*右边阴影  */ 0px 3px 5px 0px rgba(13, 4, 8, 0.05); /*下边阴影  */
  }

  .bg {
    background: url("../../assets/img/bg.png") bottom center repeat-x;
  }
</style>
