<template>
<div id="home" :style="{width:bWidth + 'px'}">
  <img alt="" src="../../assets/img/shipDisplay/lunbot.png" :style="{width:bWidth + 'px'}"/>
  <div class="header main" :style="{width:width + 'px'}">
    <el-breadcrumb separator-class="el-icon-arrow-right" :style="{width:width+'px'}" class="bc main">
      <el-breadcrumb-item :to="{ path: './shipMan' }">船舶管理</el-breadcrumb-item>
      <el-breadcrumb-item ><p class="textRed">船舶建造</p></el-breadcrumb-item>
    </el-breadcrumb>
    <div class="navBox">
      <div class="nav" :style="{width:width+'px'}">
        <div class="navImgBox point"
             @mouseenter="changeLiTrans(index,1,1)"
             @mouseleave="changeLiTrans(index,0,1)"
             :style="{height:navImgHeight+'px'}"
             v-for="(item,index) in navList"
             @click="toPage(item.locUrl)">
          <!-- < el-image
            :class="{toScale:item.ifShow}"
            :src="item.imgUrl"
            style="width: 100%; height: 100%"
            fit="cover"></el-image> -->
            <img :src="item.imgUrl"
              class="el-images"
              style="width: 100%; height: 100%"
              :ref="'img'+index"
              :class="{toScale:item.ifShow}"
            />
          <p><span>——</span> <span class="textS">{{item.nm}}</span> <span>——</span></p>
        </div>
      </div>
    </div>
  </div>
  <div class="content1 main" :style="{width:width + 'px'}">
    <div class="title">
      <p>
        <img :src="left"/>
        建造流程
        <img :src="right"/>
      </p>
    </div>
    <div class="cont">
      <img src="https://sinovat.oss-cn-shanghai.aliyuncs.com/c6565bb6e51c40699f4160befe329b61-步骤2.png" />
    </div>
  </div>
  <div class="body main" :style="{width:width+'px'}">
    <recomm></recomm>
  </div>
<!--  <div class="body2 main" :style="{width:width+'px'}">-->
<!--    <div class="title">-->
<!--      <p>中创海洋合作船舶建造单位<img src="@/assets/img/shipDisplay/更多.png" alt="" @click="toAllShipYard('../shipDisplay/allShipyard/')" /></p>-->
<!--    </div>-->
<!--    <div class="main"  :style="{width:width+'px'}">-->
<!--      <el-carousel :interval="4000" type="card" height="350px"  arrow="always" indicator-position="none">-->
<!--        <el-carousel-item v-for="(item,index) in shipyardList" :key="item.id">-->
<!--          <div id="ad">-->
<!--            <img :src="item.imgUrl" @click="toShipyard(item)" style="width: 100%;"/>-->
<!--            <p>{{item.company}}</p>-->
<!--          </div>-->
<!--        </el-carousel-item>-->
<!--      </el-carousel>-->
<!--    </div>-->
<!--  </div>-->
  <div class="footerImg" >
    <img src="../../assets/img/footer.png" alt=""/>
  </div>
</div>
</template>

