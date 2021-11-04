<template>
  <div id="home" :style="{width:bWidth+'px'}">
    <el-carousel :height="bannerHeight+'px'" class="adert">
      <el-carousel-item v-for="item in adertList" :key="item.aiid">
        <div class="bannerImg" :style="{backgroundImage:'url('+item.imgUrl+')'}"  @click="toPage(item.locUrl)"/>
      </el-carousel-item>
    </el-carousel>
    <div class="info" :style="{width:width + 'px'}">
      <div class="left">

        <div class="title1">
          <p><img src="@/assets/img/new.png"/>{{newTitle1}}</p>
          <p class="more" @click="toPage('../tradeInfo/tradeInfo?cid='+ '5002994673669120')">更多 >></p>
        </div>
        <div class="body">
          <div class="lf point" @click="toDetail(ifItem)">
            <img :src="lfImg" v-if="lfImg"/>
          </div>
          <div class="rt">
            <p v-for="(item,index) in infoList" :key="index" @click="toDetail(item)" :class="{seen:item.isSeen}" @mouseenter="enter(index)" @mouseleave="leave(index)" ><span>· {{item.nm}}</span><span>{{item.releTm}}</span></p>
          </div>
        </div>
      </div>
      <div class="right">
        <div class="title1">
          <p><img src="@/assets/img/new.png"/>{{newTitle2}}</p>
          <p class="more" @click="toPage('../tradeInfo/tradeInfo?cid=1')">更多 >></p>
        </div>
        <div class="body">
          <!-- <div class="top">
            <img :src="topImg" v-if="topImg" alt="topImg" class="point" @click="toDetail(topItem)"/>
            <div @click="toPage({path:'../tradeInfo/infoDetail',query:{id:topId,cid:topCid}})">
              <p class="p-title point" @click="toDetail(topItem)">{{topP}}</p>
              <P class="p-msg point" @click="toDetail(topItem)">{{topCont}}</P>
            </div>
          </div> -->
          <div class="down">
            <p v-for="(item,index) in zbggList" :key="index" @click="toDetail2(item.bidId)" :class="{seen:item.isSeen}" @mouseenter="enter3(index)" @mouseleave="leave3(index)">
              <span>· {{item.title}}</span><span>{{item.releTm}}</span><span>{{item.bidStatus}}</span></p>
          </div>
        </div>
      </div>
    </div>

    <div class="whiteBg shuzi" :style="{width:bWidth+'px'}">
      <!-- <div class="title main" :style="{width:width+'px'}">
        <h3 style="font-size: 34px">新材料船舶综合服务数字化平台</h3>

        平台提供船舶设计、船舶建造、品质保障、在线交易、金融保险、协同创新、智慧船舶、平台服务、行业信息、在线学习等服务
      </div>
      <scale-list :list="shuziList"></scale-list> -->
      <div class="title2 main"  :style="{width:width+'px'}">
        <p><span class="lineC"></span>船舶展示</p>
        <span @click="toPage('/shipDisplay/shipCat')">更多 >></span>
        <!-- <i class="iconfont icon-gengduo"   @click="toPage('/shipDisplay/shipCat')"></i> -->
      </div>
      <div class="sfq main"  :style="{width:width+'px',height:(width-30)/4*164/256+'px'}">
         <!-- <div v-for="(item,index) in shipClassify" :key="index"
              :style="{backgroundImage:`url('${item.imgUrl}')`}"
              :class="{hiddenItem:!item.ifShow,showItem:item.ifShow}"
              @click="toPage(`/shipDisplay/shipShow?cId=${item.id}&cNm=${item.nm}&cImg=${item.imgUrl2}`)"

              > -->
         <div v-for="(item,index) in shipClassify" :key="index"
              :style="{height:(width-30)/4*164/256+'px'}"
              @click="toPage(`/shipDisplay/shipShow?cId=${item.id}&cNm=${item.nm}&cImg=${item.icon1}`)"

              >
            <!-- <div class="showContent"  v-show="item.ifShow">
                <p>{{item.nm}}</p>
                <ul>
                  <li @click="toPage(`/shipDisplay/shipShow?cId=${v.id}&cNm=${v.nm}&cImg=${v.imgUrl2}`)" v-for="(v,i) in item.list" :key="i">{{v.nm}}</li>
                </ul>
                <span @click="toPage(`/shipDisplay/shipShow?cId=${item.id}&cNm=${item.nm}&cImg=${item.imgUrl2}`)">了解详情 ></span>
            </div> -->
            <el-image
              class="scallBig"
              style="width: 100%;height: 100%"
              :class="{toScale:item.ifShow}"
              :src="item.imgUrl"
              fit="cover"></el-image>
           <!-- <div class="hiddenContent"> -->
             <p class="textB"><span>——</span> <span class="textS">{{item.nm}}</span> <span>——</span></p>
           <!-- </div> -->
         </div>
      </div>

      <!--船厂总览-->
      <div class="title2 main"  :style="{width:width+'px'}">
        <p><span class="lineC"></span>船厂总览</p>
        <span @click="toPage('/shipDisplay/shipCat')">更多 >></span>
        <!-- <i class="iconfont icon-gengduo"   @click="toPage('/shipDisplay/shipCat')"></i> -->
      </div>
      <div class="sfq main"  :style="{width:width+'px',height:(width-30)/4*164/256+'px'}">
        <!-- <div v-for="(item,index) in shipClassify" :key="index"
             :style="{backgroundImage:`url('${item.imgUrl}')`}"
             :class="{hiddenItem:!item.ifShow,showItem:item.ifShow}"
             @click="toPage(`/shipDisplay/shipShow?cId=${item.id}&cNm=${item.nm}&cImg=${item.imgUrl2}`)"

             > -->
        <div v-for="(item,index) in overList" :key="index"
             :style="{height:(width-30)/4*164/256+'px'}"
             @click="toPage(item.path)"
        >
          <!-- <div class="showContent"  v-show="item.ifShow">
              <p>{{item.nm}}</p>
              <ul>
                <li @click="toPage(`/shipDisplay/shipShow?cId=${v.id}&cNm=${v.nm}&cImg=${v.imgUrl2}`)" v-for="(v,i) in item.list" :key="i">{{v.nm}}</li>
              </ul>
              <span @click="toPage(`/shipDisplay/shipShow?cId=${item.id}&cNm=${item.nm}&cImg=${item.imgUrl2}`)">了解详情 ></span>
          </div> -->
          <el-image
            class="scallBig"
            style="width: 100%;height: 100%"
            :class="{toScale:item.ifShow}"
            :src="item.imgUrl"
            ></el-image>
          <!-- <div class="hiddenContent"> -->
          <p class="textB"><span>——</span> <span class="textS">{{item.nm}}</span> <span>——</span></p>
          <!-- </div> -->
        </div>
      </div>
    </div>




    <!--热门设备-->
    <div class="whiteBg hotEquip" :style="{width:bWidth+'px'}">
      <div class="title2 main"  :style="{width:width+'px'}" style="margin-top: 30px;">
        <p><span class="lineC"></span>热门产品</p>
        <span  @click="toPage('/Trade/productTrading')">更多 >></span>
        <!-- <i class="iconfont icon-gengduo"  @click="toPage()"></i> -->
      </div>
      <div class="ulBox main midSwiper" :style="{width:width+'px'}">
        <!-- <ul class="main" v-viewer>
          <li v-for="(item,index) in  " :key=""
              :class="{bgShadow:item.ifShow}"
              :style="{height:width*0.235*proSize+70+'px'}"
              @mouseenter="changeLiTrans(index,1,'equip')"
              @mouseleave="changeLiTrans(index,0,'equip')"
              @click.stop="toPage('../Trade/productDetail?type=product&id='+item.goodsId)">
            <div class="img" >
              <el-image
                :src="item.img"
                fit="contain"></el-image>
            </div>
            {{item.nm}}
            <p class="red">￥{{item.origPrice}}</p>
          </li>
        </ul> -->

        <swiper class="swiper" :options="swiperOption">
          <!-- <div style="width: 90%;overflow: hidden;"> -->
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

           <!-- <div class="swiper-button-prev swiper-button-black" slot="button-prev"></div>
            <div class="swiper-button-next swiper-button-black" slot="button-next"></div> -->
            <div class="swiper-button-prev swiper-button-white swiperBtn" style="left: 0;" slot="button-prev">
              <img :src="btR" alt="" style="transform: rotate(180deg);"/>
            </div>
            <div class="swiper-button-next swiper-button-white swiperBtn" style="right: 0;" slot="button-next">
              <img :src="btR" alt=""/>
            </div>
          <!-- </div> -->
          <!-- <div class="swiper-pagination grayColor" slot="pagination"></div> -->
        </swiper>


      </div>
    </div>


    <div class="homeAdert" >
      <div class="adertBox main" :style="{width:width+'px'}">
        <div class="adertNav point"
             v-for="(item,index) in sevenList1"
             @mouseenter="changeLiTrans(index,1,1)"
                @mouseleave="changeLiTrans(index,0,1)"
                @click="toPage(item.locUrl)">
             <el-image
               style="width: 100%;height: 100%"
               :class="{toScale:item.ifShow}"
               :src="item.imgUrl"
               fit="cover"></el-image>
          <p><span>——</span> <span class="textS">{{item.nm}}</span> <span>——</span></p>
        </div>
        <div class="cloumnBox">
          <div class="adertNav point"
               v-for="(item,index) in sevenList2"
               @mouseenter="changeLiTrans(index,1,2)"
                  @mouseleave="changeLiTrans(index,0,2)"
                  @click="toPage(item.locUrl)"
                  style="margin-right: 0;">
               <el-image
                 style="width: 100%;height: 100%"
                 :class="{toScale:item.ifShow}"
                 :src="item.imgUrl"
                 fit="cover"></el-image>
            <p><span>——</span> <span class="textS">{{item.nm}}</span> <span>——</span></p>
          </div>
        </div>
      </div>
      <div class="adertBox main" :style="{width:width+'px'}">
        <div class="adertNav point"
            v-for="(item,index) in sevenList3"
            @mouseenter="changeLiTrans(index,1,3)"
               @mouseleave="changeLiTrans(index,0,3)"
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
  </div>
