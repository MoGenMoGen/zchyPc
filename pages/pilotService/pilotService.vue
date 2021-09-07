<template>
  <!-- 试点服务 -->
  <div id="home" :style="{width:bWidth + 'px'}">
    <div class="Carousel">
      <el-carousel trigger="click" :height="bannerHeight+'px'">
        <el-carousel-item v-for="item in adertList" :key="item.id">
          <div class="bannerImg" :style="{backgroundImage:'url('+item.imgUrl+')'}" @click="toPage(item.locUrl)">
          </div>
        </el-carousel-item>
      </el-carousel>
    </div>

    <div class="info" :style="{width:width + 'px'}">
      <div class="left">
        <div class="title1">
          <p><img src="@/assets/img/pilotService/信息公开.png"/>{{newTitle1}}</p>
          <p class="more" @click="toPage('../pilotService/serviceInfo?nm=信息公开&id='+'5074848269325312')">更多 >></p>
        </div>
        <div class="tab">
          <p v-for="(item,index) in messageList" :key="index" :class="{checkedP:item.id==mId}" @click="changeTab(item,1)">{{item.nm}}</p>
        </div>
        <div class="body">
          <!--<div class="lf point" @click="toDetail(lfItem)">-->
            <!--<img :src="lfImg" v-if="lfImg"/>-->
          <!--</div>-->
          <div class="rt">
            <p v-for="(item,index) in infoList" :key="index" @click="toDetail(item)" :class="{seen:item.isSeen}" @mouseenter="enter(index)" @mouseleave="leave(index)" ><span>· {{item.nm}}</span><span>{{item.releTm}}</span></p>
          </div>
        </div>
      </div>
      <div class="right">
        <div class="title1">
          <p><img src="@/assets/img/pilotService/标准建设.png"/>{{newTitle2}}</p>
          <p class="more" @click="toPage('../pilotService/serviceInfo??nm=标准建设&id='+ '5074854473782272')">更多 >></p>
        </div>
        <div class="tab">
          <p v-for="(item,index) in jianList" :key="index" :class="{checkedP:item.id==jId}" @click="changeTab(item,2)">{{item.nm}}</p>
        </div>
        <div class="body">
          <!--<div class="lf point" @click="toDetail(rfItem)">-->
            <!--<img :src="rfImg" v-if="rfImg"/>-->
          <!--</div>-->
          <div class="rt">
            <p v-for="(item,index) in zbList" :key="index" @click="toDetail(item)" :class="{seen:item.isSeen}" @mouseenter="enter(index)" @mouseleave="leave(index)" ><span>· {{item.nm}}</span><span>{{item.releTm}}</span></p>
          </div>
        </div>
        <!-- <div class="body">
          <div class="top">
            <img :src="topImg" v-if="topImg" alt="topImg" class="point" @click="toDetail(topItem)"/>
            <div @click="toPage({path:'../tradeInfo/infoDetail',query:{id:topId,cid:topCid}})">
              <p class="p-title point" @click="toDetail(topItem)">{{topP}}</p>
              <P class="p-msg point" @click="toDetail(topItem)">{{topCont}}</P>
            </div>
          </div>
          <div class="down">
            <p v-for="(item,index) in zbList" :key="index" @click="toDetail(item)" :class="{seen:item.isSeen}" @mouseenter="enter2(index)" @mouseleave="leave2(index)">
              <span>· {{item.nm}}</span><span>{{item.releTm}}</span></p>
          </div>
        </div> -->

      </div>
    </div>

    <!-- 质量管控  办事窗口 -->
    <div class="whiteBg" :style="{width:bWidth+'px'}">
      <div class="conANDwin main" :style="{width:width+'px'}">
        <div class="title3 control" style="width: 76.25%;">
          <p><span class="lineC"></span>质量管控</p>
          <div class="controlBox">
            <div class="adertNav point"
                 v-for="(item,index) in controlList"
                    @mouseenter="changeLiTrans(index,1,1)"
                    @mouseleave="changeLiTrans(index,0,1)"
                    :style="{height:navImgHeight+'px'}"
                    @click="toPage(item.locUrl)">
                 <el-image
                   style="width: 100%;height: 100%"
                   :class="{toScale:item.ifShow}"
                   :src="item.imgUrl"
                   fit="cover"></el-image>
              <p><span>——</span> <span class="textS">{{item.nm}}</span> <span>——</span></p>
            </div>
          </div>
        </div>

        <div class="title3 window" style="width: 23.75%;">
          <p><span class="lineC"></span>办事窗口</p>
          <div class="windowBox" :style="{height:navImgHeight+'px'}" >
            <div style="margin-bottom: 26px;" class="point" @click="toPage('./applyFor')">
              <p style="margin-right: 8px;"><img :src="SQ" alt=""></p>
              <p>补贴在线申请</p>
            </div>
            <div class="point" @click="toPage('./onlineInquiry')">
              <p style="margin-right: 8px;"><img :src="CX" alt=""></p>
              <p>补贴申请查询</p>
            </div>
          </div>
        </div>
      </div>

    </div>

    <!-- 船舶展示 -->
    <div class="whiteBg shuzi" :style="{width:bWidth+'px'}">
      <div class="title2 main"  :style="{width:width+'px'}">
        <p><span class="lineC"></span>标准船型展示</p>
        <span @click="toPage('/shipDisplay/shipCat')">更多 >></span>
      </div>

      <div class="sfq main"  :style="{width:width+'px',height:(width-30)/4*164/256+'px'}">
         <!-- <div v-for="(item,index) in shipClassify" :key="index"
              :style="{backgroundImage:`url('${item.imgUrl}')`}"
              :class="{hiddenItem:!item.ifShow,showItem:item.ifShow}"
              @click="toPageTest()"
              @mouseenter="changeLiTrans(index,1,4)"
              @mouseleave="changeLiTrans(index,0,4)"> -->
              <!-- @click="toPage(`/shipDisplay/shipShow?cId=${item.id}&cNm=${item.nm}&cImg=${item.imgUrl2}`)" -->
        <div v-for="(item,index) in shipClassify" :key="index"
             :style="{height:(width-30)/4*164/256+'px'}"
             @click="toPage2(`/shipDisplay/shipShow?cId=${item.id}&cNm=${item.nm}&cImg=${item.icon1}`)"
             @mouseenter="changeLiTrans(index,1,4)"
             @mouseleave="changeLiTrans(index,0,4)"
             >
           <el-image
             style="width: 100%;height: 100%"
             :class="{toScale:item.ifShow}"
             :src="item.imgUrl"
             fit="cover"></el-image>
          <!-- <div class="hiddenContent"> -->
            <p class="textB"><span>——</span> <span class="textS">{{item.nm}}</span> <span>——</span></p>
          <!-- </div> -->
         </div>
      </div>
    </div>

    <!-- 材料和装备 -->
    <div class="whiteBg hotEquip" :style="{width:bWidth+'px'}">
      <div class="title2 main"  :style="{width:width+'px'}">
        <p><span class="lineC"></span>材料和装备</p>
        <span  @click="toPage('/Trade/productTrading')">更多 >></span>
      </div>
      <div class="ulBox main midSwiper" :style="{width:width+'px'}">

        <swiper class="swiper" :options="swiperOption">
            <swiper-slide v-for="(item,index) in equipList" :key="index" v-if="equipList.length>0">
              <div class="productSwiper point">
                <div @click="toPage('../Trade/productDetail?type=product&id='+item.goodsId)">
                  <img :src="item.img"
                    class="el-images"
                    :ref="'img'+index"
                  />
                </div>
                <el-tooltip :content="item.nm" placement="top" effect="light">
                  <p class="p">{{item.nm}}</p>
                </el-tooltip>
              </div>
            </swiper-slide>
            <div class="swiper-button-prev swiper-button-white swiperBtn" style="left: 0;" slot="button-prev">
              <img :src="btR" alt="" style="transform: rotate(180deg);"/>
            </div>
            <div class="swiper-button-next swiper-button-white swiperBtn" style="right: 0;" slot="button-next">
              <img :src="btR" alt=""/>
            </div>
        </swiper>
      </div>
    </div>


    <div class="footerImg" :style="{width:bWidth+'px'}">
      <img src="@/assets/img/footer.png" alt=""/>
    </div>
  </div>
