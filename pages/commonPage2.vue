<template>
  <div id="home" :style="{width:bWidth+'px'}">
    <el-carousel :height="bannerHeight+'px'" class="adert" v-if="adertList.length>0">
      <el-carousel-item v-for="(item,index) in adertList" :key="index">
        <div class="bannerImg" :style="{backgroundImage:'url('+item+')'}" />
      </el-carousel-item>
    </el-carousel>
    <div class="main" :style="{width:width + 'px'}">
        <div class="pos" v-if="info"><p @click="toPage('/')">首页</p> > <p @click="toPage(decodeURIComponent(pUrl))">{{info.cnms}}</p> > <p>{{info.nm}}</p></div>
        <div class="info" v-html="info.cont" v-if="info"></div>
    </div>
  </div>
</template>

<script>

  import {mapState} from "vuex";
  export default {

    data(){
      return{
        adertList:[],//轮播图列表
        bannerHeight:0,
        id:'',
        info:null,
        pUrl:'',
      }
    },
    computed: {
      ...mapState([
        'bWidth',
        'width',
      ])
    },
    components: {
    },
    watch:{

    },
    created(){

    },
    async asyncData(context){
      if(process.client){
        return
      }

      let info = await context.app.api.ruleDetail(context.query.id)
       return {
        info:info,
      }
    },
    mounted(){
      this.pUrl = this.until.getQueryString('pUrl')
      // console.log(decodeURIComponent(this.pUrl))
      this.getData()

    },
    watch:{
      $route(){
        this.pUrl = this.until.getQueryString('pUrl')
        // console.log(decodeURIComponent(this.pUrl))
        this.getData()
      }
    },
    methods:{
      async getData(){
        this.id = this.until.getQueryString('id')
        this.info = await this.api.ruleDetail(this.id)
        // console.log(this.info.cont)
        this.info.cont = this.info.cont.replace(/<img[^>]*>/gi, function (match, capture) {
          // console.log('=======')
          return match.replace(/style\s*?=\s*?([‘"])[\s\S]*?\1/ig, 'style="max-width:100%;height:auto;width:auto"') // 替换style
        })
        this.adertList = this.info.imgUrl ? this.info.imgUrl.split(',') : []
        if(this.adertList.length>0){
          let myImg = new Image()
          myImg.src = this.adertList[0]
          myImg.onload = ()=>{
            this.bannerHeight = myImg.height
          }
        }else {
          this.bannerHeight = 0
        }
      },

      //页面跳转
      toPage(url){
        if(url && url.indexOf('http')>-1){
          window.open(url)
        }else if(url){
          this.$router.push(url)
        }
      },
    }
  }
</script>

<style lang="less" scoped>
  @import url("../assets/css/init.less");

  #home{
    background-color: #F3F3F3;
    background-image: url("../assets/img/footer.png");
    background-position: center bottom;
    background-repeat: repeat-x;
    padding-bottom: 80px;
    /deep/img{
      display: block;
    }
    .adert{
      width: 100%;
      text-align: center;
      /*max-height: 350px;*/
      .bannerImg{
        height: 100%;
        background-position: center;
      }
    }

  }
  .pos{
    padding-top: 30px;
    p:not(:last-of-type){
      cursor: pointer;
    }
    p{
      display: inline-block;
    }
  }
  .info{
    margin: 30px auto 0;

  }


</style>
