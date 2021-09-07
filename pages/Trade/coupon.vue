<template>

  <div id="home">

    <div  class="cont" :style="{width:width+'px'}">
      <div class="block1">
        <img :src="title">
      </div>
      <div class="block2">
          <ul>
            <li v-for="(item,index) in list" :key="index" @click="receive(item)" :class="item.isReceive === 1?'ok':'bg'">
              <div>
                <img :src="item.img">
              </div>
              <div v-if="item.discType === 0">
                <p><sup>￥</sup><span>{{item.discAmt}}</span><span>满{{item.payAmt}}元可用</span></p>
                <p v-if="item.couponType === 1" class="p1">限购[<span>{{item.shopNm}}</span>]店内产品</p>
                <p v-if="item.couponType === 0" class="p1">平台券</p>
                <p v-if="item.useType === 0" class="p1">全场类</p>
                <p v-if="item.useType === 2" class="p1">限定商品</p>
              </div>
              <div v-if="item.discType === 1">
                <p><sup></sup><span>{{item.discRate*10}}折</span><span>折扣券</span></p>
                <p v-if="item.couponType === 1" class="p1">限购[<span>{{item.shopNm}}</span>]店内产品</p>
                <p v-if="item.couponType === 0" class="p1">平台券</p>
                <p v-if="item.useType === 0" class="p1">全场类</p>
                <p v-if="item.useType === 2" class="p1">限定商品</p>
              </div>
              <div v-if="item.discType === 2">
                <p><sup>￥</sup><span>{{item.discAmt}}</span><span>代金券</span></p>
                <p v-if="item.couponType === 1" class="p1">限购[<span>{{item.shopNm}}</span>]店内产品</p>
                <p v-if="item.couponType === 0" class="p1">平台券</p>
                <p v-if="item.useType === 0" class="p1">全场类</p>
                <p v-if="item.useType === 2" class="p1">限定商品</p>
              </div>
            </li>
          </ul>
      </div>
    </div>

  </div>

</template>

<script>
    import {mapState} from "vuex";
    //import paihangbang from '@/assets/img/onlineTrade/优惠券标题.png'
    import title from '../../assets/img/onlineTrade/优惠券标题.png'


    export default {
        name: "ranking",
        data(){
            return{
              title,
                list:[],
            }
        },
        mounted(){
          this.getList()
        },
        methods:{
          async getList(){
             this.list=await this.api.couponListSpe()
            console.log(this.list)
          },
          //领取优惠券
          receive(item){
            //已领取跳转
            if (item.isReceive === 1){
              window.open('./Trade/onlinetrading')
            }else {
              this.api.receiveCoupon(item.id).then((res) => {
                if (res.code === 0){
                  this.$message({
                    message: '领取成功',
                    type: 'success'
                  });
                  this.getList()
                }
              })
            }




          }


        },
        computed:{
            ...mapState([
                'width',
            ]),
        }
    }
</script>

<style scoped lang="less">
  #home{
    background-color: #F3F3F3;
    background-image: url("../../assets/img/bg.png");
    background-position: center bottom;
    background-repeat: repeat-x;
    padding-bottom: 120px;
    .p1{
      font-size: 12px;
      color: #e01010;
      display: -webkit-box;
      -webkit-box-orient: vertical;
      overflow: hidden;
    }
  }

.cont{
  margin: 0 auto;
  .block1{
    padding:30px 0;
    img{
      display: block;
      margin: 0 auto;
      width: 250px;

    }
  }
  .bg {
    background: url("../../assets/img/onlineTrade/优惠券.png");
  }
  .ok {
    background: url("../../assets/img/onlineTrade/优惠券已领取.png");
  }
  .block2{
    ul{
      display: flex;
      flex-wrap: wrap;
      justify-content: space-between;
      li{
        margin-bottom: 30px;
        width: 32%;
        position: relative;
        display: flex;
        //background: url("../../assets/img/onlineTrade/优惠券.png");
        //background: url("../../assets/img/onlineTrade/优惠券已领取.png");
        background-size: 100% 100%;
        height: 160px;
        div:nth-of-type(1){
          padding: 20px 5px 20px 20px;
          flex-shrink: 0;
          img{
            width: 110px;
            height: 110px;
            border:1px solid rgba(237, 237, 237, 1);
            /*padding: 20px;*/

          }
        }
        div:nth-of-type(2){
          padding: 24px 50px 14px 0px;
          width: 100%;
          p:nth-of-type(1){
            height: 40%;
            width: 100%;
            color: #E4393C;
            sup{
              font-size: 14px;
              vertical-align:top;
            }
            span:nth-of-type(1){
              font-size: 36px;
            }
            span:nth-of-type(2){
              width:69px;
              height:11px;
              font-size:12px;
              font-family:Microsoft YaHei;
              font-weight:400;
              color:rgba(228,57,60,1);
              line-height:18px;
              background: #FFF4EC;
              padding: 5px;
              margin-left: 5px;
            }

          }
          p:nth-of-type(2){
            display: flex;
            align-items: center;
            color: #333333;


          }
        }

      }
    }

  }

}
</style>
