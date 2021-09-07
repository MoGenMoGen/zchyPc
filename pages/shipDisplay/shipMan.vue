<template>
<!--  船舶管理-->
    <div class="cont" :style="{width:bWidth+'px'}">
      <design-appo :show="appoShow" @close="close" @submit="submit"></design-appo>
      <div class="cont-1">
        <el-carousel :height="bannerHeight+'px'" class="adert">
          <el-carousel-item v-for="item in adertList" :key="item.id">
            <div class="bannerImg" :style="{backgroundImage:'url('+item.imgUrl+')'}"  @click="toPage(item.locUrl)"/>
          </el-carousel-item>
        </el-carousel>
      </div>
      <div class="navBox">
        <div class="nav" :style="{width:width+'px'}">
          <div class="navImgBox point"
               @mouseenter="changeLiTrans(index,1,1)"
               @mouseleave="changeLiTrans(index,0,1)"
               :style="{height:navImgHeight+'px'}"
               v-for="(item,index) in navList"
               @click="toPage(item.locUrl)">
            <img :src="item.imgUrl"
                 class="el-images"
                 :ref="'img'+index"
                 :class="{toScale:item.ifShow}"
            />
            <!--< el-image-->
              <!--:ref="'img'+index"-->
              <!--:style="{width: '100%',height:navImgHeight+'px'}"-->
              <!--:class="{toScale:item.ifShow}"-->
              <!--:src="item.imgUrl"-->
              <!--fit="cover"></el-image>-->
            <p><span>——</span> <span class="textS">{{item.nm}}</span> <span>——</span></p>
          </div>
        </div>
      </div>
      <!-- <div class="shipDesign" id="shipDesign">
        <div class="head">
          <img :src="navImg[1] ? navImg[1].imgUrl : ''"><div><p>{{navImg[1] ? navImg[1].nm : ''}}</p><button class="point" @click="toPage(navList[0].locUrl)">查看详情</button></div>
        </div>
        <div class="tab" :style="{width:width+'px'}">
          <div class="tabImgBox point" :style="{height:tabImgHeight+'px'}" v-for="(item,index) in tabList1" @click="toPage(item.href)" @mouseenter="changeLiTrans(index,1,2)" @mouseleave="changeLiTrans(index,0,2)">
            <img :src="whiteJ" v-if="index==0">
            <div v-show="!item.ifTrans" class="normal"><img :src="item.img"><p>{{item.nm}}</p></div>
            <div v-show="item.ifTrans" class="transContent toScale" :style="{width:tabImgHeight*1.2+'px',background:item.color}">
              <div><img :src="item.img2"/><p>{{item.nm}}</p></div>
            </div>
          </div>
        </div>
        <img :src="designStep" :style="{width:width+'px'}" id="designL">
      </div> -->

     <!-- <div class="shipBuild" id="shipBuild">
        <div class="head" >
          <img :src="navImg[0] ? navImg[0].imgUrl : ''"><div><p>{{navImg[0] ? navImg[0].nm : ''}}</p><button class="point" @click="toPage(navList[1].locUrl)">查看详情</button></div>
        </div>
        <div class="tab" :style="{width:width+'px'}">
          <div class="tabImgBox point" :style="{height:tabImgHeight+'px'}" v-for="(item,index) in tabList2" @click="toPage(item.href)" @mouseenter="changeLiTrans(index,1,3)" @mouseleave="changeLiTrans(index,0,3)">
            <img :src="greyJ" v-if="index==0">
            <div v-show="!item.ifTrans" class="normal"><img :src="item.img"><p>{{item.nm}}</p></div>
            <div v-show="item.ifTrans" class="transContent toScale" :style="{width:tabImgHeight*1.2+'px',background:item.color}">
              <div><img :src="item.img2"/><p>{{item.nm}}</p></div>
            </div>
          </div>
        </div>
        <img :src="buildStep" :style="{width:width+'px'}" id="buildL">
      </div> -->

      <!-- <div class="quality" id="quality">
        <p class="headMore"><img :src="moreIc" style="opacity: 0;"><span>品质保障</span><img :src="moreIc" alt=""></p>
        <img v-if="PCjyjc.length>0" :src="PCjyjc[0].imgUrl" @click="toPage(PCjyjc[0].locUrl)">
        <img v-if="PCjljz.length>0" :src="PCjljz[0].imgUrl" @click="toPage(PCjljz[0].locUrl)">
      </div> -->

