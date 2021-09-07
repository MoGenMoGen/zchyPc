<template>
<div id="home" :style="{width:bWidth + 'px'}">
  <div class="header">
    <el-carousel :height="bannerHeight+'px'" class="adert">
      <el-carousel-item v-for="item in adertList" :key="item.aiid">
        <div class="bannerImg" :style="{backgroundImage:'url('+item.imgUrl+')'}"  @click="toUrl(item.locUrl)"/>
      </el-carousel-item>
    </el-carousel>
    <el-breadcrumb separator-class="el-icon-arrow-right" :style="{width:width+'px'}" class="bc main">
      <el-breadcrumb-item :to="{ path: './pilotService' }">试点服务</el-breadcrumb-item>
      <el-breadcrumb-item ><p class="textRed">{{nm}}</p></el-breadcrumb-item>
    </el-breadcrumb>
  </div>
  <div class="content main" :style="{width:width + 'px'}">
    <div class="partA">
      <info-menu @changeActive="getData"></info-menu>
    </div>
    <div class="partB">
      <div class="info" v-for="(item,index) in list" :key="index" @click="toPage(item)">
        <div class="cont">
          <p>{{item.nm}}</p>
        </div>
        <p>{{item.releTm}}</p>
        <!--<div class="img">-->
<!--&lt;!&ndash;          <img :src="item.imgUrl" alt=""/>&ndash;&gt;-->
          <!--<img :src="item.imgUrl"-->
               <!--class="el-images"-->
               <!--:ref="'img'+index"-->
          <!--/>-->
          <!--&lt;!&ndash;< el-image&ndash;&gt;-->
            <!--&lt;!&ndash;style="width: 100%; height: 100%"&ndash;&gt;-->
            <!--&lt;!&ndash;:src="item.imgUrl"&ndash;&gt;-->
            <!--&lt;!&ndash;fit="cover"></el-image>&ndash;&gt;-->
        <!--</div>-->
        <!--<div class="right">-->
          <!--<p>{{item.nm}}</p>-->
          <!--<p >{{item.cont}}<span >查看更多<i class="el-icon-arrow-right"></i></span></p>-->
          <!--<p>{{item.releTm}}</p>-->
        <!--</div>-->
      </div>
      <div class="foot mian">
        <el-pagination
          v-if="this.list != ''"
          background
          @current-change="handleCurrentChange"
          :current-page.sync="currentPage3"
          :page-size='pageSize'
          layout="prev, pager, next, jumper"
          :total= 'totalNm'>
        </el-pagination>
      </div>
    </div>
  </div>
  <div class="footerImg" :style="{width:bWidth+'px'}">
    <img src="../../assets/img/footer.png" alt=""/>
  </div>
</div>
</template>