</template>

<script>
import sz1 from '@/assets/img/homeSZ1.png'
import szh1 from '@/assets/img/homeSZH1.png'
import sz2 from '@/assets/img/homeSZ2.png'
import szh2 from '@/assets/img/homeSZH2.png'
import sz3 from '@/assets/img/homeSZ3.png'
import szh3 from '@/assets/img/homeSZH3.png'
import sz4 from '@/assets/img/homeSZ4.png'
import szh4 from '@/assets/img/homeSZH4.png'
import sz5 from '@/assets/img/homeSZ5.png'
import szh5 from '@/assets/img/homeSZH5.png'
import sz6 from '@/assets/img/homeSZ6.png'
import szh6 from '@/assets/img/homeSZH6.png'
import sz7 from '@/assets/img/homeSZ7.png'
import szh7 from '@/assets/img/homeSZH7.png'
import sz8 from '@/assets/img/homeSZ8.png'
import szh8 from '@/assets/img/homeSZH8.png'
import bk1 from '@/assets/img/供应链金融.png'
import bk2 from '@/assets/img/金融服务.png'
import bk3 from '@/assets/img/人员保险.png'
import bk4 from '@/assets/img/船舶保险.png'
import ql1 from '@/assets/img/检验检测.png'
import ql2 from '@/assets/img/监理监造.png'
import ql3 from '@/assets/img/专家评审.png'
import ql4 from '@/assets/img/标准规则.png'
import ql5 from '@/assets/img/船厂总览.png'
import ql6 from '@/assets/img/船舶设计.png'
import bx from '@/assets/img/homeBX.png'
import bz from '@/assets/img/homeBZ.png'
import btR from '@/assets/img/personal/rightArrow.png';

