<template>
  <!--  结算页-->
  <div id="home" :style="{width:bWidth+'px'}">
    <!--新增、修改地址-->
    <div class="addrAdd" v-show="addrShow">
      <div class="addrAddMain">
        <div class="title">
          {{title}}
          <img :src="close" @click="addrShow=false"/>
        </div>
        <addr-change :addrInfo="currentAddrInfo" @cancel="addrShow=false" @save="addrSave"></addr-change>
      </div>
    </div>
    <div class="cont" :style="{width:width+'px'}">
      <div class="line1">
        <p><img :src="payment">结算页</p>
        <div class="step">
          <!--          <el-steps :active="active" finish-status="success" align-center="true">-->
          <!--            <el-step title="我到购物车"></el-step>-->
          <!--            <el-step title="填写核对订单信息"></el-step>-->
          <!--            <el-step title="成功提交订单"></el-step>-->
          <!--          </el-steps>-->
          <img src="../../assets/img/step2.png">
        </div>
      </div>
      <div class="cont">
        <div class="block1">
          <p>选择收货地址</p>
          <p @click="addAdd('')">新增收货地址</p>
        </div>
        <div class="block2">
          <div class="item" v-for="item in showList" :key="item.id">
            <div class="item-in" @click="toChoose(item)" :class="{active: item.id==selectedId}"
                 @mouseover="item.setHover=true" @mouseleave="item.setHover=false">
              <p>{{item.addrNm}}({{item.receNm}}收)</p>
              <p>{{item.addrDtl}} {{item.receMob}}</p>
              <p class="alter" v-if="item.id==selectedId" @click.stop="addAdd(item)">修改</p>
              <p class="default" v-if="item.hasDef">默认地址</p>
              <p class="set" v-if="item.id==selectedId && !item.hasDef" :class="{showHover:item.setHover}" @click="toDefault(item)">设为默认</p>
              <img class="choose" :src="icChoose" v-if="item.id==selectedId">
            </div>
          </div>
        </div>
        <div class="block3" v-if="addrList.length>4">
          <p @click="show">{{showAll?'收起':'显示全部地址'}}</p>

        </div>
        <div class="block4">
          <p>确认订单信息</p>
        </div>
        <div class="block5">
          <div>
            <p>产品信息</p>
            <p>产品属性</p>
            <p>单价</p>
            <p>数量</p>
            <p>小计</p>
          </div>
        </div>
        <div class="block6">
          <div class="shopItem" v-for="item in list" :key="item.id">
            <p class="name"><img :src="shopName">店铺：{{item.nm}}</p>
            <div class="goodCont">
              <div >
                <div  v-for="j in item.items" :key="j.id">
                  <div class="d1">
                    <p>
                      <img :src="j.img">
                      <span>{{j.goodsNm}}</span>
                    </p>
                    <p v-if="j.leadTime">{{j.attrDesc}}<br/>交货期：{{j.leadTime}}天</p>
                    <p v-else>{{j.attrDesc}}</p>
                    <p v-if="j.origPrice!=price">￥{{j.origPrice}}</p>
                    <p v-else>价格面议</p>
                    <p>{{j.qty}}</p>
                    <p v-if="j.origPrice!=price">￥{{(j.qty *j.origPrice).toFixed(2)}}</p>
                    <p v-else>价格面议</p>
                  </div>
                  <div class="d2" v-if="j.gift === 1 && type === 'cart'">
                    <div v-for="z in j.giftItems" :key="z.id">
                      <el-link @click="toGoods(z.id)"  style="color: #7F828B" :underline="false">[赠品] {{z.nm}} {{z.attrDesc}} </el-link>
                    </div>
                  </div>
                  <div class="d2" v-if="j.gift === 1 && type === '1'">
                    <div v-for="z in j.giftItems" :key="z.giftGoodsId">
                      <el-link @click="toGoods(z.giftGoodsId)"  style="color: #7F828B" :underline="false">[赠品] {{z.goodsNm}} {{z.model}} </el-link>
                    </div>
                  </div>

                </div>
                <div>
                  <el-divider></el-divider>
                  <span>运费：￥ {{item.fare}}</span>
                </div>

              </div>
            </div>

          </div>
        </div>

        <!-- 优惠券-->
