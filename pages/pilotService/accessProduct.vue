<template>
  <!-- 产品准入 -->
<div id="home" :style="{width:bWidth + 'px'}">
  <div class="header">
    <el-carousel :height="bannerHeight+'px'">
      <el-carousel-item v-for="item in adertList" :key="item.id">
        <img :src="item.imgUrl" style="width:100%">
      </el-carousel-item>
    </el-carousel>
    <el-breadcrumb separator-class="el-icon-arrow-right" :style="{width:width+'px'}" class="bc main">
      <el-breadcrumb-item :to="{ path: './pilotService' }">试点服务</el-breadcrumb-item>
      <el-breadcrumb-item ><p class="textRed">产品准入</p></el-breadcrumb-item>
    </el-breadcrumb>
  </div>
  <div class="ad main" :style="{width:width+'px'}" style="display: flex;">
    <div class="lfBox">
      <div v-for="(item,index) in list" :key="index" class="info" :class="{active:choose == item.id}" @click="changeActive(item.id)">
        <p>{{item.nm}}</p>
      </div>
    </div>
    <div class="contBox">
      <div class="head" v-bind="info">
        <!-- <p>
          <span @click="back"><i class="el-icon-arrow-left"></i>返回</span>
          {{info.nm}}
        </p>
        <p><span>{{info.releTm}}</span><span>来源：{{info.src}}</span><span>作者：{{info.author}}</span><share></share></p> -->
      </div>
      <div class="body" v-html="info.cont">

      </div>
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
        name: "vision",
      data(){
          return{
            bannerHeight:'300',
            navImgHeight:'160',
            tabImgHeight:'285',
            adertList:[],
            list:[],
            choose:'',
            info:{},
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
      this.getList()
    },
    methods:{
      async getList(){
        let qry = this.query.new()
        this.query.toW(qry,'cid','5075273153057792','EQ')
        this.query.toP(qry,this.currentPage3,this.pageSize)
        let data = await this.api.serviceContent(this.query.toEncode(qry))
        this.list=data.data.list
        console.log(this.list)
        this.choose=this.list[0].id
        this.getInfo(this.choose)
      },
      //  选中项高亮
      changeActive(id){
        //将选中项的id赋值给this.choose
        //   this.choose = item.id
        console.log('选中项Id',id)
        this.choose=id
        this.getInfo(this.choose)
      },
      async getInfo(id){
        this.info=await this.api.serviceDetail(id)
        console.log(this.info)
      },
      async getAdert(){
        this.adertList = await this.api.getAdert('accessProduct')
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
  }
  .lfBox{
    border: 1px solid #F3F3F3;
    .info{
      width: 160px;
      height: 60px;
      >p{
        color: #333333;
        font-size: 16px;
        text-align: center;
        line-height: 60px;
      }
      border-bottom: 1px solid #F3F3F3;
      cursor: pointer;
    }
    .active{
      background-color: @themeColor;
      >p{
        color: #FFFFFF;
      }
    }
  }
  .contBox{
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
          margin-left: 34px;
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
  .footerImg{
    margin-top: 50px;
  }
}
</style>