import {mapState} from "vuex";
import scaleList from '@/components/scaleList'
export default {
  // head(){
  //   return{
  //     title:this.title,
  //     meta:[{
  //       'name':'keywords',
  //       'content': this.webKeyWord
  //     },{
  //       'name':'description',
  //       'content': this.webContent
  //     }]
  //   }
  // },
  data(){
    return{
      bx,
      bz,
      btR,
      shipClassify:[],
      overList:[
        {
          imgUrl:ql5,
          nm:'船厂总览',
          path:'/shipDisplay/allShipyard'
        },
        {
          imgUrl:ql6,
          nm:'船舶设计',
          path:'/shipDisplay/shipDesign'
        },
      ],//船舶总览列表
      bannerHeight:450,
      bannerHeight2:450,//卡片轮播
      roleAllList:[],//设计院列表
      swiperOption: {
        slidesPerView: 5,
        spaceBetween: 1,
        freeMode: true,
        navigation: {
          nextEl: '.swiper-button-next',
          prevEl: '.swiper-button-prev'
        }
      },

      adertList:[],//轮播图列表
      equipList:[],//热门设备
      shipyardList:[],//船厂列表
      sevenList:[],//7张广告图
      sevenList1:[],
      sevenList2:[],
      sevenList3:[],
      adertHeight:"",
      lfImg:'',
      ifItem:'',
      topImg:'',
      topId:'',
      topCid:'',
      topP:'',
      topTm:'',
      topCont:'',
      topItem:{},
      infoList:[],//导航信息的列表
      zbList:[],//招标
      zbggList: [],//招标公告
      tabCd:0,
      newTitle1:'',
      newTitle2:'',
    }
  },
  computed: {
    ...mapState([
      'bWidth',
      'width',
      "proSize"
    ])
  },
  components: {
    scaleList,
  },
  watch:{

    width(){
      if(this.shipyardList.length>0){
        let myImg = new Image()
        myImg.src = this.shipyardList[0].imgUrl
        myImg.onload = () =>{
          this.bannerHeight2 = this.width*0.5*myImg.height/myImg.width
        }
      }
    },
    // lfImg(){
    //   this.infoList.forEach((item,index){
    //
    //   })
    // }
  },
  created(){
  },
  async asyncData(context){
    if(process.client){
      return
    }
    let qry = context.app.query.new()
    context.app.query.toW(qry,'cid','5002994673669120','EQ')

    //筛选需求
    let qry2 =  context.app.query.new()
    context.app.query.toW(qry2,'cid','5032642222281728','EQ')

    //船舶分类
    let qry3 = context.app.query.new()
    context.app.query.toW(qry3,'pid','4942035725390848','EQ')
    let classify = await context.app.api.getClassify(context.app.query.toEncode(qry3))
    classify = classify.slice(0,4)
    for(let i=0;i<classify.length;i++){
      let qry33 = context.app.query.new()
      context.app.query.toW(qry33,'pid',classify[i].id,'EQ')
      context.app.query.toO(qry33,'seq','asc')
      let data = await context.app.api.getClassify(context.app.query.toEncode(qry33))
      classify[i].list = data

    }

    let arr = await Promise.all([
      context.app.api.tradeInfo(context.app.query.toEncode(qry)),
      context.app.api.tradeInfo(context.app.query.toEncode(qry2)),
      context.app.api.getAdert('homeSeven'),
      context.app.api.shopRecomdPro()
    ])
    return {
      arr:arr,
      classify:classify
    }
  },
  mounted(){
    this.getData()
    // this.getInfoTab()
    // this.getZbData()
    this.getInfoData()
    this.getZbggList()
  },
  methods:{
    async getData(){
      await this.getAdert()
      this.getNewCat()
      await this.getSevenA()
      await this.getShip()
      await this.getEquipList()
      // await this.getRoleList()
    },
    async getNewCat(){
      let qry = this.query.new()
      let list = await this.api.infoList(this.query.toEncode(qry))
      list.forEach(item=>{
        if(item.id=='5002994673669120'){
          this.newTitle1 = item.nm
        }
        if(item.id=='5032642222281728'){
          this.newTitle2 = item.nm
        }
      })
    },
    //获取广告轮播
    async getAdert(){
      this.adertList = await this.api.getAdert('homeAdert')
      if(this.adertList.length>0){
        let myImg = new Image()
        myImg.src = this.adertList[0].imgUrl
        myImg.onload = ()=>{
          this.bannerHeight = myImg.height
        }
      }
    },
    //获取广告轮播
    async getSevenA(){
      this.sevenList = await this.api.getAdert('homeSeven')
      console.log('7张广告图')
      this.sevenList.forEach((item,index)=>{
        if(index<2){
          this.sevenList1.push(item)
        }else if(index<4){
          this.sevenList2.push(item)
        }else{
          this.sevenList3.push(item)
        }
      })
      console.log(this.sevenList)
      // if(this.sevenList.length>0){
      //   this.
      // }
    },
    //获取船厂列表
    // async getShipyard(){
    //   let qry = this.query.new()
    //   this.query.toW(qry,'identityNm','船厂')
    //   this.shipyardList = await this.api.getRoleAll(this.query.toEncode(qry))
    //   this.shipyardList.forEach(item=>{
    //     item.imgUrl = item.imgUrl.split(',')[0]
    //   })
    //   if(this.shipyardList.length>0){
    //     let myImg = new Image()
    //     myImg.src = this.shipyardList[0].imgUrl
    //     myImg.onload = () =>{
    //       this.bannerHeight2 = this.width*0.5*myImg.height/myImg.width
    //     }
    //   }
    // },
    // toShipyard(item){
    //   this.$router.push({
    //     path:'../shipDisplay/shipyard',
    //     query:{
    //       id:item.id,
    //     }
    //   })
    // },

    //入驻设计院
    // async getRoleList(){
    //   let qry = this.query.new()
    //   this.query.toW(qry,'identityCd','identity20','EQ')
    //   this.query.toP(qry,1,4)
    //   this.roleAllList = await this.api.getRoleAll(this.query.toEncode(qry))
    //   //暂时只取第一张
    //   this.roleAllList.splice(1)
    //   this.roleAllList[0].imgUrl= this.roleAllList[0].imgUrl.split(',')[0]
    //   console.log('111',this.roleAllList)
    // },
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

    //鼠标滑动效果  索引，类型（1滑入，0滑出）
    changeLiTrans(index,type,nm){
      if(nm==1){
        this.sevenList1[index].ifShow = type ? true : false
        this.$set(this.sevenList1, index, this.sevenList1[index])
      }
      if(nm==2){
          this.sevenList2[index].ifShow = type ? true : false
          this.$set(this.sevenList2,index,this.sevenList2[index])
      }
      if(nm==3){
          this.sevenList3[index].ifShow = type ? true : false
          this.$set(this.sevenList3,index,this.sevenList3[index])
      }
      if(nm==4){
        // console.group()
        // console.log(type)
        // console.log(index)
        // console.groupEnd()
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
    //页面跳转
    toPage(url){
        this.$router.push(url)
    },
    // async getInfoTab(){
    //   // this.infoTabList = []
    //   let qry = this.query.new()
    //   let list = await this.api.infoList(this.query.toEncode(qry))
    //   list.forEach(item =>{
    //     if(item.nm == '行业新闻'||item.nm == '需求发布'){
    //       this.infoTabList.push(item)
    //     }
    //   })
    // },
    // 更新选中
    // change(item,index){
    //   this.tabCd = index
    //   console.log('item.cid',item.id)
    //   this.getInfoData(item.id)
    // },
   // 筛选行业新闻
    async getInfoData(){
      let qry = this.query.new()
      this.query.toW(qry,'cid','5002994673669120','EQ')
      // this.query.toO(qry,'releTm','desc')
      let data = await this.api.tradeInfo(this.query.toEncode(qry))
      this.infoList = data.data.list
      console.log('行业新闻')
      this.infoList.splice(6)
      this.lfImg = this.infoList[0].imgUrl.toString()
      this.ifItem = this.infoList[0]
      console.log(this.infoList)
      // this.zbList.splice(6)
    },
    async getZbData(){
      let qry = this.query.new()
      //筛选需求
      this.query.toW(qry,'cid','5032642222281728','EQ')
      // this.query.toO(qry,'releTm','desc')
      let data = await this.api.tradeInfo(this.query.toEncode(qry))
      this.zbList = data.data.list
      this.topImg = this.zbList[0].imgUrl
      this.topP = this.zbList[0].nm
      this.topTm = this.zbList[0].releTm
      this.topId = this.zbList[0].id
      this.topCid = this.zbList[0].cid
      this.topCont = this.zbList[0].cont.replace(/<\/?[^>]*>/g, "").slice(0,35) + '...';
	    this.topCont = this.topCont.replace(/(&nbsp;)/g, "");
      this.topItem=this.zbList[0]
      this.zbList.splice(0,1)
      this.zbList.splice(3)
      console.log('发布')
      console.log(this.zbList)
    },
    getZbggList() {
      let qry = this.query.new()
      this.query.toP(qry,1,6)
      // this.query.toW(qry,'afficheTypeNm','采购公告','EQ')
      this.api.getZbggList(this.query.toEncode(qry)).then(res => {
        console.log('招标公告',res)
        res.data.list.forEach(item => {
          item.releTm = item.releTm.substring(0,10)
          item.isSeen = false
        })
        this.zbggList = res.data.list

      })
    },
    toDetail(item){
      this.$router.push({
        path:'../tradeInfo/infoDetail',
        query:{
          id:item.id,
          cid:item.cid
        }
      })
    },
    toDetail2(id) {
      this.$router.push({
        path: '../tradeInfo/zbDetail',
        query: {
          id: id,
          cid:1
        }
      })
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
    enter3(index){
      this.zbggList[index].isSeen = true
      this.$set(this.zbList,index,this.zbList[index])
    },
    leave3(index){
      this.zbggList[index].isSeen = false
      this.$set(this.zbList,index,this.zbList[index])
    },
  }
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
      overflow: visible;
    }
    .ensure{
      .el-image{
        width: 100%;
        height: 100%;
      }
    }
    // .el-carousel__indicators--horizontal{
    //   left: 75%;
    // }
    .point{
      cursor: pointer;
    }
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
        width: 25px;
        top: 42%;
      }
    }
  }