<!--      <div class="customer" :style="{backgroundImage:'url('+custBg+')'}">-->
      <div class="customer">
        <p class="headMore" :style="{width:width+'px'}"><img :src="moreIc" style="opacity: 0;"><span>合作单位</span><img :src="moreIc" alt=""></p>
        <div class="unitTab" :style="{width:width+'px'}">
          <p @click="tabCd=item.cd" :class="{clickP:tabCd==item.cd}" class="point" v-for="item in designTabList" :key="item.cd">{{item.nm}}</p>

        </div>
        <div class="unitBox" :style="{width:width+'px'}">
          <div v-for="(item,index) in unitList" style="cursor: pointer;" >
            <p>
              <img :src="item.logo" @click="toDesignDetail(item)">
            </p>
            <!-- <span>
              {{item.company}}
            </span> -->
          </div>
        </div>
        <p class="headMore" :style="{width:width+'px'}"><img :src="moreIc" style="opacity: 0;"><span>典型客户</span><img :src="moreIc" alt=""></p>
        <div class="unitBox" :style="{width:width+'px'}">
          <div v-for="(item,index) in custList">
            <p>
              <img :src="item.imgUrl">
            </p>
            <!-- <span>
              {{item.nm}}
            </span> -->
          </div>
        </div>
      </div>

      <!-- <div class="videoBox" :style="{width:width+'px'}" v-if="video.length>0">
        <p class="title" >{{video[0].nm}}</p>
        <video :src="video[0].locUrl" controls="controls" :poster="video[0].imgUrl">
        </video>
        <div class="videoCont" v-html="video[0].showContent"></div>
      </div> -->

    </div>
</template>

