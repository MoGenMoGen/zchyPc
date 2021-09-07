<template>
  <div id="home" :style="{width:bWidth + 'px'}">
    <div class="header">
      <el-carousel :height="bannerHeight+'px'">
        <el-carousel-item v-for="item in adertList" :key="item.id">
          <img :src="item.imgUrl" style="width:100%">
        </el-carousel-item>
      </el-carousel>
      <el-breadcrumb separator-class="el-icon-arrow-right" :style="{width:width+'px'}" class="bc main">
        <el-breadcrumb-item :to="{ path: './shipMan' }">船舶管理</el-breadcrumb-item>
        <el-breadcrumb-item ><p class="textRed">检验检测</p></el-breadcrumb-item>
      </el-breadcrumb>
      <div class="ad main" :style="{width:width+'px'}" v-for="(item,index) in list" :key="index">
        <img :src="item.imgUrl"/>
      </div>
    </div>
    <div class="footerImg" :style="{width:bWidth+'px'}">
      <img src="@/assets/img/footer.png" alt=""/>
    </div>
  </div>
</template>

<script>
  import {mapState} from "vuex";
    export default {
        name: "detect",
      data(){
          return{
            bannerHeight:'300',
            navImgHeight:'160',
            tabImgHeight:'285',
            adertList:[],
            list:[],
          }
      },
      computed: {
        ...mapState([
          'bWidth',
          'width',
          'currentRole'
        ])
      },
      mounted() {
        this.getAdert()
      },
      methods:{
        async getAdert(){
          this.adertList = await this.api.getAdert('shipAdert')
          if(this.adertList.length>0){
            let myImg = new Image()
            myImg.src = this.adertList[0].imgUrl
            myImg.onload = ()=>{
              if(myImg.width>this.bWidth){
                this.bannerHeight = myImg.height/myImg.width*this.bWidth
              }else {
                this.bannerHeight = myImg.height
              }
            }
          }
          this.list = await this.api.getAdert('JYJC')
          this.getHeight()
        },
        getHeight(){
          this.navImgHeight=this.width*0.24*9/16;
          this.tabImgHeight=this.width*0.24;
          console.log('nav : '+this.navImgHeight+'tab : '+this.tabImgHeight)
        },
        toPage(url){
          this.$router.push(url)
        }
      }

    }
</script>

<style lang="less" scoped>
  @import url("../../assets/css/init.less");
  #home{
    background-color: #FFFFFF;
    .header{
      .bc{
        height: 52px;
        display: flex;
        align-items: center;
        .textRed{
          color: #0064B2;
        }
      }
      .ad{
        margin-top: 11px;
      }
    }
    .footerImg{
      margin-top: 50px;
    }
  }
</style>
