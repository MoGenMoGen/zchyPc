<template>
<!--  支付成功页面-->
  <div id="home" :style="{width:bWidth+'px'}">
    <div class="cont" :style="{width:width+'px'}">
      <div class="line1">
        <p><img :src="successLog">您已成功付款</p>
      </div>
      <div class="line2">
        <p><span>收货地址：</span><span>浙江省宁波市镇海区骆驼街道329创业社区&nbsp;&nbsp;&nbsp;&nbsp;</span> <span>&nbsp;&nbsp;收货人：</span><span>王小二 &nbsp;&nbsp; 18888888888</span></p>
        <p ><span>订单编号：</span><span>11515151511515</span></p>
        <p ><span>商品名称：</span><span>新型中创休闲渔船</span></p>
        <p ><span>交易金额：</span><span>￥120000</span></p>
        <p ><span>支付时间：</span><span>2020年04月15日 15:25:de</span></p>
        <div class="tips">您可以 <span>查看其它商品</span> <span>查看订单详情</span></div>
      </div>
      <div class="line3">
        <div class="box">
          <div class="item" v-for="item in tabList" :key="item.id" @click="toChooseTab(item)" >
            <p :class="{active:item.id==selectedId}" > {{item.nm}}</p>
          </div>
        </div>


      </div>
      <product-list></product-list>
    </div>
  </div>
</template>

<script>
  import {mapState} from "vuex";
   import successLog from '../../assets/img/success.png'
   import productList from "../../components/onlineTrading/productList";
    export default {
        name: "success",
        components:{
            productList
        },
        computed:{
          ...mapState([
            'bWidth',
            'width'
          ]),

        },
        methods:{
            //选中猜你喜欢标签
            toChooseTab(item){
                if(item.id!=this.selectedId){
                    this.selectedId=item.id
                }
            },
        },
        data(){
            return{
                successLog,
                tabList:[
                    {
                        id: 2,
                        nm: '猜你喜欢'
                    },
                    {
                        id: 3,
                        nm: '猜你喜欢3'
                    },
                    {
                        id: 1,
                        nm: '猜你喜欢33'
                    },
                ],
                selectedId:1,//选中tab
            }
        }
    }
</script>

<style scoped lang="less">

  #home {
    background: #F3F3F3;
    box-shadow:0px 6px 10px 0px rgba(13,4,8,0.22) inset;
    padding-top: 20px;
    .cont {
      margin: 0 auto;
      .line1{
        background: white;
        height: 95px;
        width: 100%;
        display: flex;
        align-items: center;
        padding-left: 53px;
        border-bottom: 1px solid #E8E8E8;
        p{
          display: flex;
          align-items: center;
          color: #404040;
          font-size: 18px;
          font-weight: bold;
          img{
            width: 28px;
            margin-right: 18px;
          }
        }
      }
      .line2{
        background: white;
        padding:30px 53px;
        p{
          margin-bottom: 20px;
          span:nth-of-type(1){
            font-weight: bold;
            color: #333333;
          }
        }
        .tips{
          margin-top: 50px;
          span{
            margin-left: 20px;
            border-bottom: 1px solid #2778BE;
            color: #2778BE;
            &:hover{
              cursor: pointer;
            }
          }
        }
      }
      .line3{
        margin-top: 16px;margin-bottom: 16px;margin-left: 21px;

        .box{
          display: flex;
          width: 100%;
          color: #333333;
          .item{
            padding:  0 10px;
            p{
              padding: 10px 0;
            }
            &:hover{
              color: #E64347;
              cursor: pointer;

            }
          }
        }

        .active{
          color: #E64347 !important;
          border-bottom: 2px solid #E64347;
        }
      }



    }
  }
</style>