<script>
    import moreIc from '@/assets/img/shipDisplay/更多.png'
    // import tab1 from '@/assets/img/shipDisplay/设计流程.png'
    // import tab2 from '@/assets/img/shipDisplay/diy.png'
    // import tab3 from '@/assets/img/shipDisplay/设计预约.png'
    // import tab4 from '@/assets/img/shipDisplay/设计规范.png'
    // import tab5 from '@/assets/img/shipDisplay/品质保障.png'
    // import tabW1 from '@/assets/img/shipDisplay/设计流程-白.png'
    // import tabW2 from '@/assets/img/shipDisplay/diy-白.png'
    // import tabW3 from '@/assets/img/shipDisplay/设计预约-白.png'
    // import tabW4 from '@/assets/img/shipDisplay/设计规范-白.png'
    // import tabW5 from '@/assets/img/shipDisplay/品质保障-白.png'
    // import whiteJ from '@/assets/img/shipDisplay/白.jpg'
    // import greyJ from '@/assets/img/shipDisplay/灰1.jpg'
    // import designStep from '@/assets/img/shipDisplay/船舶设计流程.png'
    // import buildStep from '@/assets/img/shipDisplay/船舶建造流程.png'
    // import custBg from '@/assets/img/shipDisplay/custBg.png'
    import {mapState} from "vuex";
    import designAppo from "@/components/designAppo";
    export default {
        name: "shipCat",
        components: {designAppo},
        data(){
            return{
              // whiteJ,
              // greyJ,
              moreIc,
              // designStep,
              // buildStep,
              // custBg,
              designTabList:[],
              navList:[],
              navImg:[],
              appoShow:false,
              // tabList1:[{
              //   nm:'设计流程',
              //   img:tab1,
              //   img2:tabW1,
              //   color:'#3EA708',
              //   cont:'设计流程简介：',
              // },{
              //   nm:'DIY报价',
              //   img:tab2,
              //   img2:tabW2,
              //   color:'#EC51EE',
              //   cont:'DIY报价简介：',
              //   href:'./shipDIY?tab=1',
              // },{
              //   nm:'设计预约',
              //   img:tab3,
              //   img2:tabW3,
              //   color:'#FF5931',
              //   cont:'设计预约简介：',
              //   href:'popup'
              // },{
              //   nm:'设计规范',
              //   img:tab4,
              //   img2:tabW4,
              //   color:'#00BAB4',
              //   cont:'设计规范简介：',
              //   href:'./designRule',
              // }],
              // tabList2:[{
              //   nm:'建造流程',
              //   img:tab1,
              //   img2:tabW1,
              //   color:'#3EA708',
              //   cont:'建造流程简介：',
              //   // href:'/',
              // },{
              //   nm:'DIY报价',
              //   img:tab2,
              //   img2:tabW2,
              //   color:'#EC51EE',
              //   cont:'DIY报价简介：',
              //   href:'./shipDIY?tab=2',
              // },{
              //   nm:'品质保障',
              //   img:tab5,
              //   img2:tabW5,
              //   color:'#FF5931',
              //   cont:'品质保障简介：',
              //   href:'./quality',
              // },{
              //   nm:'建造规范',
              //   img:tab4,
              //   img2:tabW4,
              //   color:'#00BAB4',
              //   cont:'建造规范简介：',
              //   href:'./buildRule',
              // }],
              // PCjyjc:[],
              // PCjljz:[],
              unitList:[],
              custList:[],
              adertList:[],
              bannerHeight:'300',
              navImgHeight:'160',
              tabImgHeight:'285',
              // video:[],//视频
              tabCd:1,
            }
        },
      computed: {
        ...mapState([
          'bWidth',
          'width'
        ]),
      },
      async asyncData(context){
        if(process.client){
          return
        }
        let arr = []
        arr = await Promise.all([context.app.api.getAdert('shipAdert'),
          context.app.api.getAdert('shipManageMenu'),
          context.app.api.getAdert('shipManNav'),
          context.app.api.getAdert('successCust'),
          // context.app.api.getAdert('PCjyjc',1),
          // context.app.api.getAdert('PCjljz',1)
        ]),
          context.app.api.getRoleList()
        return {
          arr:arr,
        }
      },
      created(){
        // let navImg=document.getElementById("navImg");
        // console.log('宽度'+navImg.clientWidth)
      },
      watch:{
        width(){
          this.getHeight()
        },
        tabCd(){
          let qry = this.query.new()
          this.query.toW(qry,'identityCd',this.tabCd,'EQ')
          this.query.toP(qry,1,20)
          this.api.getRoleAll(this.query.toEncode(qry)).then(res=>{
            this.unitList = res
          })
        }
      },
      mounted() {
        this.getData()
        if(this.until.getQueryString('toSee')=='designL'){
          this.toScroll(3)
        }
        if(this.until.getQueryString('toSee')=='buildL'){
          this.toScroll(4)
        }
      },
      methods:{
        async getData(){
          await this.getAdert()
          await this.getCompany()
        },
        //跳转到指定位置
        toScroll(index){
          let pos=''
          if(index==0){
            pos = 'shipDesign'
          }else if(index==1){
            pos = 'shipBuild'
          }else if(index==2){
            pos = 'quality'
          }else if(index==3){
            pos = 'designL'
          }else if(index==4){
            pos = 'buildL'
          }
          document.getElementById(pos).scrollIntoView()
        },
        //鼠标滑动切换放大效果
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
        //合作单位
        async getCompany(){
          let designTabList = await this.api.getRoleList()//入驻机构
          this.designTabList = []
          designTabList.forEach(item=>{
            if(item.cd=='identity20' || item.cd == 'identity30' || item.cd=='identity40' || item.cd=='identity50'){
              this.designTabList.push(item)
            }
          })
          this.tabCd = this.designTabList[0].cd
        },
        close() {
          // 取消弹窗回调
          this.appoShow = false
        },
        submit() {
          // 确认弹窗回调
          this.appoShow = false
        },
        //页面跳转
        toPage(url){
          console.log(url)
          if(url){
            console.log(url.indexOf('/')>-1)
            if(url.indexOf('/')>-1){
              this.$router.push(url)
            }else {
              this.appoShow=true
            }
          }

        },
        //获取广告轮播
        async getAdert(){
          this.adertList = await this.api.getAdert('shipAdert')
          if(this.adertList.length>0){
            let myImg = new Image()
            myImg.src = this.adertList[0].imgUrl
            myImg.onload = ()=>{
              this.bannerHeight = myImg.height
            }
          }
          this.adertList.forEach((item,index)=>{
            this.objectFitImages(this.$refs['img'+index])
          })
          this.navList = await this.api.getAdert('shipManageMenu')
          console.log('导航菜单')
          console.log(this.navList)
          this.navImg = await this.api.getAdert('shipManNav')
          this.custList = await this.api.getAdert('successCust')
          console.log('客户')
          console.log(this.custList)
          this.getHeight()
          // this.PCjyjc = await this.api.getAdert('PCjyjc',1)
          // this.PCjljz = await this.api.getAdert('PCjljz',1)
          //视频
          // this.video = await this.api.getAdert('PCVideo',1)
        },
        //获取高度，达到1：1
        getHeight(){
          this.navImgHeight=this.width*0.24*9/16;
          this.tabImgHeight=this.width*0.24;
          console.log('nav : '+this.navImgHeight+'tab : '+this.tabImgHeight)
        },
        //跳到设计或船厂详情
        toDesignDetail(item){
          let url = ''
          if(item.identityCd=='identity20'){//设计院
            url = '/shipDisplay/design?id='+item.id
          }else if(item.identityCd=='identity30'){ //船厂
            url = '/shipDisplay/shipyard?id='+item.id
          }else if(item.identityCd=='identity40'){ //监理单位
            url = '/shipDisplay/JLDetail?id='+item.id
          }
          this.$router.push(url)
        },
      }
    }
