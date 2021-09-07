<template>
      <div class="banner" :style="{width:bWidth+'px'}">
        <el-carousel :height="bannerHeight+'px'" class="adert">
          <el-carousel-item v-for="item in adertList" :key="item.aiid">
            <img :src="item.imgUrl"/>
          </el-carousel-item>
        </el-carousel>
      </div>
</template>
<script>
  import {mapState} from "vuex";
  export default {
    props:[],
    data(){
      return {
        adertList: [],
        bannerHeight:400,
      }
    },
    components: {
    },
    watch:{
      bWidth(){
        if(this.adertList.length>0){
          let myImg = new Image()
          myImg.src = this.adertList[0].imgUrl
          myImg.onload = ()=>{
            // this.bannerHeight = myImg.height/myImg.width*this.bWidth
            if(myImg.width>this.bWidth){
              this.bannerHeight = myImg.height/myImg.width*this.bWidth
            }else {
              this.bannerHeight = myImg.height
            }
          }
        }
      },
    },
    computed: {
      ...mapState([
        'bWidth',
      ])
    },
    mounted(){
      this.getAdert()
    },
    methods:{
      //获取广告轮播
      async getAdert(){
        this.adertList = await this.api.getAdert('homeAdert')
        if(this.adertList.length>0){
          let myImg = new Image()
          myImg.src = this.adertList[0].imgUrl
          myImg.onload = ()=>{
            // this.bannerHeight = myImg.height/myImg.width*this.bWidth
            if(myImg.width>this.bWidth){
              this.bannerHeight = myImg.height/myImg.width*this.bWidth
            }else {
              this.bannerHeight = myImg.height
            }
          }
        }
      },
    }
  }
</script>
<style lang="less">

</style>
<style lang="less" scoped>
  @import url("../assets/css/init.less");
  .banner{
    text-align: center;
  }
</style>