<!--        <div class="coupon">-->
<!--          <div class="item" v-for="item in couponList" :key="item.couponId">-->
<!--            <div class="item-in" @click="toCoupon(item)" :class="{active: item.couponId==couponId}"  @mouseover="item.setHover=true" @mouseleave="item.setHover=false">-->
<!--              <el-row>-->
<!--                <el-col :span="12">-->
<!--                  <div style="padding: 10px">-->
<!--                    <p class="p1" v-if="item.discType !== 1">¥</p><p class="p2" v-if="item.discType !== 1">{{item.discAmt}}</p>-->
<!--                    <p class="p1" v-if="item.discType === 1"></p><p class="p2" v-if="item.discType === 1">{{item.discRate*10}}折</p>-->
<!--                  </div>-->
<!--                </el-col>-->
<!--                <el-col :span="12">-->
<!--                  <p class="p3" v-if="item.discType === 0">满减券({{item.couponType===1?'店铺':'平台'}})</p>-->
<!--                  <p class="p3" v-if="item.discType === 2">代金券({{item.couponType===1?'店铺':'平台'}})</p>-->
<!--                  <p class="p3" v-if="item.discType === 1">折扣券({{item.couponType===1?'店铺':'平台'}})</p>-->
<!--                  <p v-if="item.discType === 0">满{{item.payAmt}}元可用</p>-->
<!--                </el-col>-->
<!--              </el-row>-->
<!--                <p class="p4" style="margin-top: 15px">{{item.useType===0?'全品类商品可使用':'限定部分商品是用'}}</p>-->
<!--                <p class="p4">有限期至{{item.vldToTm.slice(0,10)}}</p>-->
<!--            </div>-->
<!--          </div>-->
<!--        </div>-->

        <div class="block7">
          <div>
            <p><span class="red">{{totalNum}}</span> 件商品，总商品金额：</p>
            <p v-if="totalPrice=='价格面议'">价格面议</p>
            <p v-else>￥{{totalPrice.toFixed(2)}}</p>
          </div>
        </div>
<!--        <div class="block7">-->
<!--          <div>-->
<!--            <p>优惠金额：</p>-->
<!--            <p>-￥{{couponPrice}}</p>-->
<!--          </div>-->
<!--        </div>-->

        <div class="block7">
          <div>
            <p>运费：</p>
            <p>￥{{shipPrice}}</p>
          </div>
        </div>

        <div class="block7">
          <div style="background: #F3F3F3;padding-top: 16px;padding-bottom: 16px">
            <p>应付总额：</p>
            <p style="color: #E64347;font-size: 24px">{{payPrice}}</p>
          </div>
        </div>
        <div class="block8">
          <p>寄送至：{{ currentAddr.addrNm ? currentAddr.addrNm+currentAddr.addrDtl : ''}}</p>
          <p>收货人：{{currentAddr.receNm ? currentAddr.receNm+currentAddr.receMob : ''}}</p>
        </div>

        <div class="block9">
          <p @click="toPage('../order/cart')"><img :src="backIcon">返回购物车</p>
          <p @click="toSub">提交订单</p>
        </div>

      </div>


    </div>
  </div>

</template>