</style>
<style lang="less" scoped>
  #home{
    background-color: #F3F3F3;
    background-image: url("../assets/img/footer.png");
    background-position: center bottom;
    background-repeat: repeat-x;
    padding-bottom: 80px;
    .adert{
      width: 100%;
      text-align: center;
      /*max-height: 350px;*/
      .bannerImg{
        height: 100%;
        background-position: center;
        background-repeat: no-repeat;
      }
    }

  }
  .info{
    margin: 30px auto 0;
    display: flex;
    justify-content: space-between;
    .left{
      width: 58.33%;
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
        }
      }
      .body{
        margin-top: 20px;
        width:100%;
        display: flex;
        .lf{
          width: 41.3%;
          // height: 100%;
          >img{
            object-fit: cover;
            width: 100%;
            height: 100%;
            max-height: 200px;
          }
        }
        .rt{
          width: 58%;
          margin-left: 20px;
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
      width: 38.33%;
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
        }
      }
      .body{
        margin-top: 20px;
        .top{
          width: 100%;
          display: flex;
          height: 76.8px;   //和左侧前三行相同
          margin-bottom: 5px;
          >img{
            width: 25.8%;
            height: 81px;
            object-fit: cover;
          }
          >div{
            width: 70%;
            flex: 1;
            cursor: pointer;
            .p-title{
              margin-left: 16px;
              color: #2778BE;
              line-height: 34px;
              font-weight: bold;
              // display: flex;
              // justify-content: space-between;
              overflow: hidden;
              white-space: nowrap;
              text-overflow: ellipsis;
              // width: 70%;
              // width: 100%;
            }
            .p-msg{
              font-size: 12px;
              // width: 70%;
              margin-left: 16px;
            }
          }

        }
        .down{
          width: 100%;
          >p{
            width: 100%;
            cursor: pointer;
            display: flex;

            justify-content: space-between;
            line-height: 30px;
            span:nth-child(1){
              // width: 462px;
              white-space: nowrap;
              text-overflow: ellipsis;
              overflow: hidden;
              display: block;
              color: #636363;
              width: 60%;
            }
            span:nth-child(2){
              white-space: nowrap;
              text-overflow: ellipsis;
              overflow: hidden;
              color: #999999;
              font-size: 12px;
              width: 20%;
              text-align: center;
            }
            span:nth-child(3){
              white-space: nowrap;
              text-overflow: ellipsis;
              overflow: hidden;
              color: #2778BE;
              width: 20%;
              text-align: center;
            }
          }
          .seen{
            span:nth-child(1){
              color: #0064B2;
            }
          }
        }
        >p{
          white-space: nowrap;
          text-overflow: ellipsis;
          overflow: hidden;
          cursor: pointer;
        }
        .seen{
          color: #0064B2;
        }
      }
    }
  }
  .homeAdert{
    margin: 30px auto;

    .adertBox{
      display: flex;
      justify-content: space-between;
      margin-bottom: 10px;
      .adertNav{
        position: relative;
        overflow: hidden;
        /*display: inline-block;*/
        margin-right: 10px;
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
            margin: 0 10px;
            opacity: 1;
            font-size:19px;
            font-weight: 600;
          }
          >span{
            opacity: 0.2;
          }
        }
      }
      .adertNav:last-child{
        margin-right: 0;
      }
      .cloumnBox{
        display: flex;
        flex-direction: column;
        justify-content: space-between;
      }
    }
  }
  @import url("../assets/css/init.less");
  .cardImg{
    position: relative;
    p{
      position: absolute;
      text-align: center;
      left: 0;
      bottom: 0;
      background: rgba(0,0,0,0.3);
      width: 100%;
      height: 40px;
      color: #ffffff;
      line-height: 40px;
    }
  }
  h3{
    color: #333333;
    font-size: 24px;
    line-height: 60px;
    padding-top: 30px;
    i{
      color: #A1A1A1;
    }
  }
  .title{
    text-align: center;
    font-size: 14px;
    color: #999999;
    padding-top: 10px;
    padding-bottom: 40px;

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
  .whiteBg{
    background: #ffffff;
    overflow: hidden;
  }
  .grayBg{
     background: #F5F5F5;
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
    /*height: 240px;*/
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
  .shipServe{
    padding-bottom: 60px;
  }
  /*最后一个，保险，保障*/
  .ensure{
    padding-top: 60px;
    padding-bottom: 10px;
    >div{
      height: 520px;
      margin-bottom: 60px;
      display: flex;
      display: -webkit-flex;
      .intro{
         width: 40%;
        text-align: center;
        color: #ffffff;
        img{
          width: 100px;
          height: 100px;
          margin-top: 136px;
        }
        h5{
          font-size: 20px;
          margin: 20px auto;
        }
        p{
          text-align: center;
          width: 75%;
          margin: 0 auto;
        }
      }
      ul{
        flex: 1;
        li{
          height: 250px;
          display: inline-block;
          position: relative;
          cursor: pointer;
          overflow: hidden;
          >p{
            position: absolute;
            width: 100%;
            left: 0;
            bottom: 0;
            background: rgba(0,0,0,0.5);
            height: 40px;
            line-height: 40px;
            z-index: 8;
            color: #ffffff;
            text-align: center;
          }
          &:nth-of-type(1),&:nth-of-type(2){
             margin-bottom: 20px;
          }
          &:nth-of-type(1),&:nth-of-type(3){
            margin-left: 20px ;
            margin-right: 20px ;
          }
        }

      }
    }
  }
  .more{
    cursor: pointer;
  }
  .scallBig{
    transition: all 0.9s;
      &:hover{
          transform: scale(1.2);
      }
  }
</style>
