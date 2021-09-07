<template>
  <div id="home" :style="{width: bWidth+'px'}">
    <!--头部-->
    <div class="header main" :style="{width:width + 'px'}">
      <el-breadcrumb separator-class="el-icon-arrow-right" :style="{width:width+'px'}" class="bc main">
        <el-breadcrumb-item :to="{ path: '/' }">首页</el-breadcrumb-item>
        <el-breadcrumb-item :to="{ path: './allShipyard' }">船厂总览</el-breadcrumb-item>
        <el-breadcrumb-item ><p class="textRed">船厂详情</p></el-breadcrumb-item>
      </el-breadcrumb>
    </div>
    <!--内容-->
    <div class="body main" :style="{width:width + 'px'}">
      <div class="left">
<!--        <img :src="info.imgUrl" alt=""/>-->

        <el-carousel>
          <el-carousel-item v-for="item in info.imgUrl" :key="item.id">
            <img :src="item" alt="" style="width: 100%"/>
          </el-carousel-item>
        </el-carousel>
      </div>
      <div class="right">
        <p>{{info.company}}</p>
        <p>建造优势：{{info.business}}</p>
        <p>{{info.summery}}</p>
        <p><img src="@/assets/img/shipDisplay/地址.png" alt=""/><span>公司地址：</span>{{info.addrNm}}{{info.address}}</p>
        <p><img src="@/assets/img/shipDisplay/网址.png" alt="" style="height: 13px;"/><span>公司网址：</span>{{info.linkMan}}</p>
        <p><img src="@/assets/img/shipDisplay/联系电话.png" alt=""/><span>联系电话：</span>{{info.mob}}</p>
        <p><img src="@/assets/img/shipDisplay/联系邮箱.png" alt=""/><span>联系邮箱：</span>{{info.email}}</p>
        <!-- <button>设计预约</button> -->
      </div>
    </div>
    <!-- 内容-->
    <div class="content main" :style="{width:width + 'px'}">
      <div class="head">
        <p>船厂详情</p>
      </div>
      <div v-html="info.intro" class="cont">
      </div>
    </div>
    <!--脚部-->
    <div class="footer" :style="{width:bWidth+'px'}">
      <div class="mid main" :style="{width:width + 'px'}">
        <div class="up">
          <p><span class="line"></span>推荐船型</p>
        </div>
        <div class="down">
          <div>
            <div class="part" v-for="(item,index) in list2" :key="index" @click="toDetail(item)">
              <img :src="item.soluImg"
                    class="el-images"
                    :ref="'img'+index"/>
              <div class="ct">
                <p>{{item.soluNm}}</p>
                <!--<p>{{item.size}}</p>-->
              </div>
            </div>
          </div>
        </div>
      </div>
      <div class="footerImg" >
        <img src="../../assets/img/footer.png" alt=""/>
      </div>
    </div>
  </div>
</template>
<script>
  import {mapState} from "vuex";
  export default {
    name: "shipyard",
    data(){
      return{
        id:"",
        info:{},
        list2:[],
      }
    },
    computed: {
      ...mapState([
        'bWidth',
        'width',
        'currentRole',
        'typeSize'
      ])
    },
    async asyncData(context){
      if(process.client){
        return
      }

      let info = await context.app.api.getRoleDetail2(context.query.id)

      let qry = context.app.query.new()
      let data = await context.app.api.shipList2(context.app.query.toEncode(qry))
      return{
        info:info,
        data:data,
      }
    },
    async mounted() {
      this.id=this.until.getQueryString('id')
      this.info = await this.api.getRoleDetail2(this.id)
      this.info.imgUrl=this.info.imgUrl.split(',')
      this.info.summery = this.info.summery.slice(0, 80) + '...';
      console.log(this.info.imgUrl)
      this.getShipList()
    },
    methods:{
      async getShipList(){
        let qry = this.query.new()
        this.query.toP(qry,1,5)
        this.list2 = await this.api.factoryRecom(this.id,this.query.toEncode(qry))
        this.list2.forEach((item,index)=>{
          this.objectFitImages(this.$refs['img'+index])
        })
        console.log('推荐船型')
        console.log(this.list2)
      },
      toList(item){
        this.$router.push({
          path:'../shipDisplay/shipShow',
          query:{
            cId3:item.id,
            cNm:item.nm,
            cImg:item.imgUrl2
          }
        })
      },
      toDetail(item){
        this.$router.push({
          path:'./showDetail',
          query:{
            id:item.solutionId,
          }
        })
      },
    },

  }
