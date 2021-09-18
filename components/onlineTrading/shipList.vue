<template>
<div id="app">
  <div class="mainView">
    <div class="ship" v-for="(item,index) in list" :key="index" >
      <!--左一图片-->
      <div class="left1">
        <div class="img" @click="toDetail(item.goodsId)">
          <span>
              <img :src="item.img" alt=""/>
          </span>
        </div>
      </div>
      <!--左二名称/属性-->
      <div class="left2">
        <p>{{item.nm}}</p>
        <img :src="item.used==1 ? tp1 : tp2" alt=""/>
         <img :src="VRImg" style="margin-top: 10px;cursor: pointer;" >
      </div>
      <!--中部船舶属性-->
      <div class="mid">
        <p><span>船舶总长：</span>{{item.totalLen}}m</p>
        <p><span>公约船长：</span>{{item.pactLen}}m</p>
        <p><span>型宽：</span>{{item.width}}m</p>
        <p><span>型深：</span>{{item.deep}}m</p>
      </div>
      <!--右一价格-->
      <div class="right1">
        <p v-if="item.origPrice==price">参考价：<span class="textRed">价格面议</span></p>
        <p v-else>参考价：<span class="textRed">￥</span><span class="textRed">{{item.origPrice}}</span>/艘</p>
      </div>
      <!--右二立即下单-->
      <div class="right2">
        <button @click="toDetail(item.goodsId)">立即下单</button>
      </div>
    </div>
  </div>
</div>
</template>

<script>
  import img1 from "../../assets/img/homeSZ1.png";
  import {mapState} from "vuex";
  import tp1 from '@/assets/img/onlineTrade/新造船.png'
  import tp2 from '@/assets/img/onlineTrade/二手船.png'
  import VRImg from '../../assets/img/personal/VR.png'
  export default {
      name: "shipList",
      props:['list'],
      computed:{
        ...mapState([
          'width',
          'shipSize',
          'price'
        ])
      },
      data(){
          return{
            VRImg,
            tp1,
            tp2,
            shipList1:[{
              imh:img1,nm:'金枪鱼延绳钓渔船 隆兴801',tp:tp1,tl:'90m',ml:'80m',wd:'12m',hd:'6m',pr:'120,0000.00'
            },{
              imh:img1,nm:'金枪鱼延绳钓渔船 隆兴801',tp:tp2,tl:'90m',ml:'80m',wd:'12m',hd:'6m',pr:'120,0000.00'
            },{
              imh:img1,nm:'金枪鱼延绳钓渔船 隆兴801',tp:tp1,tl:'90m',ml:'80m',wd:'12m',hd:'6m',pr:'120,0000.00'
            },{
              imh:img1,nm:'金枪鱼延绳钓渔船 隆兴801',tp:tp2,tl:'90m',ml:'80m',wd:'12m',hd:'6m',pr:'120,0000.00'
            },{
              imh:img1,nm:'金枪鱼延绳钓渔船 隆兴801',tp:tp1,tl:'90m',ml:'80m',wd:'12m',hd:'6m',pr:'120,0000.00'
            },{
              imh:img1,nm:'金枪鱼延绳钓渔船 隆兴801',tp:tp2,tl:'90m',ml:'80m',wd:'12m',hd:'6m',pr:'120,0000.00'
            }],
          }
      },
      methods:{
        toDetail(id){
          window.open('./Trade/productDetail?type=ship&id='+id)
          // this.$router.push('/Trade/productDetail?type=ship&id='+id)
        }
      }
    }
</script>

<style lang="less" scoped>
  @import url("../../assets/css/init.less");
#app{

}
  .mainView{
    display: flex;
    margin-top: 10px;
    width: 100%;
    flex-wrap: wrap;
    border: 1px solid #F3F3F3;
    .ship{
      width: 100%;
      border-bottom: 1px solid #F3F3F3;
      padding: 20px 30px;
      display: inline-flex;
      align-items: center;
      .left1{
        width: 28.4%;
        margin-right: 20px;
        .img{
          display: flex;
          display: -webkit-flex;
          cursor: pointer;
          >span{
            margin: auto;
          }
          img{
            border: 1px solid #F3F3F3;
          }
        }

      }
      .left2{
        display: flex;
        flex-wrap: wrap;
        justify-content: center;
        flex-direction: column;
        width: 25%;
        p:nth-child(1){
          line-height: 30px;
          font-size: 16px;
          color: #333333;
        }
        >img{
          width: 80px;
        }
      }
      .mid{
        width: 16.7%;
        display: flex;
        flex-wrap: wrap;
        justify-content: center;
        flex-direction: column;
        >p{
          height: 27px;
          color: #999999;
          display: inline-flex;
          justify-content: space-between;
          width: 135px;
        }
      }
      .right1{
        width: 18%;
        display: flex;
        align-items: center;
        .textRed{
          color: #FF3E3E;
        }
        >p{
          color: #999999;
          font-size: 14px;
          line-height: 21px;
          >span:nth-child(1){
            font-size: 8px;
          }
        }
      }
      .right2{
        width: 11.8%;
        display: flex;
        justify-content: center;
        align-items: center;
        >button{
          background-color: #2778BE;
          color: #FFFFFF;
          font-size: 16px;
          border-radius: 5px;
          width: 120px;
          height: 40px;
          border: 0;
          cursor: pointer;
        }
      }
    }
  }
</style>
