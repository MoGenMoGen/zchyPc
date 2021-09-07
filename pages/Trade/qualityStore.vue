<template>
<!--  优质店铺-->
  <div id="home">

    <div  class="cont" :style="{width:width+'px'}">
      <div class="block1">
        <img :src="paihangbang">
      </div>
<!--      前三名-->
      <div class="block2">
        <ul class="block2-ul">
          <li class="block2-li" v-for="(item,index) in list" :key="index" :style="{width:(width-10)/2+'px'}">
            <div class="left" >
              <img :src="bg1" v-if="index % 4==1" class="bg">
              <img :src="bg2" v-if="index % 4==2" class="bg">
              <img :src="bg3" v-if="index % 4==3" class="bg">
              <img :src="bg4" v-if="index % 4==0" class="bg">
              <div>
                <img :src="item.shopImg" class="icon">
                <p>{{item.nm}}</p>
                <p>{{item.follow}}人关注</p>
                <p @click="toShipDetail(item.id)">进店看看</p>
              </div>
            </div>
            <div class="right">
              <ul>
                <li v-for="item in item.items">
                  <img :src="item.img" @click="toProDetail(item)">
                  <p>{{item.nm}}</p>
                  <p v-if="item.origPrice!=price && item.origPrice">￥{{item.origPrice}}</p>
                  <p v-else>价格面议</p>
                </li>
              </ul>
            </div>
          </li>

        </ul>
      </div>


    </div>

  </div>

</template>

<script>
    import {mapState} from "vuex";
    import paihangbang from '@/assets/img/onlineTrade/优质店铺.png'
    import bg1 from '@/assets/img/onlineTrade/店铺背景-1.png'
    import bg2 from '@/assets/img/onlineTrade/店铺背景-2.png'
    import bg3 from '@/assets/img/onlineTrade/店铺背景-3.png'
    import bg4 from '@/assets/img/onlineTrade/店铺背景-4.png'

    export default {
        name: "ranking",
        data(){
            return{
                paihangbang,
                bg1,
                bg2,
                bg3,
                bg4,
                list:[],
                shopList:[
                    {
                        id:1,
                        nm:'商品1',
                        price:255,
                        img:'https://jl-dev.oss-cn-shanghai.aliyuncs.com/1f50162fc9064f43a2b1cd92179a61d8.jpg'
                    },
                    {
                        id:2,
                        nm:'商品2',
                        price:255,
                        img:'https://jl-dev.oss-cn-shanghai.aliyuncs.com/1f50162fc9064f43a2b1cd92179a61d8.jpg'
                    },
                    {
                        id:3,
                        nm:'商品3',
                        price:255,
                        img:'https://jl-dev.oss-cn-shanghai.aliyuncs.com/1f50162fc9064f43a2b1cd92179a61d8.jpg'
                    }
                ]

            }
        },
        mounted(){
          this.getList()
        },
        methods:{
          async getList(){
             this.list=await this.api.shopGoodStore()
             console.log(this.list)
          },
          //跳转产品详情
          toProDetail(item) {
            let url = '../sinovat/Trade/productDetail?id='+item.goodsId + "&type=product"
            window.open(url)
          },
          //跳转店铺详情
          toShipDetail(id) {
            window.open('../sinovat/Trade/storeHome?shopId='+id)
          },
        },
        computed:{
            ...mapState([
                'width',
                'price'
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
  }
.cont{
  margin: 0 auto;
  .block1{
    img{
      display: block;
      margin: 0 auto;
      width:100%;
    }
  }
  .block2{
    .block2-ul{
      display: flex;
      flex-wrap: wrap;
      .block2-li{
        margin-bottom: 30px;
        width: 595px;
        height: 225px;
        display: flex;
        background: white;
        .left{
          width: 28%;
          flex-shrink: 0;
          height: 100%;
          position: relative;

          .bg{
            top:0;
            left: 0;
            right: 0;
            bottom: 0;
           /*width: 100%;*/
            height: 100%;
            position: absolute;
            z-index:1;
          }
          div{
            color: white;
            position: absolute;

            top:0;
            left: 0;
            right: 0;
            bottom: 0;
            z-index: 2;
            img{
              z-index:3;
              display: block;
              width: 65px;
              height: 65px;
              border-radius: 3px;
              margin: 22px auto;
            }
            p:nth-of-type(1){
              font-size: 16px;
              text-align: center;

            }
            p:nth-of-type(2){
              margin-top: 10px;
              text-align: center;

            }
            p:nth-of-type(3){
              margin: 10px auto;
              width:81px;
              height:26px;
              border:1px solid rgba(255, 255, 255, 1);
              display: flex;
              align-items: center;
              justify-content: center;
              &:hover{
                cursor: pointer;
              }
              &:active{
                opacity: 0.5;
              }
            }
          }
        }
        .right{
          width: 100%;
          ul{
            width: 100%;
            display: flex;
            flex-wrap: nowrap;
            li{
              flex:1;
              flex-shrink: 0;
              padding-top: 42px;
              img{
                display: block;
                width: 100px;
                height: 100px;
                margin: 0 auto;
                cursor: pointer;
              }
              p:nth-of-type(1){
                margin:0 auto ;
                margin-top: 13px;
                text-align: center;
                color: #999999;
                width: 100%;
                height: 30px;
                line-height: 30px;
                overflow: hidden;
                text-overflow: ellipsis;
                white-space: nowrap;
                max-width: 120px;
              }
              p:nth-of-type(2){
                text-align: center;
                margin-top: 5px;
                color: #E4393C;

              }
            }
          }
        }


      &:nth-child(2n+1){
        margin-right: 10px;
      }

      }
    }

  }

}
</style>
