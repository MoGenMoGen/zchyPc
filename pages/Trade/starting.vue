<template>
<!--  新品首发-->
  <div id="home">
    <div class="cont0">
      <img :src="bg">
      <div class="place"></div>
      <div class="lunbo">
        <el-carousel height="500px">
          <el-carousel-item v-for="item in list" :key="item.id">
            <div class="carousel-item">
              <img :src="item.img">
              <div>
                <p>{{item.nm}}</p>
                <p v-if="item.origPrice!=price"><sup>￥</sup><span>{{item.origPrice}}</span></p>
                <p v-else><span>价格面议</span></p>
                <p @click="toProDetail(item)">立即购买</p>
<!--                <el-button type="primary" round>主要按钮</el-button>-->
              </div>
            </div>
          </el-carousel-item>
        </el-carousel>
      </div>
    </div>

    <div  class="cont" :style="{width:width+'px'}">
      <div class="block1">
        <img :src="tuijian">
      </div>
<!--      前三名-->
      <div class="block2">
        <ul>
          <li v-for="(item,index) in list" :key="index" >
            <img :src="item.img" class="pre">
            <p>{{item.nm}}</p>
            <p v-if="item.origPrice!=price"><sup>￥</sup><span>{{item.origPrice}}</span></p>
            <p v-else><span>价格面议</span></p>
            <img :src="lijigoumai" class="lijigoumai" @click="toProDetail(item)">
          </li>

          <li v-for="(item,index) in 5-list.length % 5" :key="index" v-if="list.length % 5!=0" style="visibility: hidden">

          </li>

        </ul>
      </div>


    </div>

  </div>

</template>

<script>
    import {mapState} from "vuex";
    import paihangbang from '@/assets/img/onlineTrade/优惠券标题.png'
    import bg from '@/assets/img/onlineTrade/新品首发背景.png'
    import tuijian from '@/assets/img/onlineTrade/上新推荐修饰.png'
    import lijigoumai from '@/assets/img/onlineTrade/立即购买.png'
    export default {
        name: "ranking",
        data(){
            return{
                paihangbang,
                bg,
                tuijian,
                lijigoumai,
                list:[],


            }
        },
        mounted(){
          this.getList()
        },
        methods:{
          async getList(){
             this.list=await this.api.shopNewPro()
          },
          //跳转产品详情
          toProDetail(item) {
            let url = '../sinovat2/Trade/productDetail?id='+item.goodsId + "&type=product"
            window.open(url)
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
  .cont0{
    width: 100%;
    position: relative;
    height: 610px;
    img{

      width: 100%;
      height: 400px;
      display: block;
      ;
    }
    .place{
      width: 100%;
      height: 210px;
    }
    .lunbo{
      width: 850px;
      height: 500px;
      z-index: 20;
      position: absolute;
      top:110px;
      left:50%;
      transform: translateX(-50%);
      .carousel-item{
        display: flex;
        height: 100%;
        img{
          display: block;
          width: 550px;
          height: 100%;
        }
        div{
          background: white;
          width: 300px;
          padding: 101px 22px;
          p:nth-of-type(1){
            text-align: center;
            font-size: 28px;
            line-height: 28px;
            color: #333333;
            cursor:pointer;
          }
          p:nth-of-type(2){
            cursor:pointer;
            margin-top: 54px;
            text-align: center;
            color: #E4393C;
            sup:nth-of-type(1){
             font-size: 19px;
              vertical-align: baseline;
            }
            span{
              font-size: 42px;

            }
            sup:nth-of-type(2){
              font-size: 30px;
              vertical-align: baseline;
            }
          }
          p:nth-of-type(3){
            cursor:pointer;
            width:179px;
            height:52px;
            background:rgba(39,120,190,1);
            border-radius:26px;
            color: white;
            display: flex;
            align-items: center;
            justify-content: center;
            margin: 0 auto;
            margin-top: 120px;
            font-size: 24px;
          }
        }
      }

    }

  }

.cont{
  margin: 0 auto;
  .block1{
    padding:40px 0;
    img{
      display: block;
      margin: 0 auto;
      width: 220px;

    }
  }
  .block2{
    ul{
      display: flex;
      flex-wrap: wrap;
      justify-content: space-between;
      li{
        cursor:pointer;
        box-sizing: border-box;
        margin-bottom: 20px;
        width:232px;
        position: relative;
        background-size: 100% 100%;
        height: 343px;
        padding: 20px 20px 30px 20px;
        background: white;
        p:nth-of-type(1){
          margin-top: 29px;
          color: #666666;
          height: 34px;
        }
        p:nth-of-type(2){
          margin-top: 15px;
          color: #E4393C;
          sup:nth-of-type(1){
            font-size: 14px;
            vertical-align: baseline;
          }
          span{
            font-size: 24px;
          }
          sup:nth-of-type(2){
            font-size: 18px;
            vertical-align: baseline;
          }
        }
        .pre{
          width: 192px;
          height: 192px;
          display: block;
          margin:0 auto;
        }
        .lijigoumai{
          position: absolute;
          right: -2px;
          bottom: 21px;
          width: 81px;
        }


      }
    }

  }

}
</style>