<script>
    import {mapState} from "vuex";
    import recomm from "@/components/recomm";
    import left from "../settleIn/component/img/left.png";
    import right from "../settleIn/component/img/right.png";
    export default {
        name: "shipBuild",
        components: {recomm},
      data(){
          return{
            left,
            right,
            bannerHeight:400,
            navImgHeight:160,
            shipList:[],
            navList:[],
            adertList:[],
            shipyardList:[],

          }
      },
      watch:{
        width(){
          if(this.shipList.length>0){
            let myImg = new Image()
            myImg.src = this.shipList[0].imgUrl
            myImg.onload = () =>{
              this.bannerHeight = this.width*0.5*myImg.height/myImg.width

            }
          }
        }
      },
      computed: {
        ...mapState([
          'bWidth',
          'width',
          'currentRole',
          'typeSize',
          'ifLogin'
        ])
      },
      watch:{
        width(){
          this.getHeight()
        },
      },
      async asyncData(context){
        if(process.client){
          return
        }
        let qry = context.app.query.new()
        context.app.query.toW(qry,'pid','4942035725390848','EQ')
        let classify =await context.app.api.getClassify(context.app.query.toEncode(qry))
        for(let i=0;i<classify.length;i++){
          let qry2 = context.app.query.new()
          context.app.query.toW(qry2,'pid',classify[i].id,'EQ')
          classify[i].list =await context.app.api.getClassify(context.app.query.toEncode(qry2))
        }
        return{
          classify:classify,
        }
      },
      mounted(){
          this.getAdert()
        // this.getShipList()
        // this.getShipyard()
      },
      methods:{
          async getAdert(){
            this.adertList = await this.api.getAdert('shipConAdert',1)
            this.navList = await this.api.getAdert('CBJZ')
            this.navList.forEach((item,index)=>{
              this.objectFitImages(this.$refs['img'+index])
            })
          },
        //船舶列表
        async getShipList(){
          let qry = this.query.new()
          this.query.toP(qry,1,6)
          let data = await this.api.shipList(this.query.toEncode(qry))
          this.shipList = data.data.list
          if(this.shipList.length>0){
            let myImg = new Image()
            myImg.src = this.shipList[0].imgUrl
            myImg.onload = () =>{
              this.bannerHeight = this.width*0.5*myImg.height/myImg.width
            }
          }
        },
        async getShipyard() {
          let qry = this.query.new()
          this.query.toW(qry, 'identityCd', 'identity30')
          this.shipyardList = await this.api.getRoleAll(this.query.toEncode(qry))
          console.log('this.shipyardList',this.shipyardList)
        },
        toShipyard(item){
          this.$router.push({
            path:'../shipDisplay/shipyard',
            query:{
              id:item.id,
            }
          })
        },
        toAllShipYard(url){
          this.$router.push(url)
        },
        close() {
          // 取消弹窗回调
          this.show = false
        },
        submit() {
          // 确认弹窗回调
          this.show = false
        },
          toPage(url){
            if(!url){
              return
            }
            if(url.indexOf('shipDIY')>-1 && !this.ifLogin){
              this.$confirm('您未登录，立即登录?', '提示', {
                confirmButtonText: '确定',
                cancelButtonText: '取消',
                type: 'warning'
              }).then(() => {
                this.$router.push('../login/login')
              })
              return
            }
            // else if(url.indexOf('shipDIY')>-1){
            //   this.$router.push(url + '?tad=2')
            // }
            // if(url.indexOf('shipDIY')>-1){
            //   this.$router.push(url+'?tad=1')
            // }
            this.$router.push(url)
          },
          //鼠标滑动切换放大效果
          // changeLiTrans(index,type){
          //   this.navList[index].ifTrans = type ? true : false
          //   this.$set(this.list,index,this.list[index])
          // },
        changeLiTrans(index,type,nm) {
          if(nm==1){
            this.navList[index].ifShow = type ? true : false
            this.$set(this.navList, index, this.navList[index])
          }
          if(nm==2 && index!=0){
            this.tabList1[index].ifTrans = type ? true : false
            this.$set(this.tabList1,index,this.tabList1[index])
          }
          if(nm==3 && index!=0){
            this.tabList2[index].ifTrans = type ? true : false
            this.$set(this.tabList2,index,this.tabList2[index])
          }
        },
          changeActive(index){
            this.list2.forEach((item,index) =>{
              item.isActive = false
            })
            this.list2[index].isActive = true
            this.$set(this.list2,index,this.list2[index])
          },
        getHeight(){
          this.navImgHeight=this.width*0.321*9/16;
          // this.tabImgHeight=this.width*0.453;
          console.log('nav : '+this.navImgHeight+'tab : '+this.tabImgHeight)
        },
      },
    }
</script>
<style lang="less">
  #home{
    /*卡片图片*/
    .el-carousel__item h3 {
      color: #475669;
      font-size: 14px;
      opacity: 0.75;
      line-height: 200px;
      margin: 0;
    }

    .el-carousel__item:nth-child(2n) {
      /*background-color: #99a9bf;*/
    }

    .el-carousel__item:nth-child(2n+1) {
      /*background-color: #d3dce6;*/
    }
    /*热门设备图片*/
    .el-image{
      margin: auto;
      max-width: 100%;
      max-height: 100%;
    }
    .ensure{
      .el-image{
        width: 100%;
        height: 100%;
      }
    }
  }
</style>
<style lang="less" scoped>
  @import url("../../assets/css/init.less");