<script>
    import {mapState} from "vuex";
    import InfoMenu from "./components/infoMenu";
    export default {
        name: "tradeInfo",
      components: {InfoMenu},
      data(){
          return{
            bannerHeight:350,
            currentPage3: 1,
            pageSize: 30,
            list:[],
            id:'', //筛选id
            totalNm: 0, //当前页码
            NoNow:'', //未使用
            adertList:[],
            nm:'',
          }
        },
      computed: {
        ...mapState([
          'bWidth',
          'width',
          'currentRole'
        ])
      },
      watch:{
        $route(e){
          let arr = this.until.getQueryString('cid')
          console.log('此时arr',arr)
          this.id=arr
          this.currentPage3=1
          this.nm = this.until.getQueryString('nm')
          this.getData()
        },

      },
      async asyncData(context){
        if(process.client){
          return
        }
        let qry = context.app.query.new()
        let list = await context.app.api.infoList(context.app.query.toEncode(qry))
        let id = context.query.cid ? context.query.cid : list[0].id

        let qry2 = context.app.query.new()
        context.app.query.toW(qry2,'cid',id,'EQ')
        context.app.query.toP(qry2,1,100)
        let data = await context.app.api.tradeInfo(context.app.query.toEncode(qry2))
        return{
          list:list,
          data:data
        }
      },
      mounted(){
        let arr = this.until.getQueryString('cid')
        this.nm = this.until.getQueryString('nm')
        console.log('此时arr2',arr)
        this.getAdert()
        if(arr){
          this.id=arr
        }
        this.getData()
        // this.changeActive()
      },
      methods:{
        toUrl(url){
          if(url){
            window.location.href = url
          }
        },
        async getAdert(){
          this.adertList = await this.api.getAdert('subsidies')
          if(this.adertList.length>0){
            let myImg = new Image()
            myImg.src = this.adertList[0].imgUrl
            myImg.onload = ()=>{
              // this.bannerHeight = myImg.height/myImg.width*this.bWidth
              this.bannerHeight = myImg.height
            }
          }
        },
        handleCurrentChange(val) {
          this.currentPage3 = val
          this.getData()
        },
        toPage(item){
          console.log(item)
          if(item.cids){
            item.pid=item.cids.split(",")[0]
          }
          console.log(item.pid)
          this.$router.push({
            path:'./infoDetail',
            query:{
              id:item.pid,
              cid:item.cid,
              sid:item.id
            }
          });
        },
        async getData(val){
          this.list=[]
          console.log('父组件')
          console.log(this.id)
          console.log(val)
          let qry = this.query.new()
          if(!this.id && val){
            this.id =val
          }
          if(!this.id && !val){
            return
          }
          this.query.toW(qry,'cid',this.id,'EQ')
          this.query.toP(qry,this.currentPage3,this.pageSize)
          let data = await this.api.serviceContent(this.query.toEncode(qry))
          console.log('data: '+val)
          this.list = data.data.list
          console.log(this.list)
          this.totalNm =  data.page.total
          this.list.forEach((item,index) =>{
            item.cont = item.cont.replace(/<\/?[^>]*>/g, "").slice(0,150) + '...';
            item.cont = item.cont.replace(/(&nbsp;)/g, "");
            this.objectFitImages(this.$refs['img'+index])
          // console.log(item.id)
          })
        },
      },

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
      text-align: center;
      /*max-height: 350px;*/
      .bannerImg{
        height: 100%;
        background-position: center;
      }
    }
    .bc{
      height: 52px;
      display: flex;
      align-items: center;
      .textRed{
        color: #0064B2;
      }
    }
  }
  .content{
    padding-top: 20px;
    display: inline-flex;
    justify-content: space-between;
    .partA{

    }
    .partB{
      padding: 0 40px;
      flex:1;
      .info{
        display: flex;
        display: -webkit-flex;
        /*flex: 1;*/
        /*padding: 30px 0;*/
        /*border-bottom: 1px solid #E8E8E8;*/
        /*cursor: pointer;*/
        font-size: 18px;
        color: #333333;
        padding: 2px 0;
        height: 40px;
        line-height: 40px;
        .cont{
          flex: 1;
          position: relative;
          padding-left: 12px;
          height: 40px;
          line-height: 40px;
          overflow: hidden;
          >p{
            display: inline-block;
            cursor: pointer;
            &:hover{
              color: @themeColor;
              text-decoration: underline;
            }
          }
          &:before{
            position: absolute;
            left: 0;
            top: 20px;
            transform: rotateX(-50%);
            display: block;
            content: "";
            width: 3px;
            height: 3px;
            background: #333;
          }

        }
        /*&:first-of-type{*/
          /*padding-top: 0;*/
        /*}*/
        .img{
          /*height: 160px;*/
          width: 240px;
          display: flex;
          flex-shrink: 0;
          >img{
            /*margin: auto;*/
            /*object-fit: cover;*/

          }
        }
        .right{
          margin-left: 39px;
          display: flex;
          flex-direction: column;
          flex-wrap: wrap;
          justify-content: space-between;
          flex: 1;
          p:nth-child(1){
            font-size: 18px;
            color: #333333;
            line-height: 40px;
          }
          p:nth-child(2){
            color: #666666;
            font-size: 14px;
            width: 100%;
            line-height: 25px;
            flex: 1;
            >span{
              margin-left: 10px;
              color: @themeColor;
              cursor: pointer;
            }
          }
          p:nth-child(3){
            font-size: 12px;
            line-height: 30px;
            color: #999999;
          }
        }
      }
      .foot{
        margin-top: 40px;
        width: 100%;
        display: flex;
        justify-content: center;
      }
    }
  }
  .footerImg{
    width: 100%;
    margin-top: 53px;
  }
</style>