</template>

<script>
  import btR from '@/assets/img/personal/rightArrow.png';
  import CX from '@/assets/img/pilotService/补贴查询.png';
  import SQ from '@/assets/img/pilotService/补贴申请.png';
  import XXGK from '@/assets/img/pilotService/信息公开.png';
  import BZJS from '@/assets/img/pilotService/标准建设.png';
  // import test1 from '@/assets/img/pilotService/玻璃钢渔船.png';
  // import test2 from '@/assets/img/pilotService/玻璃钢渔船-1.png';
  // import test3 from '@/assets/img/pilotService/玻璃钢渔船-2.png';
  // import test4 from '@/assets/img/pilotService/玻璃钢休闲渔船.png';
  import {mapState} from "vuex";
    export default {
        name: "service",
        data(){
            return{
              btR,
              CX,
              SQ,
              XXGK,
              BZJS,
              // test1,
              // test2,
              // test3,
              // test4,
              adertList:[], //轮播图
              bannerHeight:0,
              newTitle1:'信息公开',
              newTitle2:'标准建设',
              lfImg:'',
              lfItem:{},
              rfImg:'',
              rfItem:{},
              infoList:[],//导航信息的列表
              zbList:[],//招标
              messageList:[],
              jianList:[],
              mId:'',
              jId:'',

              controlList:[], //质量管控列表
              shipClassify:[
                // {imgUrl: test1 ,ifShow:true,nm:'玻璃钢渔船'},
                // {imgUrl: test2 ,ifShow:false,nm:'玻璃钢渔船'},
                // {imgUrl: test3 ,ifShow:false,nm:'玻璃钢渔船'},
                // {imgUrl: test4 ,ifShow:false,nm:'玻璃钢休闲渔船'}
              ],
              //船舶展示列表


              swiperOption: {   //热门设备轮播 样式设置
                slidesPerView: 5,
                spaceBetween: 1,
                freeMode: true,
                navigation: {
                  nextEl: '.swiper-button-next',
                  prevEl: '.swiper-button-prev'
                }
              },
              equipList:[],//热门设备
              navImgHeight:'',
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
        width(){
          this.getHeight()
        },
        mId(){
          this.getMList(this.mId)
        },
        jId(){
          this.getJList(this.jId)
        },
      },
      async asyncData(context){
        let arr = await Promise.all([
          context.app.api.getAdert('SDFWAdert'),
          context.app.api.getAdert('SDFW'),
        ])
        return {
          arr:arr
        }
      },
      mounted(){
        this.navImgHeight=this.width*0.762/3*9/16;
        this.getData()
        this.getService()
        this.getShip()
        this.getEquipList()
        this.getSevenA()
      },
      methods:{
        async getService(){
          let qry = this.query.new()
          this.query.toW(qry,'pid','5074848269325312','EQ')
          this.query.toO(qry,'seq','asc')
          this.messageList = await this.api.serviceColumn(this.query.toEncode(qry))
          this.mId=this.messageList[0].id


          console.log('tab1')
          console.log(this.messageList)
          let qry1 = this.query.new()
          this.query.toW(qry1,'pid','5074854473782272','EQ')
          this.query.toO(qry1,'seq','asc')
          this.jianList = await this.api.serviceColumn(this.query.toEncode(qry1))
          this.jId=this.jianList[0].id
          console.log('tab2')
          console.log(this.jianList)
        },
        async getMList(id){
          let qry = this.query.new()
          this.query.toW(qry,'cid',id,'EQ')
          this.query.toO(qry,'seq','desc')
          this.query.toP(qry,'1','6')
          let data = await this.api.serviceContent(this.query.toEncode(qry))
          this.infoList=data.data.list
          if(this.infoList.length>0){
            this.lfImg=this.infoList[0].imgUrl
            this.lfItem=this.infoList[0]
          }else {
            this.lfImg=''
            this.lfItem=null
          }

          console.log('infolIst')
          console.log(this.infoList)
        },
        async getJList(id){
          let qry = this.query.new()
          this.query.toW(qry,'cid',id,'EQ')
          this.query.toO(qry,'seq','desc')
          this.query.toP(qry,'1','6')
          let data = await this.api.serviceContent(this.query.toEncode(qry))
          this.zbList=data.data.list
          if(this.zbList.length>0){
            this.rfImg=this.zbList[0].imgUrl
            this.rfItem=this.zbList[0]
          }else {
            this.rfImg=''
            this.rfItem=null
          }

          console.log('zbList')
          console.log(this.zbList)
        },
        async getData(){
          this.adertList = await this.api.getAdert('SDFWAdert')
          if(this.adertList.length>0){
            let myImg = new Image()
            myImg.src = this.adertList[0].imgUrl
            myImg.onload = ()=>{
              // this.bannerHeight = myImg.height/myImg.width*this.bWidth
              this.bannerHeight = myImg.height
            }
          }
          this.navList = await this.api.getAdert('PTFW')
          this.navList.forEach((item,index)=>{
            this.objectFitImages(this.$refs['img'+index])
          })
        },
        //热门设备
        async getEquipList(){
          this.equipList = await this.api.shopRecomdPro()
          this.equipList.forEach((item,index)=>{
            this.objectFitImages(this.$refs['img'+index])
          })
          // console.log('热门设备')
          // console.log(this.equipList)
          this.equipList.splice(15)
        },
        async getSevenA(){
          this.controlList = await this.api.getAdert('controlList')
          console.log('质量管控列表')
          console.log(this.controlList)
        },
        // 获取船舶分类信息
        async getShip(){
          let qry = this.query.new()
          this.query.toW(qry,'pid','4942035725390848','EQ')
          this.query.toO(qry,'seq','asc')
          this.shipClassify = await this.api.getClassify(this.query.toEncode(qry))
          this.shipClassify = this.shipClassify.slice(0,4)
          // for(let i=0;i<this.shipClassify.length;i++){
          //   let qry2 = this.query.new()
          //   this.query.toW(qry2,'pid',this.shipClassify[i].id,'EQ')
          //   this.query.toO(qry2,'seq','asc')
          //   let data = await this.api.getClassify(this.query.toEncode(qry2))
          //   this.shipClassify[i].list = data
          //   this.shipClassify[i].imgUrl = this.shipClassify[i].imgUrl ? this.shipClassify[i].imgUrl.split(',')[0] : ''
          //   // if(i==0){
          //     // this.shipClassify[0].ifShow = true
          //   // }else{
          //     this.shipClassify[i].ifShow = false
          //   // }
          //   this.$set(this.shipClassify,i,this.shipClassify[i])
          // }
          console.log('大大大')
          console.log(this.shipClassify)
        },
        toDetail(item){
          if(item.cids){
            item.pid=item.cids.split(",")[0]
          }
          console.log(item.pid)
          this.$router.push({
            path:'../pilotService/infoDetail',
            query:{
              id:item.pid,
              cid:item.cid,
              sid:item.id
            }
          });

          // this.$router.push({
          //   path:'./infoDetail',
          //   query:{
          //     id:item.id,
          //     cid:item.cid
          //   }
          // });
        },

        //鼠标滑动效果  索引，类型（1滑入，0滑出）
        changeLiTrans(index,type,nm){
          if(nm==1){
            this.controlList[index].ifShow = type ? true : false
            this.$set(this.controlList, index, this.controlList[index])
          }
          // if(nm==2){
          //     this.sevenList2[index].ifShow = type ? true : false
          //     this.$set(this.sevenList2,index,this.sevenList2[index])
          // }
          // if(nm==3){
          //     this.sevenList3[index].ifShow = type ? true : false
          //     this.$set(this.sevenList3,index,this.sevenList3[index])
          // }
          if(nm==4){
            this.shipClassify[index].ifShow = type ? true : false
            // if(type){
            //   this.shipClassify[0].ifShow = false
            //   this.shipClassify[index].ifShow =  true
            // }else{
            //   this.shipClassify[index].ifShow =  false
            //   this.shipClassify[0].ifShow = true
            // }
            this.$set(this.shipClassify,index,this.shipClassify[index])
          }
        },
        changeTab(item,type){
          if(type==1){
            this.mId=item.id

          }else if(type==2){
            this.jId=item.id
          }
        },
        enter(index){
          this.infoList[index].isSeen = true
          this.$set(this.infoList,index,this.infoList[index])
          // this.lfImg = this.infoList[index].imgUrl.0
        },
        leave(index){
          this.infoList[index].isSeen = false
          this.$set(this.infoList,index,this.infoList[index])
        },
        enter2(index){
          this.zbList[index].isSeen = true
          this.$set(this.zbList,index,this.zbList[index])
        },
        leave2(index){
          this.zbList[index].isSeen = false
          this.$set(this.zbList,index,this.zbList[index])
        },
        toPageTest(){
          let url= '/shipDisplay/shipShow?cId=4961563628229632&cNm=休闲渔船&cImg=https%3A%2F%2Fsinovat.oss-cn-shanghai.aliyuncs.com%2Fbabcdff2b9d640c585f99a23b482d1ae-休闲渔船.png'
          this.$router.push(url)
        },
        toPage(url){
          if(url){
            let arr = url.split('^')
            if(arr[0].indexOf('http')>-1){
              location.href = arr[0]
            }else {
              this.$router.push(arr[0])
            }
          }
        },
        toPage2(url){
          if(url){
            this.$router.push(url)
          }
        },
        //获取高度，达到1：1
        getHeight(){
          this.navImgHeight=this.width*0.24*9/16;
        },
      }
    }
</script>
<style lang="less">
  .ulBox{
    .swiper-container{
      // padding: 0 40px;
      .p{
        margin: 10px auto;
        font-size: 15px;
        text-align: center;
      }
    }
    .swiper-slide{
      margin: 0 2% !important;
      width: 16% !important;
    }
    .swiperBtn{
      background: none;
      width: 15px;
      top: 42%;
    }
  }
</style>
<style lang="less" scoped>
  @import url("../../assets/css/init.less");
  #home{
    background-color: #F5F5F5;
    .Carousel{
      width: 100%;
      .bannerImg{
        height: 100%;
        background-position: center;
      }
    }
    .header{
      /*margin-top: 45px;*/
      display: flex;
      flex-direction: row;
      justify-content: space-between;
      .navBox{
        .nav{
          display: flex;
          justify-content: space-between;
          padding:30px 0;
          margin: 0 auto;
          .navImgBox{
            width: 24%;
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
    .conANDwin{
      display: flex;
      .control{
        // flex: 3;
        .controlBox{
          display: flex;
          justify-content: space-between;
          margin: 20px 0 0;
          .adertNav{
            position: relative;
            overflow: hidden;
            /*display: inline-block;*/
            width: 33.33%;
            margin-right: 20px;
            >img{
              width: 100%;
              height: 100%;
              max-width: 200%;
              max-height: 200%;
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
                margin: 0 0.15rem;
                opacity: 1;
                font-size:19px;
              }
              >span{
                opacity: 0.2;
              }
            }
          }
        }
      }
      .window{
        // flex: 1;
        .windowBox{
          background-color: #F1F3F1;
          display: flex;
          flex-direction: column;
          justify-content: center;
          align-items: center;
          // padding: 29px 0;
          >div{
            width: 71.2%;
            padding: 14px 0;
            background: #2777BD;
            display: flex;
            align-items: center;
            justify-content: center;
            >p{
              display: flex;
              font-size: 16px;
              color: #ffffff;
              text-align: center;
              >img{
                width: 19px;
                height: 20px;
              }
            }
          }
        }
      }
    }
    .shuzi{
      /*数字平台*/
      // padding-top: 20px;
      >ul{
        box-sizing: border-box;
        border-left: 1px solid rgba(0,0,0,0.2);
        border-top: 1px solid rgba(0,0,0,0.2);
        margin-bottom: 20px;
        li{
          border-right: 1px solid rgba(0,0,0,0.2);
          border-bottom: 1px solid rgba(0,0,0,0.2);
          box-sizing: border-box;
          width: 25%;
          display: inline-block;
          height: 300px;
          cursor: pointer;
          position: relative;
          font-size: 16px;

          .showContent{
            width: 100%;
            height: 100%;
            display: flex;
            display: -webkit-flex;
            align-items: center;
            justify-content: center;
            flex-direction: column;
            img{
              width: 55px;
              height: auto;
              margin-bottom: 15px;
            }
            p{
              text-align: center;
            }
          }
          .transContent{
            width:100%;
            height:300px;
            box-shadow:0px 5px 13px 0px rgba(86,86,86,0.33);
            position: absolute;
            top:-1px;
            left: -1px;
            padding: 61px 21px 21px;
            box-sizing: border-box;
            color: rgba(255,255,255,0.7);
            font-size: 12px;
            z-index: 9;

            p{
              display: flex;
              display: -webkit-flex;
              align-items: center;
              justify-content: center;
              margin-bottom: 30px;
              font-size: 16px;
              color: rgba(255,255,255,1);
              img{
                width: 30px;
                margin-right: 8px;
              }
            }
          }

        }
      }

    }
    /*手风琴*/
    .sfq{
      display: flex;
      display: -webkit-flex;
      height: 240px;
      // margin-bottom: 60px;
      >div{
        flex: 1;
        height: 100%;
        overflow: hidden;
        cursor: pointer;
        background-size: cover;
        background-position: center;
        margin-right: 10px;
        position: relative;
        .textB{
          width: 100%;
          background-color: rgba(0,0,0,0.35);
          position: absolute;
          padding: 8px 0;
          bottom: 0;
          color:rgba(255,255,255,1);
          text-align: center;
          .textS{
            margin: 0 10px;
            opacity: 1;
            font-size:19px;
            font-weight: 600;
          }
          >span{
            opacity: 0.2;
          }

        }
        .showContent{
          width: 100%;
          height: 100%;
          background: rgba(39, 120, 190, 0.7);

          box-sizing: border-box;
          color: #ffffff;

          p{
            padding-top: 150px;
            text-align: center;
            font-size: 24px;
          }
          .cont{
            font-size: 16px;
            padding-top: 29px;
            height: 150px;
          }
          ul{
            padding-top: 29px;
            height: 150px;
            width: 70%;
            margin: 0 auto;
            li{
              display: inline-block;
              width: 32%;
              line-height: 30px;
            }
            li:hover{
              text-decoration: underline;
            }
          }
          span{
            display: block;
            text-align: center;
          }
          span:hover{
            text-decoration: underline;
          }
        }
      }
      >div:last-child{
        margin-right: 0;
      }
      .showItem{
        flex: 2.8;
        animation:toShow 0.5s;
        -webkit-animation:toShow 0.5s ; /* Safari 和 Chrome */
      }
      .hiddenItem{
        flex: 1;
        animation:toHidden 0.5s;
        -webkit-animation:toHidden 0.5s ; /* Safari 和 Chrome */
      }
      /*显示动画*/
      @keyframes toShow
      {
        from {flex: 1;}
        to {flex: 2.8;}
      }

      @-moz-keyframes toShow /* Firefox */
      {
        from {flex: 1;}
        to {flex: 2.8;}
      }

      @-webkit-keyframes toShow /* Safari 和 Chrome */
      {
        from {flex: 1;}
        to {flex: 2.8;}
      }

      @-o-keyframes toShow /* Opera */
      {
        from {flex: 1;}
        to {flex: 2.8;}
      }
      /*隐藏*/
      @keyframes toHidden
      {
        from {flex: 2.8; }
        to {flex: 1;}
      }

      @-moz-keyframes toHidden /* Firefox */
      {
        from {flex: 2.8; }
        to {flex: 1;}
      }

      @-webkit-keyframes toHidden /* Safari 和 Chrome */
      {
        from { flex: 2.8;}
        to {flex: 1;}
      }

      @-o-keyframes toHidden /* Opera */
      {
        from { flex: 2.8;}
        to {flex: 1;}
      }
    }
    .info{
      margin: 30px auto 0;
      display: flex;
      justify-content: space-between;
      height: 284px;
      .left{
        width: 48.33%;
        margin-bottom: 30px;
        .title1{
          width: 100%;
          height: 45px;
          display: flex;
          align-items: center;
          border-radius: 3px;
          background:linear-gradient(0deg,rgba(242,242,242,1),rgba(254,254,254,1));
          justify-content: space-between;
          border-bottom: 1px solid #D8D8D8;
          p{
            font-size: 18px;
            color: #333333;
            display: flex;
            line-height: 45px;
            align-items: center;
            >img{
              width: 20px;
              margin: 0 10px;
            }
          }
          .more{
            color: #666666;
            font-size: 12px;
            margin-right: 10px;
            cursor: pointer;
          }
        }
        .tab{
          display: flex;
          >p{
            padding: 8px 17px;
            border: 1px solid #CDCDCD;
            border-top: none;
            border-radius:0 0 5px 5px;
            margin-right: 4px;
            cursor: pointer;
            color: #666666;
            font-size: 14px;
          }
          .checkedP{
            background-color: #2778BE;
            color: #ffffff;
          }
        }
        .body{
          margin-top: 20px;
          width:100%;
          display: flex;
          .lf{
            /*width: 41.3%;*/
            // height: 100%;
            >img{
              object-fit: contain;
              width: 100%;
              height: 100%;
              max-height: 200px;
            }
          }
          .rt{
            width: 100%;
            /*margin-left: 20px;*/
            display: flex;
            display: -webkit-flex;
            flex-direction: column;
            >p{
              flex: 1;
              width: 100%;
              cursor: pointer;
              display: flex;
              justify-content: space-between;
              line-height: 26px;
              display: -webkit-flex;
              >span{
                white-space: nowrap;
                text-overflow: ellipsis;
                overflow: hidden;
              }
              span:nth-child(1){
                // width: 216px;
                width: 80%;
                white-space: nowrap;
                text-overflow: ellipsis;
                overflow: hidden;
                display: block;
              }
              span:nth-child(2){
                white-space: nowrap;
                text-overflow: ellipsis;
                overflow: hidden;
                color: #999999;
                font-size: 12px;
              }
            }
            .seen{
              color: #0064B2;
            }
          }
        }
      }

      .right{
        width: 48.33%;
        margin-bottom: 30px;
        .title1{
          width: 100%;
          height: 45px;
          display: flex;
          border-radius: 3px;
          align-items: center;
          justify-content: space-between;
          border-bottom: 1px solid #D8D8D8;
          background:linear-gradient(0deg,rgba(242,242,242,1),rgba(254,254,254,1));
          p{
            font-size: 18px;
            color: #333333;
            display: flex;
            line-height: 45px;
            align-items: center;
            >img{
              width: 20px;
              margin: 0 10px;
            }
          }
          .more{
            color: #666666;
            font-size: 12px;
            margin-right: 10px;
            cursor: pointer;
          }
        }
        .tab{
          display: flex;
          >p{
            padding: 8px 17px;
            border: 1px solid #CDCDCD;
            border-top: none;
            border-radius:0 0 5px 5px;
            margin-right: 4px;
            cursor: pointer;
            color: #666666;
            font-size: 14px;
          }
          .checkedP{
            background-color: #2778BE;
            color: #ffffff;
          }
        }
        .body{
          margin-top: 20px;
          width:100%;
          display: flex;
          .lf{
            /*width: 41.3%;*/
            // height: 100%;
            >img{
              object-fit: contain;
              width: 100%;
              height: 100%;
              max-height: 200px;
            }
          }
          .rt{
            width: 100%;
            /*margin-left: 20px;*/
            display: flex;
            display: -webkit-flex;
            flex-direction: column;
            >p{
              flex: 1;
              width: 100%;
              cursor: pointer;
              display: flex;
              justify-content: space-between;
              line-height: 26px;
              display: -webkit-flex;
              >span{
                white-space: nowrap;
                text-overflow: ellipsis;
                overflow: hidden;
              }
              span:nth-child(1){
                // width: 216px;
                width: 80%;
                white-space: nowrap;
                text-overflow: ellipsis;
                overflow: hidden;
                display: block;
                &:hover{
                  color: @themeColor;
                }
              }
              span:nth-child(2){
                white-space: nowrap;
                text-overflow: ellipsis;
                overflow: hidden;
                color: #999999;
                font-size: 12px;
              }
            }
            .seen{
              color: #0064B2;
            }
          }
        }
        // .body{
        //   margin-top: 20px;
        //   .top{
        //     width: 100%;
        //     display: flex;
        //     height: 76.8px;   //和左侧前三行相同
        //     margin-bottom: 5px;
        //     >img{
        //       width: 25.8%;
        //       height: 81px;
        //       object-fit: cover;
        //     }
        //     >div{
        //       flex: 1;
        //       cursor: pointer;
        //       .p-title{
        //         margin-left: 16px;
        //         color: #2778BE;
        //         line-height: 34px;
        //         font-weight: bold;
        //         // display: flex;
        //         // justify-content: space-between;
        //         overflow: hidden;
        //         white-space: nowrap;
        //         text-overflow: ellipsis;
        //         width: 70%;
        //         // width: 100%;
        //       }
        //       .p-msg{
        //         font-size: 12px;
        //         width: 70%;
        //         margin-left: 16px;
        //       }
        //     }

        //   }
        //   .down{
        //     width: 100%;
        //     >p{
        //       width: 100%;
        //       cursor: pointer;
        //       display: flex;

        //       justify-content: space-between;
        //       line-height: 30px;
        //       span:nth-child(1){
        //         // width: 462px;
        //         white-space: nowrap;
        //         text-overflow: ellipsis;
        //         overflow: hidden;
        //         display: block;
        //         color: #636363;
        //         width: 80%;
        //       }
        //       span:nth-child(2){
        //         white-space: nowrap;
        //         text-overflow: ellipsis;
        //         overflow: hidden;
        //         color: #999999;
        //         font-size: 12px;
        //       }
        //     }
        //     .seen{
        //       span:nth-child(1){
        //         color: #0064B2;
        //       }
        //     }
        //   }
        //   >p{
        //     white-space: nowrap;
        //     text-overflow: ellipsis;
        //     overflow: hidden;
        //     cursor: pointer;
        //   }
        //   .seen{
        //     color: #0064B2;
        //   }
        // }
      }
    }
    .title2{
      display: flex;
      display: -webkit-flex;
      padding-top: 20px;
      padding-bottom: 15px;
      align-items: center;
      >p{
        display: flex;
        align-items: center;
        .lineC{
          display: block;
          height: 16px;
          width: 4px;
          background-color: #2778BE;
          margin-right: 8px;
        }
        flex: 1;
        font-size:18px
      }
      >span{
        font-size: 12px;
        padding-right: 5px;
        display: inline-block;
        cursor: pointer;
      }
      i{
        cursor: pointer;
        color: #cccccc;
        font-size: 12px;
      }
    }
    .title3{
      display: flex;
      display: -webkit-flex;
      flex-direction: column;
      padding-top: 20px;
      padding-bottom: 15px;
      >p{
        display: flex;
        // align-items: center;
        .lineC{
          display: block;
          height: 16px;
          width: 4px;
          background-color: #2778BE;
          margin-right: 8px;
        }
        flex: 1;
        font-size:18px
      }
    }
    .whiteBg{
      background: #ffffff;
      overflow: hidden;
    }
    /*热门设备*/
    .hotEquip{
      padding-bottom: 10px;
      .ulBox{
        ul{
          overflow: hidden;
          li{
            float: left;
            width: 23.5%;
            margin-right: 2%;
            height: 300px;
            background: #ffffff;
            text-align: center;
            font-size: 16px;
            color: #333333;
            margin-bottom: 30px;
            cursor: pointer;
            &:nth-of-type(4n){
              margin-right: 0;
            }
            .red{
              padding-top: 5px;
            }
            .img{
              width: 100%;
              height: 78%;
              display: flex;
              display: -webkit-flex;
              margin-bottom: 10px;
              img{
                width: auto;
                height: auto;
                max-height: 100%;
                max-width: 100%;
                margin: auto;
              }
            }
          }
        }
      }

    }
    .content{
      padding-top: 90px;
      p:nth-child(1){
        font-size: 30px;
        width: 100%;
        text-align: center;
        line-height: 70px;
        color: #333333;
      }
      p:nth-child(2){
        font-size: 15px;
        width: 100%;
        text-align: center;
        line-height: 30px;
        color: #666666;
      }
      p:nth-child(3){
        font-size: 15px;
        width: 100%;
        text-align: center;
        line-height: 30px;
        color: #666666;
        margin-bottom: 40px;
      }
      /*p:nth-child(4){*/
      /*  margin-top: 40px;*/
      /*}*/
    }
    .footer{
      /*margin-top: 30px;*/
      padding-top: 30px;
      p{
        width: 100%;
        text-align: center;
        font-size: 20px;
        color: #333333;
        line-height: 30px;
      }
      .center{
        width: 100%;
        display: flex;
        flex-direction: row;
        flex-wrap: wrap;
        .unitBox{
          margin: 0 auto;
          display: flex;
          flex-wrap: wrap;
          >p{
            margin-top: 30px;
            width: 18%;
            height: 110px;
            margin-right: 2.5%;
            display: flex;
            justify-content: center;
            align-items: center;
            background-color: #FFFFFF;
            border:1px solid rgba(217, 217, 217, 1);
            padding: 17px 0;
            flex-direction: column;

            >img{
              padding-top: 10px;
              // width: 100%;
              // height: 100%;
              // line-height: 100px;
            }
            >span{
              bottom: 2px;
              font-weight: 700;
              font-size: 13px;
              /*margin-top: 10px;*/
            }
          }
          p:nth-child(5n){
            margin-right: 0;
          }
        }
      }
    }
    .bgFoot{
      background-image: url("../../assets/img/shipDisplay/custBg.png");
    }
    .footerImg{
      padding-top: 40px;
      background-color: #FFFFFF;
      &:nth-of-type(5n){
        margin-right: 0;
      }
    }
  }
  .point{
    cursor: pointer;
  }
</style>