#home{
  background-color: #FFFFFF;
}
  .header{
    .bc{
      height: 52px;
      display: flex;
      align-items: center;
    }
    .textRed{
      color: #0064B2;
    }
    .navBox{
      .nav{
        display: flex;
        justify-content: space-between;
        padding:30px 0;
        margin: 0 auto;
        .navImgBox{
          width: 32.16%;
          position: relative;
          overflow: hidden;
          cursor: pointer;
          .el-image{
            width: 100%;
            height: 100%;
          }

          >p{
            width: 100%;
            background-color: rgba(0,0,0,0.35);
            position: absolute;
            padding: 8px 0;
            bottom: 0;
            color:rgba(255,255,255,1);
            text-align: center;
            .textS{
              margin: 0 20px;
              opacity: 1;
              font-size:20px;
            }
            >span{
              opacity: 0.2;
            }
          }
        }
      }
    }
  }
  .content1{
    margin-top: 10px;
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: column;
    .title{
      /*margin-top: 52px;*/
      p{
        color: #333333;
        font-size: 20px;
        >img{
          width: 94px;
          margin-right: 20px;
          margin-left: 20px;
        }
      }
    }
    .cont{
      margin-top: 30px;
      width: 100%;
      >img{
        width: 100%;
      }
    }

  }
  .body{
    margin-top: 30px;
    .title{
      display: inline-flex;
      width: 100%;
      >p{
        font-size: 26px;
        color: #333333;
        display: flex;
        flex-direction: row;
        align-items: center;
        line-height: 76px;
        width: 15%;
        .line{
          display: block;
          width: 5px;
          height: 22px;
          background-color: #2778BE;
          margin: 0 16px 0 20px;
        }
      }
      ul{
        flex: 1;
        display: flex;
        flex-direction: row;
        justify-content: space-between;
        margin-left: 30%;
        li{
          width: auto;
          display: flex;
          align-items: center;
          cursor: pointer;
          line-height: 35px;
          >p{
            color: #666;
            font-size: 14px;
            line-height: 35px;
          }
          .line2{
            display: block;
            width: 1px;
            height: 14px;
            opacity:0.2;
            background-color: #000;
            margin-left: 20px;
          }
        }
        .active{
          >p{
            color: #0064B2;
            border-bottom: 2px solid #2778BE;
          }
        }
      }
      .more{
        font-size: 14px;
        line-height: 30px;
        color: #999999;
        margin-left: 20px;
        width: auto;
        cursor: pointer;
      }
    }
    .content{
      width: 100%;
      .down{
        margin-top: 5px;
        width: 100%;
        flex: 1;
        display: flex;
        justify-content: space-between;
        .part{
          width: 19.25%;
          display: flex;
          flex-direction: column;
          flex-wrap: wrap;
          position: relative;
          img{
            width: 100%;
            height: 100%;
            border: 1px solid #999999;
          }
          .ct{
            position: absolute;
            bottom: 13px;
            width: 100%;
            align-items: center;
            text-align: center;
            p:nth-child(1){
              line-height: 30px;
              font-size: 20px;
              text-align: center;
              /*color: #FFFFFF;*/
              color: #999999;
            }
            p:nth-child(2){
              line-height: 20px;
              text-align: center;
              /*color: #FFFFFF;*/
              color: #999999;
            }
          }
        }
      }
    }
  }
  /*.body2{*/
  /*  margin-top: 60px;*/
  /*  .title{*/
  /*    width: 100%;*/
  /*    >p{*/
  /*      font-size: 30px;*/
  /*      text-align: center;*/
  /*      color: #333333;*/
  /*      line-height: 90px;*/
  /*      >img{*/
  /*        width: 21px;*/
  /*        height: 21px;*/
  /*        vertical-align: middle;*/
  /*        margin-left: 28px;*/
  /*        cursor: pointer;*/
  /*      }*/
  /*    }*/
  /*  }*/
  /*}*/
  #ad{
    width: 100%;
    height: 100%;
    position: relative;
    >p{
      position: absolute;
      bottom: 0;
      line-height: 40px;
      font-size: 20px;
      width: 100%;
      color:rgba(255,255,255,1);
      text-align: center;
      background-color: rgba(0,0,0,0.35);

    }
  }
  .footerImg{
    margin-top: 51px;
    width: 100%;
  }
</style>
