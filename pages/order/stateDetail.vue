<template>
  <!-- 对账单详情 -->
  <div id="home">
      <div class="cont">
        <div class="line1">
          <span class="point" @click="toPage('/')">首页&nbsp;&nbsp;>&nbsp;&nbsp;</span>
          <span class="point" @click="toPage('../personal/')">个人中心&nbsp;&nbsp;>&nbsp;&nbsp;</span>
          <span class="point" @click="toPage('../personal/withMoney')">往来款项&nbsp;&nbsp;>&nbsp;&nbsp;</span>
          <span class="red">对账单详情</span>
        </div>
        <div class="tableOrder">
         <div class="orderTitle">
           <p class="firstP" style="text-align: start;">产品详情</p><p>单价</p><p>数量</p>
           <p>小计</p>
         </div>
         <div class="orderBox" v-for="item in info.itms">
            <p class="firstP" style="text-align: start;">
              <img :src="item.goodsImgUrl">
              <span style="color: #333333;margin-left: 10px;">{{item.goodsNm}}</span>
              <span style="color: #999999;">{{item.goodsSkuAttrNm}}</span>
            </p>
            <p>{{item.type==1?'-':''}}{{item.unitPrice}}</p><p>{{item.qty}}</p><p>{{item.type==1?'-':''}}{{item.totalPrice}}</p>
         </div>
         <div class="totalFooter" v-if="dataShow">
           <p style="text-align: end;">应收金额：<span>￥{{info.total?info.total:0}}</span> </p>
           <p style="text-align: end;">已收金额：<span style="color: #2778BD;">￥{{info.payment?info.payment:0}}</span></p>
           <p style="text-align: end;">未收金额：<span style="color: #E3393B;">￥{{info.uncollected?info.uncollected:0}}</span></p>
         </div>
        </div>

      </div>
  </div>
</template>

<script>

import {mapState} from "vuex";
import steps from "../../components/steps/steps";
import mO from "../../assets/img/personal/我的订单.png"
export default {
  head () {
    return {
      title: '对账单详情',
    }
  },
  data(){
    return{
      id:"",
      info:{},
      dataShow:false,
    }
  },
  computed: {
    ...mapState([
      'bWidth',
      'width'
    ])
  },
  components: {
      steps,

  },
  mounted(){
    this.id = this.until.getQueryString('id')
    this.getList()
  },
  methods:{
    async getList(){
      this.info=await this.api.statDetail(this.id)
      if(this.info.itms.length>0){
        this.dataShow=true
      }
      console.log('对账单详情')
      console.log(this.info)
    },
    toPage(url){
      if(url){
        this.$router.push(url)
      }
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
      .tableOrder{
        width: 100%;
        padding: 31px;
        background-color: #ffffff;
        .orderTitle,.orderBox{
          display: flex;
          justify-content: space-between;
          >p{
            text-align: center;
            flex: 1;
          }
          .firstP{
            flex: 3;
            padding-left: 24px;
          }
        }
        .orderTitle{
            background-color: #F7F7F7;
            font-size: 16px;
            color: #666666;
            padding: 22px 0;
        }
        .orderBox{
          padding: 20px 0;
          border-bottom: 1px solid #E8E8E8;
          .firstP{
            display: flex;
            >span{
              margin-left: 30px;
              flex: 1;
            }
            >img{
              width: 90px;
            }
          }
        }
        .totalFooter{
          margin-top: 24px;
          padding-right: 9px;
          >p{
            font-size: 20px;
            color: #656565;
            margin-bottom: 12px;
            >span{
              // margin-left: 20px;
              width: 160px;
              display: inline-block;
            }
          }
        }
      }

    }

  }
  .totalAmout{
    font-size: 24px;
  }
</style>