</script>

<style scoped lang="less">
  @import url("../../assets/css/init.less");
  .adert{
    width: 100%;
    text-align: center;
    /*max-height: 350px;*/
    .bannerImg{
      height: 100%;
      background-position: center;
    }
  }
  .cont{
    margin: 0 auto;
    /*padding-bottom: 121px;*/
    /*background: url("../../assets/img/bg.png") no-repeat bottom center;*/
    background-color: #FFFFFF;
    .point{
      cursor: pointer;
    }
    .navBox{
      background-color: #F5F5F5;
      .nav{
        display: flex;
        justify-content: space-between;
        padding:30px 0;
        margin: 0 auto;
        .navImgBox{
          width: 24%;
          position: relative;
          overflow: hidden;
          /*.el-image{*/
            /*width: 100%;*/
            /*height: 100%;*/
          /*}*/
          .el-image{
            margin: auto;
            max-width: 100%;
            max-height: 100%;
            overflow: visible;
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
    .shipDesign{
        background-color: #FFFFFF;
    }
    .shipBuild{
        background-color: #F5F5F5;
    }
    .shipDesign,.shipBuild{
      padding-bottom: 60px;
      .head{
        width: 100%;
        margin:60px 0 20px;
        position: relative;
        display: flex;
        justify-content: center;
        align-items: center;
        >img{
          width: 100%;
        }
        >div{
          position: absolute;
          >p{
            font-size:39px;
            font-family:FZLanTingHei-DB-GBK;
            color:rgba(255,255,255,1);
            margin-bottom: 22px;
            line-height: 39px;
            letter-spacing: 12px;
          }
          >button{
            background:rgba(39,120,190,1);
            color: #FFFFFF;
            font-size:16px;
            padding: 9px 15px;
            border: none;
            margin: 0 auto;
            display: block;
          }
        }
      }
      .tab{
        display: flex;
        justify-content: space-between;
        padding-bottom:50px;
        margin: 0 auto;
        .tabImgBox{
          width: 24%;
          display: flex;
          justify-content: center;
          align-items: center;
          position: relative;
          background-color: #FFFFFF;
          box-shadow: 0 0 3px 3px rgba(0,0,0,0.10);
          >img{
            position: absolute;
            bottom: -5px;
            left: 46%;
          }
          .normal{
            z-index: 2;
            text-align: center;
            >img{
              width: 50px;
              height: 50px;
              margin-bottom: 23px;
            }
            >p{
              font-size:22px;
              color:rgba(102,102,102,1);
            }
          }
          .transContent{
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            padding: 0 10px;
            height: 100%;
            box-shadow:0px 5px 13px 0px rgba(86,86,86,0.33);
            >div{
              display: flex;
              align-items: center;
              margin-bottom: 30px;
              >img{
                width: 30px;
                margin-right: 8px;
              }
              >p{
                color: #FFFFFF;
                font-size: 16px;
              }
            }
            >p{
              color: rgba(255, 255, 255, 0.7);;
              font-size: 12px;
            }
          }
        }
      }
      >img{
        width: 76%;
        margin: 4px auto 60px;
        display: block;
      }
    }
    .quality,.customer{
      padding-top: 58px;
      .headMore{
        margin: 0 auto;
        text-align: center;
        line-height: 32px;
        font-size:20px;
        >span{
          font-weight:500;
          color:rgba(51,51,51,1);
          margin: 0 11px;
        }
        >img{
          width:16px;
          height:16px;
        }
      }
    }
    .quality{
      background-color: #FFFFFF;
      >img{
        margin-top: 1px;
        width: 100%;
        display: block;
        cursor: pointer;
      }
      .headMore{
          margin-bottom: 48px;
      }
    }
    .customer{
      background-image: url("../../assets/img/shipDisplay/custBg.png");
      padding: 30px 0 10px;
      .unitTab{
        border-bottom: 1px solid #2778BE;
        height: 40px;
        display: flex;
        margin: 59px auto 0;
        >p{
          display: flex;
          justify-content: center;
          align-items: center;
          padding: 14px 44px;
          height: 100%;
          color: #666666;
          font-size:14px;
        }
        .clickP{
          background-color: #2778BE;
          color: #FFFFFF;
        }
      }
      .unitBox{
               margin: 0 auto 30px;
               display: flex;
               flex-wrap: wrap;
               >div{
                 margin-top: 30px;
                 width: 18%;
                 // height: 110px;
                 margin-right: 2.5%;
                 display: flex;
                 justify-content: center;
                 align-items: center;
                 background-color: #FFFFFF;
                 border:1px solid rgba(217, 217, 217, 1);
                 // padding: 17px 0;
                 display: flex;
                 flex-direction: column;
                 >p{
                   width: 100%;
                   height: 100%;
                   >img{
                     width: 100%;
                     height: auto;
                     // line-height: 100px;
                   }
                 }

                 >span{
                   bottom: 2px;
                   font-weight: 700;
                   font-size: 13px;
                   margin-top: 10px;
                 }
               }
               >div:nth-child(5n){
                 margin-right: 0;
               }
             }
    }
    .videoBox{
      background-color: #FFFFFF;
      padding: 60px 30px 0;
      margin: 0 auto;
      .title{
        margin-bottom: 39px;
        font-size:30px;
        font-family:PingFang SC;
        font-weight:500;
        color:rgba(48,48,48,1);
        line-height: 30px;
        text-align: center;
      }
      >video{
        width: 100%;
        // height:460px;
        // margin-bottom: 31px;
      }
      .videoCont{
        padding: 31px 72px 0 29px;
        font-size:15px;
        color:rgba(102,102,102,1);
        line-height: 30px;
      }
    }
  }

</style>
