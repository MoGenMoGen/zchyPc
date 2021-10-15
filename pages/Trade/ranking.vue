<template>
<!--  排行榜-->
  <div id="home">

    <div  class="cont" :style="{width:width+'px'}">
      <div class="block1">
        <img :src="paihangbang">
      </div>
<!--      前三名-->
      <div class="block2">
        <ul>
          <li v-for="(item,index) in list.slice(0,3)" :key="index" >
            <p v-if="index===0" class="rank">
              <img :src="one"  >
              <span> {{index+1}}</span>
            </p>
            <p v-if="index===1" class="rank">
              <img :src="two"  >
              <span> {{index+1}}</span>
            </p>
            <p v-if="index===2" class="rank">
              <img :src="three"  >
              <span> {{index+1}}</span>
            </p>
            <div>
              <img :src="item.img"  @click="toProDetail(item)">
            </div>
            <div>
              <p>{{item.nm}}</p>
              <p v-if="item.origPrice!=price">￥<span>{{item.origPrice}}</span></p>
              <p v-else><span>价格面议</span></p>
            </div>
          </li>

        </ul>
      </div>
    <!-- 第四名开始-->
      <div class="block3">
        <ul>
          <li v-for="(item,index) in list.slice(4,29)" :key="index" >
            <p  class="rank">
              <img :src="other"  >
              <span> {{index+4}}</span>
            </p>
            <div>
              <img :src="item.img" @click="toProDetail(item)">
            </div>
            <div>
              <p>{{item.nm}}</p>
              <p>￥<span>{{item.origPrice}}</span></p>
            </div>
          </li>

        </ul>
      </div>

    </div>

  </div>

</template>

<script>
    import {mapState} from "vuex";
    import paihangbang from '@/assets/img/onlineTrade/热卖榜.png'
    import one from '@/assets/img/onlineTrade/第一.png'
    import two from '@/assets/img/onlineTrade/第二.png'
    import three from '@/assets/img/onlineTrade/第三.png'
    import other from '@/assets/img/onlineTrade/其余.png'
    export default {
        name: "ranking",
        data(){
            return{
                paihangbang,
                one,
                two,
                three,
                other,
                list:[],
                list2:[],
            }
        },
        mounted(){
          this.getList()
        },
        methods:{

          //跳转产品详情
          toProDetail(item) {
            let url = '../sinovat2/Trade/productDetail?id='+item.goodsId + "&type=product"
            window.open(url)
          },
          async getList(){
             this.list=await this.api.goodsRank30()
          }
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
      width: 300px;
    }
  }
  .block2{
    ul{
      display: flex;

      li{

        flex:1;
        position: relative;
        display: flex;
        background: white;
        padding-top: 59px;

        .rank{
          position: absolute;
          left: -5px;
          top: 15px;
          width: 40px;
          height: 24px;
          color: white;
          display: flex;
          align-items: center;
          justify-content: center;
          background-size: 100% 100%;

          img{
            width: 40px;
            height: 24px;
            position: absolute;
            top:0;
            left: 0;
            right: 0;
            bottom: 0;
          }
          span{
            z-index: 2;
          }
        }
        div:nth-of-type(1){
          flex-shrink: 0;
          img{
            width: 158px;
            height: 158px;
          }
        }
        div:nth-of-type(2){
          padding: 14px;
          width: 100%;
          p:nth-of-type(1){
            height: 40%;
            width: 100%;
            border-bottom: 1px dashed rgba(11,2,5,0.1);
            color: #666666;

          }
          p:nth-of-type(2){
            height: 50%;
            color: #FF3E3E;
            display: flex;
            align-items: center;
            span{
              font-size: 24px;
            }

          }
        }
        &:nth-child(2){
          margin: 0 10px;
        }

      }
    }

  }
  .block3{
    ul{
      display: flex;
      flex-wrap: wrap;
      justify-content: space-between;
      li{
        margin-top: 20px;
        width: 232px;
        position: relative;
        background: white;
        padding: 20px 20px 20px 20px;
        /*margin-right: 10px;*/
        .rank{
          position: absolute;
          left: -5px;
          top: 15px;
          width: 40px;
          height: 24px;
          color: white;
          display: flex;
          align-items: center;
          justify-content: center;
          background-size: 100% 100%;

          img{
            width: 40px;
            height: 24px;
            position: absolute;
            top:0;
            left: 0;
            right: 0;
            bottom: 0;
          }
          span{
            z-index: 2;
          }
        }
        div:nth-of-type(1){
          flex-shrink: 0;
          img{
            display: block;
            margin:0 auto;
            width: 192px;
            height: 192px;
          }
        }
        div:nth-of-type(2){
          width: 100%;
          p:nth-of-type(1){

            width: 100%;
            margin-top: 29px;
            color: #666666;

          }
          p:nth-of-type(2){
            margin-top: 29px;
            color: #FF3E3E;
            display: flex;
            align-items: center;
            span{
              font-size: 24px;
            }

          }
        }
        /*&:nth-child(5n){*/
        /*  margin-right: 0;*/
        /*}*/
      }
    }

  }

}
</style>