</script>

<style lang="less" scoped>
  @import url("../../assets/css/init.less");
  #home{
    background-color: #F3F3F3;
  }
  .header{
    height: 54px;
    align-items: center;
    display: flex;
    .textRed{
      color: @themeColor;
    }
  }
  .body{
    padding: 30px;
    display: flex;
    background-color: #FFFFFF;
    .left{
      width: 43.3%;
      /*display: flex;*/
      /*display: -webkit-flex;*/
      img{
        margin: auto;
      }
    }
    .right{
      flex: 1;
      margin-left: 6%;
      position: relative;
      button{
        position: absolute;
        bottom: 13px;
        right: 6px;
        width: 157px;
        height: 42px;
        color: #FFFFFF;
        background-color: #2778BE;
        border: none;
        border-radius: 2px;
      }
      i{
        margin-right: 9px;
      }
      p{
        max-width: 546px;
        line-height: 30px;
        color: #666666;
      }
      img{
        width: 16px;
        height: 16px;
        vertical-align: middle;
        margin-right: 9px;
      }
      //标题
      p:nth-child(1){
        font-size: 20px;
        color: #303030;
        line-height: 40px;
        font-weight: bold;
      }
      //设计优势
      p:nth-child(2){
        font-size: 16px;
        color: #666666;
        margin-bottom: 10px;
        line-height: 28px;
        overflow: hidden;
        display: -webkit-box;
        -webkit-box-orient: vertical;
        -webkit-line-clamp: 2;
        overflow: hidden;
      }
      //内容
      p:nth-child(3){
        font-size: 14px;
        color: #999999;
        line-height: 25px;
        margin-bottom: 20px;
        display: -webkit-box;
        -webkit-box-orient: vertical;
        -webkit-line-clamp: 2;
        overflow: hidden;
      }
    }
  }
  .content{
    margin-top: 30px;
    background-color: #FFFFFF;
    padding: 21px;
    .head{
      background-color: #ECECEC;
      height: 40px;
      p{
        line-height: 40px;
        padding-left: 28px;
        color: #666666;
      }
    }
  }
  .footer{
    margin-top: 40px;
    background-color: #FFFFFF;
    .mid{
      display: flex;
      flex-wrap: wrap;
      flex-direction: column;
      .up{
        display: inline-flex;
        p{
          font-size: 26px;
          color: #333333;
          display: flex;
          align-items: center;
          line-height: 76px;
          .line{
            display: block;
            width: 5px;
            height: 22px;
            background-color: #2778BE;
            margin: 0 16px 0 20px;
          }
        }
      }
      .down{
        width: 100%;
        margin-bottom: 80px;
        overflow-x: auto;
        >div{
          display: flex;
          .part{
          width: 19%;
          height: 140px;
          margin-right: 10px;
          display: flex;
          flex-direction: column;
          flex-wrap: wrap;
          position: relative;
          img{
            width: 100%;
            height: 100%;
            cursor: pointer;
          }
          .ct{
            position: absolute;
            bottom: 0px;
            width: 100%;
            align-items: center;
            text-align: center;
            background-color: rgba(0,0,0,0.35);
            p:nth-child(1){
              line-height: 30px;
              font-size: 12px;
              text-align: center;
              color: #FFFFFF;
              // color: #999999;
            }
            p:nth-child(2){
              line-height: 20px;
              text-align: center;
              color: #FFFFFF;
              // color: #999999;
              }
            }
          }
        }
      }
    }
    .footerImg{
      width: 100%;
    }
  }
</style>
