<template>
  <div class="home" ref="tradePrice">
    <!--下部具体内容-->
    <div class="specific">
      <!--左边广告图内容-->
      <div class="leftBanner" v-if="list.length>0" @click="toProDetail(list[0])">
        <div class="img" :style="{height:(width*0.3333-20)*proSize+'px'}">
          <img :src="list[0].img" :alt="list[0].shopNm"/>
        </div>
        <p style="font-size: 12px">{{list[0].nm}}</p>
        <p class="textRed" v-if="list[0].goodsMinPrice!=price && list[0].goodsMinPrice !== list[0].goodsMaxPrice">￥{{list[0].goodsMinPrice}}-{{list[0].goodsMaxPrice}}</p>
        <p class="textRed" v-if="list[0].goodsMinPrice!=price && list[0].goodsMinPrice === list[0].goodsMaxPrice">￥{{list[0].goodsMinPrice}}</p>
        <p class="textRed" v-if="list[0].goodsMinPrice==price">价格面议</p>

      </div>
      <!--右部四个小图-->
      <div class="rightBanner">
        <div class="productMain" v-for="(item,index) in list" v-if="index>0 && index < 5" @click="toProDetail(item)">
          <div class="leftPart" style="height:100%">
            <span><img class="pImg" :src="item.img" alt=""/></span>
          </div>
          <div class="rightPart">
            <p style="font-size: 12px ;margin-right: 3px">{{item.nm}}</p>
            <p class="textRed" v-if="item.goodsMinPrice!=price && item.goodsMinPrice!==item.goodsMaxPrice">￥{{item.goodsMinPrice}}-{{item.goodsMaxPrice}}</p>
            <p class="textRed" v-if="item.goodsMinPrice!=price && item.goodsMinPrice===item.goodsMaxPrice">￥{{item.goodsMinPrice}}</p>
            <p class="textRed" v-if="item.goodsMinPrice==price">价格面议</p>
            <p>热销中</p>
          </div>
        </div>
      </div>
    </div>
  </div>

</template>

<script>
  import {mapState} from "vuex";
    export default {
      name: "tradePrice",
      props:['list'],
      data(){
        return{
          width:500,
        }
      },
      computed: {
        ...mapState([
          'proSize',
          'price'
        ]),
      },
      mounted(){
        this.width = this.$refs.tradePrice.offsetWidth
      },
      methods:{
        toProDetail(item){
          // this.$router.push({
          //   path:'../Trade/productDetail',
          //   query:{
          //     id:item.goodsId,
          //     type:item.ship?'ship':'product'
          //   }
          // })

          let url = '../sinovat/Trade/productDetail?id='+item.goodsId + "&type=" + (item.ship ? 'ship' : 'product')
          window.open(url)
        }
      }

    }
</script>

<style lang="less" scoped>
  @import url("../../assets/css/init.less");
  .home{
    height: 100%;
    width: 100%;
    .specific{
      display: inline-flex;
      flex-wrap: nowrap;
      width: 100%;
    //左部大图
      .leftBanner{
        display: flex;
        flex-wrap: wrap;
        justify-content: space-around;
        width: 32.5%;
        cursor: pointer;
        padding-left: 15px;
        box-sizing: border-box;
        .img{
          width: 100%;
          margin-bottom: 10px;
          display: flex;
          display: -webkit-flex;
          img{
            margin: auto;
          }
        }
        >p{
          width: 100%;
          text-align: center;
          font-size: 16px;
          display: -webkit-box;
          -webkit-box-orient: vertical;
          overflow: hidden;
          padding-bottom: 5px;
        }
      //红色字体
        .textRed{
          color: #FF3E3E;
        }
      }
    //右部四个小图
      .rightBanner{
        width: 66%;
        margin-left: 1.5%;
        display: flex;
        flex-wrap: wrap;
        cursor: pointer;
      //具体商品栏
        .productMain{
          width: 48.5%;
          height: 50%;
          display: flex;
          &:nth-of-type(2n+1){
            margin-right: 3%;
          }
        //商品栏左部分
          .leftPart{
            width: 100%;
            display: flex;
            display: -webkit-flex;
            align-items: center;
            justify-content: center;
            span{
              width: 100%;
              height: auto;
              img{
                max-width: 100%;
                max-height: 100%;
              }
            }
            .pImg{
              width: auto;
              height: auto;
              max-height: 100%;
              max-width: 100%;
            }

          }
        //商品栏右部分
          .rightPart{
            display: flex;
            justify-content: space-between;
            flex-direction: column;
            padding-left: 10px;
            padding-bottom: 15px;
            >p{
              width: 84px;
              color: #666666;
              padding-bottom: 12px;
              display: -webkit-box;
            }
            .textRed{
              font-size: 16px;
              color: #FF3E3E;
            }
          }
        }
      }
    }
  }


</style>