<script>

  import payment from '../../assets/img/payment.png'
  import icChoose from '../../assets/img/choosed.png'
  import shopName from '../../assets/img/shopName.png'
  import backIcon from '../../assets/img/backIcon.png'
  import close from '@/assets/img/close.png'
  import addrChange from '@/components/addrChange'
  import {mapState} from "vuex";

  export default {
    name: "payment",
    components: {
      addrChange
    },
    data() {
      return {
        type:'',
        close,
        payment,
        icChoose,
        shopName,
        backIcon,
        addrShow: false,
        currentAddrInfo: null,
        title: '收货人信息',
        totalNum: 0,
        totalPrice: 0,
        shipPrice: 0,//运费
        selectedId: '',//地址选中id
        couponId:'', //优惠券选中的id
        currentAddr: {},
        currentCouPon:{},
        showAll: false,//显示全部地址
        showList: [],//显示的地址
        addrList: [],
        list: [],
        couponList:[],
        couponPrice:0,
        flag:0  //判断是否有价格面议
      }
    },
    created(){

    },
    mounted() {
      this.list = JSON.parse(this.until.seGet('cartList'))
      this.getList()
      console.log(this.list)
      this.getAddrList()
      this.type = this.until.getQueryString('payType');
    },
    computed: {
      ...mapState([
        'bWidth',
        'width',
        'price'
      ]),
      payPrice() {
        if(this.totalPrice =='价格面议'){
          return '价格面议'
        }else {
          console.log(this.totalPrice)
          console.log(this.shipPrice)
          return '￥'+ (Number(this.totalPrice+this.shipPrice).toFixed(2))
        }
      }
    },
    methods: {
      //跳转产品详情
      toGoods(id) {
        let url = '../sinovat/Trade/productDetail?id='+ id + "&type=product"
        window.open(url)
      },

      //商品列表
      getList() {
        this.totalNum = 0
        this.totalPrice = 0
        this.flag=0
        let length=0
        console.log(this.list)
        this.list.forEach(item => {
          item.items.forEach(v => {
            this.totalNum += v.qty
            length++
            if(v.origPrice==this.price){
              this.flag++
              this.totalPrice += 0
            }else {
              this.totalPrice += v.qty * v.origPrice
            }
          })
        })
        if(this.flag==length){//全部都是价格面议
          this.totalPrice = '价格面议'
        }

      },
      //地址列表
      async getAddrList() {
        let qry = this.query.new()
        this.query.toP(qry, 1, 100)
        this.query.toO(qry, "hasDef", "desc")
        this.addrList = await this.api.addrList(this.query.toEncode(qry))
        if (this.addrList.length > 0) {
          this.currentAddr = this.addrList[0]
          this.selectedId = this.currentAddr.id
        }
        this.showList = this.addrList.slice(0, 4)
        this.fare()



      },

      // couList(){
      //   if (this.until.getQueryString('payType') == 'cart') {
      //     this.getCouponList();
      //   } else {
      //     console.log("商品")
      //     console.log(this.list[0].skuId)
      //     //this.byNewCouponList(this.totalPrice);
      //   }
      //
      // },

      //优惠券列表
      // getCouponList(){
      //   let idArr = []
      //   this.list.forEach(item => {
      //     item.items.forEach(v => {
      //       idArr.push(v.cartId)
      //     })
      //   })
      //   this.api.cartUsable(idArr.join(',')).then((res)=>{
      //     if (res.code === 0){
      //       for (let i = 0; i < res.data.list.length; i++) {
      //           if (res.data.list[i].usable === 1){
      //             this.couponList.push(res.data.list[i])
      //           }
      //       }
      //     }
      //   })
      // },

      // //立即下单优惠券列表
      // byNewCouponList(price){
      //   this.api.byNewUsable(price,).then((res)=>{
      //     if (res.code === 0){
      //       for (let i = 0; i < res.data.list.length; i++) {
      //         if (res.data.list[i].usable === 1){
      //           this.couponList.push(res.data.list[i])
      //         }
      //       }
      //     }
      //   })
      // },



      addrSave() {
        this.getAddrList()
        this.addrShow = false
      },

      //查询运费
      fare(){
        if (!this.selectedId) {
          this.$message({
            message: '请选择收货地址！',
            type: 'warning'
          });
          return
        }
        // this.list = JSON.parse(this.until.seGet('cartList'))
        if (this.until.getQueryString('payType') == 'cart') {  //购物车
          let idArr = []
          this.list.forEach(item => {
            item.items.forEach(v => {
              idArr.push(v.cartId)
            })
          })
          let param = {
            addrId: this.selectedId,
            cartIds: idArr.join(','),
          }
          this.api.cartFare(param).then(res => {
            this.shipPrice = 0;
            if (res.code === 0){
              for(let key in res.data){
                for (let i = 0; i <this.list.length ; i++) {
                    if (key == this.list[i].id){
                      this.list[i].fare = res.data[key];
                      this.shipPrice += Number(res.data[key]);
                    }
                }
              }
            }
            // this.getList()
          })
        }else {
          this.shipPrice = 0
          this.list.forEach((item,index)=>{
            let param = {
              skuId: item.skuId,
              qty: item.items[0].qty,
              addrId: this.selectedId,
              ship: item.ship,
            }
            this.api.byNewFare(param).then(res => {
              console.log(res)
              item.fare = res.data;
              this.$set(this.list,index,item)
              this.shipPrice += parseFloat(res.data);

            })
          })
          this.getList()
        }
      },

      //提交订单
      toSub() {
        if (!this.selectedId) {
          this.$message({
            message: '请选择收货地址！',
            type: 'warning'
          });
          return
        }
        this.store.commit('changeLoading', true)
        if (this.until.getQueryString('payType') == 'cart') {  //购物车
          let idArr = []
          this.list.forEach(item => {
            item.items.forEach(v => {
              idArr.push(v.cartId)
            })
          })
          let param = {
            addrId: this.selectedId,
            cartIds: idArr.join(','),
          }
          this.api.orderCrearCart(param).then(res => {
            this.api.cartList();
            if(this.flag>0){
              this.$message({
                message: '提交成功，等待卖家修改价格,可在个人中心产品订单查询',
                type: 'success',
                duration: 1500
              });
            }else {
              this.$message({
                message: '提交成功',
                type: 'success',
                duration: 1500
              });
            }
            this.until.seSave('orderPayList', JSON.stringify(res))
            setTimeout(() => {
              if(this.flag>0){
                this.toPage('../personal/')
              }else {
                this.toPage('./toPay')
              }
            }, 1500)
          })
        } else if(this.until.getQueryString('payType')=='quickly'){ //快速下单
            let param=[]
            this.list.forEach(item=>{
              param.push({
                skuId: item.skuId,
                num: item.items[0].qty,
                goodsId:item.goodsId,
                addrId: this.selectedId,
                ship: item.ship,
                supplierId:item.supplier,
                quickId:item.quickId
                })
            })
            this.api.quickOrderCreat(param).then(res => {
              if(this.flag>0){
                this.$message({
                  message: '提交成功，等待卖家修改价格,可在个人中心产品订单查询',
                  type: 'success',
                  duration: 1500
                });
              }else {
                this.$message({
                  message: '提交成功',
                  type: 'success',
                  duration: 1500
                });
              }
              this.until.seSave('orderPayList', JSON.stringify(res))
              setTimeout(() => {
                if(this.flag>0){
                  this.toPage('../personal/')
                }else {
                  this.toPage('./toPay')
                }
              }, 1500)
            })

            console.log(param)

        }else{
            let param = {
              skuId: this.list[0].skuId,
              qty: this.list[0].items[0].qty,
              addrId: this.selectedId,
              ship: this.list[0].ship,
            }
            this.api.orderCreat(param).then(res => {
              if(this.flag>0){
                this.$message({
                  message: '提交成功，等待卖家修改价格,可在个人中心产品订单查询',
                  type: 'success',
                  duration: 1500
                });
              }else {
                this.$message({
                  message: '提交成功',
                  type: 'success',
                  duration: 1500
                });
              }
              let arr = []
              arr[0] = res
              this.until.seSave('orderPayList', JSON.stringify(arr))
              setTimeout(() => {
                if(this.flag>0){
                  this.toPage('../personal/')
                }else {
                  this.toPage('./toPay')
                }
              }, 1500)
            })
          }
      },
      toPage(url) {
        this.$router.push(url)
      },
      //新增、修改收货地址
      addAdd(info) {
        this.currentAddrInfo = info
        this.addrShow = true
      },
      //显示全部地址
      show() {
        this.showAll = !this.showAll
        if (this.showAll) {
          this.showList = this.addrList
        } else {
          this.showList = this.addrList.filter((item, index) => {
            if (index < 4) {
              return item
            }
          })
        }
      },

      //设置默认地址
      toDefault(item) {
        this.store.commit('changeLoading', true)
        this.api.addrUpdDef({id: item.id}).then(() => {
          this.getAddrList()
        })

      },
      //选中地址
      toChoose(item) {
        this.currentAddr = item
        item.setHover = !item.setHover
        if (item.id != this.selectedId) {
          this.selectedId = item.id

        }
        this.fare();
      },
      //选择优惠券
      // toCoupon(item){
      //   this.getList()
      //   this.couponPrice = 0;
      //   this.currentCouPon = item
      //   item.setHover = !item.setHover
      //   if (item.couponId !== this.couponId) {
      //     this.couponId = item.couponId
      //   }
      //   //满减券 代金券
      //   if (item.discType === 0 || item.discType === 2){
      //     this.couponPrice = item.discAmt;
      //   }
      //   //折扣券
      //   if (item.discType === 1){
      //     this.couponPrice =  this.payPrice -(this.payPrice*item.discRate)
      //   }
      // }
    },
    destroyed() {
      sessionStorage.removeItem('cartList')
    }
  }
