<template>
      <div >
        <!--产品精选-->
        <div :style="{width:width+'px'}" class="featured">
          <div class="featuredHeader">
            <p>产品精选</p>
          </div>
          <div class="featuredMain">
            <featured  :list="list1"></featured>
          </div>
        </div>
        <!--广告栏-->
        <div class="ad" :style="{width:width+'px'}">
          <el-carousel :height="bannerHeight+'px'" class="adert">
            <el-carousel-item v-for="item in adertList" :key="item.aiid">
              <div class="bannerImg" :style="{backgroundImage:'url('+item.imgUrl+')'}"  @click="toPage(item.locUrl)"/>
            </el-carousel-item>
          </el-carousel>
        </div>
        <!--看了又看-->
        <div :style="{width:width+'px'}" class="featured">
          <div class="featuredHeader">
            <p>看了又看 </p>
            <p @click="toChange"><i class="el-icon-refresh-right"></i>换一批</p>
          </div>
          <div class="featuredMain">
            <featured :list="list2"></featured>
          </div>
        </div>
      </div>
</template>
<script>
  import Featured from './onlineTrading/featured'
  import {mapState} from "vuex";
  export default {
    props:[],
    data(){
      return{
        list1:[],
        list2:[],
        adertList:[],
        bannerHeight:0
      }
    },
    components: {
      Featured
    },
    watch:{
      width(){
        if(this.adertList.length>0){
          let myImg = new Image()
          myImg.src = this.adertList[0].imgUrl
          myImg.onload = () =>{
            this.bannerHeight = this.width*0.5*myImg.height/myImg.width
          }
        }
      },
    },
    computed: {
      ...mapState([
        'bWidth',
        'width',
      ])
    },
    mounted(){
      this.getData()
    },
    methods:{
       async getData(){
         this.list1 = await this.api.personLook()
         this.list2 = await this.api.personLook()
         this.adertList = await this.api.getAdert('personAdert')
         if(this.adertList.length>0){
           let myImg = new Image()
           myImg.src = this.adertList[0].imgUrl
           myImg.onload = ()=>{
             this.bannerHeight = myImg.height
           }
         }
       },
      async toChange(){
        this.list2 = await this.api.personLook()
      },
      toPage(url){
        if(url){
          window.location.href = url
        }
      },
    }
  }
</script>
<style lang="less">
  @import url("../assets/css/init.less");
  .el-popover{
    width: 100px!important;
    img{
      width: 100%;
    }
  }
  /*.el-popper[x-placement^=bottom]{*/
    /*margin-top: 2px;*/
  /*}*/
  .topMenu{
    .el-button{
      padding: 0;
      border: none;
      background: none;
      font-size: 12px;
      span{
        display: flex;
        display: -webkit-flex;
        align-items: center;
        height: 38px;
        img{
          width: 18px;
          margin-left: 20px;
          margin-right: 5px;
        }
      }

    }
  }
</style>
<style lang="less" scoped>
  @import url("../assets/css/init.less");
  .featured{
    background-color: #FFFFFF;
    margin: 0 auto;
    height: 394px;
    border: 1px solid #DEDEDE;
    .featuredHeader{
      display: flex;
      display: -webkit-flex;
      >p{
        color: #343434;
        font-size: 20px;
        padding-top: 20px;
        line-height: 19px;
        &:first-of-type{
          flex: 1;
          padding-left: 20px;
        }
       &:nth-child(2){
          color: #333333;
          font-size: 14px;
          margin-right: 22px;
          >i{
            color: #AAAAAA;
            margin-right: 9px;
          }
        }
      }
    }
    .featuredMain{

    }
  }
  .ad{
    margin: 20px auto 21px auto ;
    background-color: #FFFFFF;
    height: 102px;
    .bannerImg{
      height: 100%;
      background-position: center;
      cursor: pointer;
    }
  }
</style>

