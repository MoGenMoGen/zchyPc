<template>
<div id="home" :style="{width:bWidth + 'px'}">
  <el-carousel :height="bannerHeight+'px'" class="adert">
    <el-carousel-item v-for="item in adertList" :key="item.aiid">
      <img :src="item.imgUrl" @click="toPage(item.locUrl)"/>
    </el-carousel-item>
  </el-carousel>
  <div class="content main" :style="{width:width + 'px'}">
    <div class="left">
      <div class="part" v-for="(v,s) in menuLis" :key="v.id" @click="tabId=v.id" :class="{active: tabId==v.id}">
        <p>{{v.nm}}</p>
      </div>
    </div>
    <div class="right">
       <div v-html="info.cont"></div>
    </div>
  </div>
  <div class="footerImg" :style="{width:bWidth+'px'}">
    <img src="../../assets/img/footer.png" alt=""/>
  </div>
</div>
</template>

<script>
  import {mapState} from "vuex";
  export default {
        name: "us",
      data(){
            return{
              bannerHeight:0,
              adertList:[],
              info:{},
              menuLis:[],
              tabId:1,
              show: false,
            }
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
      tabId(){
        if(this.tabId){
          this.api.ruleDetail(this.tabId).then(res=>{
            this.info = res
          })
        }

      }
    },
    computed: {
      ...mapState([
        'bWidth',
        'width',
        'currentRole'
      ])
    },
    mounted(){
        this.getAdert()
        this.getMenu()
    },
    methods:{
      //轮播图
      async getAdert(){
        this.adertList = await this.api.getAdert('5927')
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
      //左边栏目
      async getMenu(){
        let qry = this.query.new()
        this.query.toW(qry,'cid','5034191076054016','EQ')
        this.menuLis = await this.api.ruleList(this.query.toEncode(qry))
        this.tabId = this.menuLis[0].id
      },
      toPage(url){
        if(url){
          window.location.href = url
        }
      }
    },
  }
</script>

<style lang="less" scoped>
  @import url("../../assets/css/init.less");
#home{
  background-color: #ffffff;
}
  .content{
    margin-top: 40px;
    display: flex;
    flex-direction: row;
    .left{
      width: 20.8%;
      .part{
        width: 200px;
        height: 60px;
        border: 1px solid #E8E8E8;
        display: flex;
        align-items: center;
        >p{
          width: 100%;
          text-align: center;
          color: #666666;
          font-size: 20px;
          cursor:pointer;
        }
        &:first-of-type{
          border-bottom: 0;
        }
      }
      .active{
        background-color: @themeColor;
        >p{
          color: #FFFFFF;
        }
      }
    }
    .right{
      display: flex;
      flex: 1;
      flex-direction: column;
      flex-wrap: wrap;
      .head{
        width: 100%;
        border-bottom: 1px solid #E8E8E8;
        padding-bottom: 30px;
        >p{
         width: 100%;
          text-align: center;
          font-size: 32px;
          line-height: 40px;
          color: #333333;
        }
      }
      .head2{
        width: 100%;
        padding-bottom: 30px;
        >p{
          width: 100%;
          text-align: center;
          font-size: 34px;
          line-height: 40px;
          font-weight: bold;
          color: #333333;
        }
      }
      .body{
        padding: 28px;
      }
    }
  }
  .footerImg{
    margin-top: 15px;
  }
</style>