</script>

<style scoped lang="less">
  #home {
    .el-divider--horizontal {

      margin: 12px 0;
    }
    background: #F3F3F3;
    //收货信息弹窗
    .addrAdd {
      position: fixed;
      background: rgba(0, 0, 0, 0.23);
      width: 100%;
      height: 100%;
      z-index: 99;
      left: 0;
      top: 0;
      display: flex;
      display: -webkit-flex;

      .addrAddMain {
        width: 600px;
        margin: auto;
        background: #ffffff;
        border-radius: 8px;

        .title {
          text-align: center;
          border-bottom: 1px solid #DFDFDF;
          height: 89px;
          line-height: 89px;
          position: relative;
          font-size: 18px;

          img {
            position: absolute;
            right: 30px;
            width: 20px;
            top: 50%;
            transform: translateY(-50%);
          }
        }
      }
    }

    .cont {
      margin: 0 auto;

      .line1 {
        display: flex;
        justify-content: space-between;
        align-items: center;
        height: 91px;
        padding-left: 31px;
        padding-right: 72px;

        p {
          display: flex;
          align-items: center;
          color: #E64347;
          font-size: 22px;
          font-weight: bold;

          img {
            width: 20px;
            margin-right: 9px;

          }
        }

        .step {
          flex-shrink: 0;
          width: 400px;

          /deep/ .el-step__title {
            font-size: 14px;
          }

        }
      }

      .cont {
        padding: 22px 0px;
        background: white;

        .block1 {
          padding: 0 32px;
          display: flex;
          align-items: center;
          justify-content: space-between;

          p:nth-of-type(1) {
            color: #333333;
            font-weight: bold;
          }

          p:nth-of-type(2) {
            color: #2778BE;

            &:hover {
              cursor: pointer;
            }
          }
        }


        .coupon{
          margin-top: 18px;
          padding: 0 16px;
          .item {
            margin-bottom: 30px;
            display: inline-block;
            width: 25%;
            padding: 0 16px;
            /*行内块无法对齐*/
            vertical-align: top;
            .item-in {
              position: relative;
              padding: 12px 11px;
              height: 122px;
              background: url("../../assets/img/优惠券未选中.png");
              background-size: 100% 100%;
              display: flex;
              display: -webkit-flex;
              flex-direction: column;
              /*区域和姓名*/
              .p1{
                color: #df3033;
                display: inline;
              }
              .p2{
                color: #df3033;
                font-size: 35px;
                display: inline;
              }
              .p3{
                color: #df3033;
              }
              .p4{
                color: #7f828b;
                font-size: 12px;
                margin-left: 10px;
              }
              /*  选中图标*/
              .choose {
                width: 36px;
                position: absolute;
                bottom: 0;
                right: 0;
              }
              &:hover {
                background: url("../../assets/img/优惠券选中.png") !important;
                background-size: 100% 100% !important;
                cursor: pointer;
              }
            }
            .active {
              background: url("../../assets/img/优惠券选中.png") !important;
              background-size: 100% 100% !important;
            }
          }
        }

        .block2 {
          margin-top: 18px;
          padding: 0 16px;

          .item {
            margin-bottom: 30px;
            display: inline-block;
            width: 25%;
            padding: 0 16px;
            /*行内块无法对齐*/
            vertical-align: top;

            .item-in {
              position: relative;
              padding: 12px 11px;
              height: 122px;
              background: url("../../assets/img/border1.png");
              background-size: 100% 100%;
              display: flex;
              display: -webkit-flex;
              flex-direction: column;
              /*区域和姓名*/

              p:nth-of-type(1) {
                color: #666666;
                border-bottom: 1px solid #DEDEDE;
                padding-bottom: 5px;

              }

              /*  收件人和电话*/

              p:nth-of-type(2) {
                font-size: 13px;
                margin-top: 5px;
                flex: 1;
              }

              /*  默认地址*/

              .default {
                position: absolute;
                top: 0;
                right: 0;
                background: #BFBFBF;
                color: white;
                opacity: 0.8;
                border-radius: 3px;
                width: 63px;
                text-align: center;
                height: 21px;
                line-height: 21px;
                font-size: 12px;
              }

              /*  设为默认*/

              .set {
                position: absolute;
                top: 0;
                right: 0;
                background: #BFBFBF;
                color: white;
                opacity: 0.8;
                border-radius: 3px;
                width: 63px;
                text-align: center;
                height: 21px;
                line-height: 21px;
                font-size: 12px;
              }

              /*  修改*/

              .alter {
                color: #2778BE;
                margin-top: 2px;
              }

              /*  选中图标*/

              .choose {
                width: 36px;
                position: absolute;
                bottom: 0;
                right: 0;
              }

              &:hover {
                background: url("../../assets/img/border2.png") !important;
                background-size: 100% 100% !important;
                cursor: pointer;

              }

            }

            .active {
              background: url("../../assets/img/border2.png") !important;
              background-size: 100% 100% !important;
            }
          }

        }

        /*显示全部地址*/

        .block3 {
          color: #2778BE;
          padding: 0 32px;

          p {
            border-bottom: 1px solid #F0F0F0;
            padding-bottom: 22px;

            &:hover {
              cursor: pointer;
            }
          }

        }

        .block4 {
          color: #333333;
          padding: 0 32px;
          font-weight: bold;
          margin: 36px 0;
        }

        .block5 {
          padding: 0 32px;
          color: #666666;

          div {
            padding-left: 24px;
            padding-right: 24px;
            padding-bottom: 20px;
            border-bottom: 1px solid #2778BE;
            display: flex;
            /*产品信息*/

            p:nth-of-type(1) {
              flex: 3;
            }

            /*产品属性*/

            p:nth-of-type(2) {
              flex: 3;
            }

            /*单价*/

            p:nth-of-type(3) {
              flex: 1.5;
              text-align: center;

            }

            /*数量*/

            p:nth-of-type(4) {
              flex: 1.5;
              text-align: center;

            }

            /*小计*/

            p:nth-of-type(5) {
              margin-left: 30px;
              width: 60px;
              flex-shrink: 0;
              text-align: center;
            }

          }
        }

        .block6 {
          padding: 0 32px;

          .shopItem {
            border: 1px solid #DFEBFF;
            margin-top: 29px;

            .name {
              padding: 0 24px;
              display: flex;
              height: 49px;
              align-items: center;
              color: #666666;

              img {
                width: 14px;
                height: 13px;
                margin-right: 10px;
              }
            }

            .goodCont {
              padding: 0 24px;
              background: #F3F7FF;

              /*每一个商品项目*/
              div {
                .d2{

                  margin-top: 20px;
                }

                .d1{
                padding-top: 20px;
                display: flex;
                color: #333333;
                &:last-child {
                  padding-bottom: 20px;
                }
                /*产品信息*/
                p:nth-of-type(1) {
                  flex: 3;
                  display: flex;
                  img {
                    width: 90px;
                    height: 90px;
                    margin-right: 10px;
                  }
                  span {
                    margin-right: 41px;
                  }
                }
                /*产品属性*/
                p:nth-of-type(2) {
                  flex: 3;
                  color: #999999;
                }
                /*单价*/
                p:nth-of-type(3) {
                  flex: 1.5;
                  text-align: center;
                }
                /*数量*/
                p:nth-of-type(4) {
                  flex: 1.5;
                  text-align: center;
                }
                /*小计*/
                p:nth-of-type(5) {
                  margin-left: 30px;
                  width: 60px;
                  flex-shrink: 0;
                  text-align: center;
                }
                }
              }
            }
          }
        }

        .block7 {
          padding: 0 32px;
          color: #666666;
          margin-top: 31px;
          box-sizing: border-box;

          div {
            padding-left: 24px;
            padding-right: 24px;
            display: flex;
            /*产品信息*/

            p:nth-of-type(1) {
              flex: 1;
              text-align: right;
            }

            /*小计*/

            p:last-of-type {
              margin-left: 50px;
              flex-shrink: 0;
              width: 120px;
            }

          }
        }

        .block8 {
          padding: 0 32px;
          color: #666666;

          p {
            background: #F3F3F3;
            text-align: right;
            padding-right: 31px;
            padding-bottom: 5px;
          }

          p:last-of-type {
            padding-bottom: 15px;
          }
        }

        .block9 {
          padding: 0 32px;
          display: flex;
          justify-content: flex-end;
          align-items: center;
          height: 102px;

          p:nth-of-type(1) {
            display: flex;
            align-items: center;
            margin-right: 31px;
            color: #2778BE;

            img {
              width: 16px;
              margin-right: 3px;
            }

            &:hover {
              cursor: pointer;
            }
          }

          /*提交按钮*/

          p:nth-of-type(2) {
            width: 126px;
            height: 42px;
            background: #E3393C;
            color: white;
            font-size: 16px;
            display: flex;
            align-items: center;
            justify-content: center;
            border-radius: 4px;
            margin-right: 20px;

            &:hover {
              cursor: pointer;
            }
          }
        }
      }

    }
  }

  .active {

  }

  .showHover {
    display: block !important;

  }

</style>
