<template>
  <div id="home" :style="{width:bWidth + 'px'}">
    <div class="header">
      <el-carousel :height="bannerHeight+'px'" class="adert">
        <el-carousel-item v-for="item in adertList" :key="item.aiid">
          <div class="bannerImg" :style="{backgroundImage:'url('+item.imgUrl+')'}"  @click="toUrl(item.locUrl)"/>
        </el-carousel-item>
      </el-carousel>
    </div>
    <div class="content main" :style="{width:width + 'px'}">
      <div class="partA">
        <info-menu ></info-menu>
      </div>
      <div class="partB" >
        <div class="head" v-bind="info">
          <p>
            <span @click="back"><i class="el-icon-arrow-left"></i>返回</span>
            {{info.nm}}
          </p>
          <p><span>{{info.releTm}}</span><span>来源：{{info.src}}</span><span>作者：{{info.author}}</span><share></share></p>
        </div>
        <div class="body" v-html="info.cont">

        </div>
      </div>
    </div>
    <div class="footerImg" :style="{width:bWidth+'px'}">
      <img src="../../assets/img/footer.png" alt=""/>
    </div>
  </div>
</template>

<script>
    import InfoMenu from "./components/infoMenu";
    import share from "@/components/share";
    import {mapState} from "vuex";

    export default {
        name: "infoDetail",
      components: {InfoMenu,share},
      data(){
          return{
            bannerHeight:346,
            infoId:'',
            choose:'',
            adertList:[],
            info:{},
          }
      },
      computed: {
        ...mapState([
          'bWidth',
          'width'
        ])
      },
      watch:{
        $route(){
          this.infoId = this.$route.query.id
          this.choose = this.$route.query.cid
          this.getData()
        },

      },
      async asyncData(context){
        if(process.client){
          return
        }
        let id = context.query.id
        let data = await context.app.api.detailInfo(id)
        return{
          data:data
        }
      },
      mounted() {
        this.infoId = this.$route.query.id
        this.choose = this.$route.query.cid
        this.getAdert()
          this.getData()
        console.log("111",this.choose)

      },
      methods:{
        toUrl(url){
          if(url){
            window.location.href = url
          }
        },
        async getAdert(){
          this.adertList = await this.api.getAdert('PChyxx')
          if(this.adertList.length>0){
            let myImg = new Image()
            myImg.src = this.adertList[0].imgUrl
            myImg.onload = ()=>{
              // this.bannerHeight = myImg.height/myImg.width*this.bWidth
              this.bannerHeight = myImg.height
            }
          }
        },
        toPage(url){
          this.$router.push(url)
        },
        back(){
          this.$router.go(-1)
        },
        async getData(id){
          id = this.infoId
          this.info = await this.api.detailInfo(id)
          // this.info.cont = this.info.cont.replace("宋体",'微软雅黑')
          this.info.cont = this.info.cont.replace(/<img[^>]*>/gi, function (match, capture) {
            console.log('=======')
            return match.replace(/style\s*?=\s*?([‘"])[\s\S]*?\1/ig, 'style="max-width:100%;height:auto;width:auto;"') // 替换style

          })
        },
      }

    }
</script>

<style lang="less">
  #home{
    /*卡片图片*/
    .el-carousel__item:nth-child(2n) {
      /*background-color: #99a9bf;*/
    }
    .el-carousel__item:nth-child(2n+1) {
      /*background-color: #d3dce6;*/
    }
  }
</style>
<style lang="less" scoped>
  @import url("../../assets/css/init.less");
  #home{
    background-color: #FFFFFF;
    display: flex;
    flex-wrap: wrap;
  }
  .header{
    width: 100%;
    .adert{
      width: 100%;
      /*max-height: 350px;*/
      .bannerImg{
        height: 100%;
        background-position: center;
      }
    }
  }
  .content{
    padding-top: 50px;
    display: inline-flex;
    justify-content: space-between;
    .partA{
    }
    .partB{
      padding: 0 40px;
      width: 78.3%;
      .head{
        p:nth-child(1){
          text-align: center;
          width: 100%;
          color: #333333;
          font-size: 20px;
          line-height: 30px;
          >span{
            color: @themeColor;
            float: left;
            font-size: 14px;
            cursor: pointer;
          }

        }
        p:nth-child(2){
          color: #999999;
          text-align: center;
          line-height: 50px;
          margin-bottom: 11px;
          border-bottom: 1px solid #E8E8E8;
          >i{
            color: @themeColor;
          }
          >span{
            margin-right: 34px;
          }
          .share{
            cursor: pointer;
          }
        }
      }
      .body{
        line-height: 26px;
        color: #303030;
        text-align: justify;
        /*text-align-last:justify;*/
        -moz-text-align-last: justify ;
        /*-webkit-text-align-last:justify;*/
        text-justify:inter-ideograph;/*ie中必须有这个*/
        /*text-align-last:justify;*/

        p{
          text-indent: 18px;
        }
      }
    }
  }
  .footerImg{
    width: 100%;
    margin-top: 53px;
  }
</style>
