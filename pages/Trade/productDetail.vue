<template>
  <!--产品详情-->
  <div id="home">
    <div v-if="showShare" id="pop">
      <div class="pup">
        <div>
          <vue-qr :text="url" :size="150" :margin="8"></vue-qr>
        </div>
        <i class="el-icon-close" @click="showShare = false"></i>
      </div>
    </div>
    <div v-if="loginShow" id="pops">
      <div class="loginPup">
        <i class="el-icon-close" @click="loginShow = false"></i>
        <div id="login">
          <!--上部标题-->
          <div class="title1">
            <p>账号登录</p>
            <p @click="toPage('../login/register')">免费注册<i class="el-icon-arrow-right"></i></p>
          </div>
          <!--下部表单-->
          <el-form :model="ruleForm" :rules="rules" ref="ruleForm" class="demo-ruleForm">
            <el-form-item  prop="username" @keyup.enter="submitForm('ruleForm')">
              <el-input class="input1" placeholder="请输入手机号码或用户名" v-model="ruleForm.username" type="text" prefix-icon="el-icon-user"></el-input>
            </el-form-item>
            <el-form-item prop="password" @keyup.enter="submitForm('ruleForm')">
              <el-input class="input1" placeholder="请输入密码" maxlength="24" v-model="ruleForm.password" type="password" prefix-icon="el-icon-lock"></el-input>
              <p class="blue" @click="toPage('../login/changePWD')">忘记密码</p>
            </el-form-item>
            <el-form-item>
              <el-button class="button1" type="primary" @click="submitForm('ruleForm')" @keyup.enter="submitForm('ruleForm')">登录</el-button>
            </el-form-item>
          </el-form>
        </div>
      </div>
    </div>
    <!--店铺简介-->
    <div class="header" :style="{width:bWidth+'px'}">
      <div class="backgroundByHeader">
        <store-title :id="info.shopId" v-if="info.shopId"></store-title>
      </div>
    </div>
    <!--标题面包屑-->
    <div class="title main" :style="{width:width+'px'}">
      <el-breadcrumb separator-class="el-icon-arrow-right">
        <el-breadcrumb-item :to="{ path: './onlinetrading' }">装备在线</el-breadcrumb-item>
        <el-breadcrumb-item :to="{ path: './productTrading' }" v-if="proType==='product'">产品交易</el-breadcrumb-item>
        <el-breadcrumb-item :to="{ path: './shipTrade' }" v-if="proType==='ship'">船舶交易</el-breadcrumb-item>
        <el-breadcrumb-item>
          <p class="textRed" v-if="proType==='product'">产品详情</p>
          <p class="textRed" v-if="proType==='ship'">船舶详情</p>
        </el-breadcrumb-item>
      </el-breadcrumb>
    </div>
    <!--商品信息-->
    <div class="product" :style="{width:width+'px'}">
      <!--商品信息左部-->
      <div class="productLeft">
        <!--上部轮播图-->
        <div class="swiper">
          <swiper-example-thumbs-gallery :imgList="imgList"></swiper-example-thumbs-gallery>
        </div>
        <!--收藏 分享-->
        <div class="share">
          <p  @click="collectGoods"><img :src="info.isCollect === 1?collected:noCollect"/>收藏</p>
          <p @click="openShare"><img src="@/assets/img/微信.png" />微信分享</p>
          <p @click="shareSian"><img src="@/assets/img/微博.png" />微博分享</p>
        </div>
        <!--底部 按钮 二维码-->
        <div class="QRcode">
          <div class="leftButton"  v-if="info.shopNm">
            <button class="button1" @click="toPage('./storeHome?shopId='+info.shopId+'&tabId=0')"><p><img :src="info.shopImg"/></p>{{info.shopNm}}</button>
            <button class="button2" @click="toShop(info.shopId)"><img :src="shop"/>进店逛逛</button>
            <button class="button3" @click="follow"><img :src="start"/>{{info.isFollow ? '已关注' : '关注店铺'}}</button>
          </div>
          <div class="rightButton">
            <!--在vue-qr外面再加一个div，用以防止生命周期加载两次-->
            <div>
              <vue-qr :text="url" :size="150" :margin="8"></vue-qr>
            </div>
            <p>手机下单</p>
          </div>
        </div>
      </div>
      <!--商品信息中部-->
      <div class="productCenter">
        <!--商品 名称价格-->
        <div style="height: 120px">
          <p class="sku-name">{{info.nm}}</p>
          <p class="dt">{{proType==='ship'?'参考价 : ':'产品价格 :'}}</p>
          <p class="price" v-show="currentInfo.origPrice!=price">￥{{currentInfo.origPrice}}</p>
          <p class="price" v-show="currentInfo.origPrice==price">价格面议</p>

          <el-row style="margin-top: 15px" v-if="info.gift === 1">
            <el-col :span="2">
              <p class="dt2">赠品 :</p>
            </el-col>
            <el-col :span="21">
                <el-tooltip :content="i.goodsNm + i.model" placement="bottom" effect="light" v-for=" i in info.gifts" :key="i.goodsId">
                  <el-image  style="width: 25px; height: 25px"  :src="i.goodsImg"    fit="fit" @click="toProDetail(i.giftGoodsId)">   </el-image>
				  <!-- <img :src="i.goodsImg"
				       class="el-images"
				       :ref="'img'+index"
					   style="width: 25px; height: 25px"
				       :class="{toScale:item.ifShow}"
					    @click="toProDetail(i.giftGoodsId)"
				  /> -->
				</el-tooltip>
              <span style="font-size: 12px;color: #999">（条件：购买1件及以上，赠完即止）</span>
            </el-col>
          </el-row>

        </div>
        <!--设备信息-->
        <div class="equipInfo" v-if="proType==='ship'">
          <p >船舶类型： {{info.typesNm}}</p>
          <p>船型编码： {{currentInfo.shipCd}}</p>
          <p>设 计 院：{{info.design}}</p>
          <p>承建船厂：{{info.supplier}}</p>
          <p v-if="currentInfo.leadTime">交货期：{{currentInfo.leadTime}}天</p>
        </div>
        <div class="equipInfo" v-if="proType==='product'">
          <p>设备编号： {{info.deviceCd}}</p>
          <p>设备型号： {{info.deviceModel}}</p>
          <p>设备供应商：{{info.supplier}}</p>
          <p>证书标识：<img :src="item" v-for="(item,index) in ceList" :key="index"/></p>
          <p v-if="currentInfo.leadTime">交货期：{{currentInfo.leadTime}}天</p>
        </div>
        <!--规格属性-->
        <div class="specs">
          <div v-for="(item,index) in specs" :index="index">
            <p>{{item.nm}}</p>：
            <ul>
              <li v-for="(v,i) in item.vals" :key="i" :class="{active:currentSpecArr[index].split(':')[1]==v.id}" @click="toChangeSpecs(index,i)">{{v.nm}}</li>
            </ul>
          </div>
        </div>
        <!--数量-->
        <div class="num">
          <div class="proNum">
            <span>数量</span>：<el-input-number v-model="num" style="width: 120px;" :max="currentInfo.stock" :precision="2" :step="1" :controls="false"></el-input-number>
            <p>
              <i class="el-icon-arrow-up" @click="toAdd"></i>
              <i class="el-icon-arrow-down" @click="toLess"></i>
            </p>
            （库存：{{currentInfo.stock}}）<span v-if="currentInfo.skuUnitNm" style="color: #595757;">单位：{{currentInfo.skuUnitNm}}</span>
          </div>
          <div class="btn">
            <p v-if="proType === 'product' && currentInfo.origPrice!=price" @click="toAddCart">加入购物车</p>
            <p @click="toPay">立即下单</p>
          </div>
        </div>
      </div>
      <!--商品信息右部-->
      <div class="productRight">
        <p><span></span>看了又看<span></span></p>
        <right-look :list="rightList" :type="proType"></right-look>
      </div>
    </div>
    <!-- 商品对应配件列表 -->
    <div class="pratList" :style="{width:width+'px'}" v-if="pratsList.length>0">
      <p class="pratTitle">商品对应配件</p>
      <div class="gallery-thumbsPrat" ref="pratthumbImg">
        <!--:style="{height:(swiperOptionThumbsRIght.height+10)*3+32+'px'}"-->
        <swiper :options="swiperOptionThumbsPrat">
          <!--:style="{height: (swiperOptionThumbsRIght.height+10)*3+'px'}"-->
          <swiper-slide v-for="(item,index) in pratsList" :key="index" class="thumbImg" >
            <div class="img" @click="toProDetail(item.id)">
              <img :src="item.fmImgUrl"/>
            </div>
            <p  @click="toProDetail(item.id)">{{item.nm}}</p>
            <p class="red"  @click="toProDetail(item.id)">￥{{item.mktPrice}}</p>
            <!-- <p class="red"  @click="toProDetail(item.id)" v-if="item.goodsMinPrice!=price && item.goodsMinPrice!==item.goodsMaxPrice">￥{{item.goodsMinPrice}}-{{item.goodsMaxPrice}}</p> -->
            <!-- <p class="red"  @click="toProDetail(item.id)" v-if="item.goodsMinPrice!=price && item.goodsMinPrice===item.goodsMaxPrice">￥{{item.goodsMinPrice}}</p> -->
            <!-- <p class="red"  @click="toProDetail(item.id)" v-if="item.goodsMinPrice==price">价格面议</p> -->
          </swiper-slide>
          <div class="swiper-button-next" slot="button-next" style="right: 0;"> <i class="el-icon-arrow-right"></i> </div>
          <div class="swiper-button-prev" slot="button-prev" style="left: 0;"> <i class="el-icon-arrow-left"></i> </div>
        </swiper>

      </div>
    </div>
    <!--商品详细信息栏-->
    <div class="detail main" :style="{width:width+'px'}">
      <pro-detail :info="info" ref="shipDetail" @setType="setType" v-if="info.id"></pro-detail>
    </div>

  </div>
</template>

<script>
  import {mapState} from "vuex";
  import StoreTitle from "../../components/onlineTrading/storeTitle2";
  import rightLook from "@/components/onlineTrading/rightLook";
  import proDetail from "@/components/onlineTrading/proDetail";
  import SwiperExampleThumbsGallery from "../../components/onlineTrading/swiper-example-thumbs-gallery";
  import collected from '@/assets/img/proDetail/collected.png'
  import noCollect from '@/assets/img/proDetail/noCollect.png'
  import shop from '@/assets/img/proDetail/shop.png'
  import start from '@/assets/img/proDetail/start.png'
  import VRImg from '../../assets/img/personal/VR.png'
  // import Vue from 'vue'
  // import vueQr from 'vue-qr'
  // import test from '@/components/test'
  export default {
    name: "productDetail",
    components: {
      SwiperExampleThumbsGallery,
      StoreTitle,
      rightLook,
      proDetail,
      // Vue,
      // vueQr
      // test
    },
    data() {
      return {
		  VRImg,
        id: '',
        showShare:false,
        loginShow:false,
        ruleForm:{
          username:'',
          password:'',
        },
        rules: {
          username: [
            { required: true, message: '请输入用户名或手机号', trigger: 'blur' },
          ],
          password: [
            { required: true, message: '请输入密码', trigger: 'blur' },
          ],
        },
        qrImg:'',
        url2: '',
        imgList: [],
        proType: '',
        rightList: [],
        ceList: [],//证书
        shareUrl:'',
        url: '移动端地址',
        inviCd:'',
        collected,
        noCollect,
        shop,
        start,
        ifColllect: false,
        num: 1,
        info: {
          shopId: '',
        },
        currentInfo: {},//选中的规格的信息
        specs: [],
        currentSpecArr: [],
        currentSpecs: '',
        pratWidth:100,
        pratsList:[],//商品对应配件列表
        swiperOptionThumbsPrat: {
          slidesPerView :'auto',
          autoplay:5000,
          direction:'horizontal',
          grabCursor:true,
          autoplayDisableOnInteraction:false,
          mousewheelControl:true,
          autoHeight:true,
          speed:500,
          // height:400,
          spaceBetween: 10,
          loop: true,
          navigation: {
            nextEl: '.swiper-button-next',
            prevEl: '.swiper-button-prev'
          }
        },
      }
    },
    computed: {
      ...mapState([
        'bWidth',
        'width',
        'ifLogin',
        'userInfo',
        'price',
        'proSize'
      ]),

    },
    watch: {
      $route() {
        this.id = this.$route.query.id
        this.url = window.location.origin+'/mob/views/trading/shipDetail.html?id='+this.id
        this.proType = this.$route.query.type
        this.shareUrl = window.location.origin+'/sinovat2/login/register'
        this.getData()
      },
      currentSpecArr: {
        handler: function () {
          this.currentSpecs = this.currentSpecArr.join(',')
          this.currentInfo = this.info.skus.find(item => item.skuAttr == this.currentSpecs)
          this.getSkuParts()
          if (this.currentInfo && this.proType == 'ship' && this.currentInfo.shipId) {
            this.$refs.shipDetail.getInfo(this.currentInfo.shipId)
          }
        },
        deep: true
      },

      userInfo(){
        if(this.userInfo.inviCd){
          this.shareUrl = window.location.origin+'/sinovat2/login/register?inviCd='+this.userInfo.inviCd
        }else {
          this.shareUrl = window.location.origin+'/sinovat2/login/register'
        }
      }
    },
    async asyncData(context){
      if(process.client){
        return
      }
      let id = context.query.id
      let proType = context.query.type
      let info = await context.app.api.productDetail(id)
      // if (proType === 'ship') {  //在线交易 船舶详情
      //   info = await context.app.api.tradeShipDetail(id)
      // } else if (proType === 'manage') { //船舶管理 船舶详情
      //   info = await context.app.api.shipDetail(id)
      // }else { //产品详情
      //   info = await context.app.api.productDetail(id)
      // }
      proType = info.ship ? 'ship' : 'product'

      let arr = []
      if(proType === 'product'){
        arr = await Promise.all([context.app.api.proDetailChoice(id),
          context.app.api.proDetailCeList(id)])
      }else if(proType==='ship'){
        arr = await Promise.all([context.app.api.shipDetailCeList(id),
          context.app.api.shipDetailSurvey(id)])
      }
      let arr2 = await Promise.all([context.app.api.proDetailLook(id),
        context.app.api.shopIntro(info.shopId)])
      // arr.push(await context.app.api.proDetailLook(id))
      // arr.push(await context.app.api.shopIntro(info.shopId))
      // return
      return {
        info:JSON.stringify(info),
        arr:arr,
        arr2:arr2
      }
    },
    mounted() {
      console.log('详情=================== =')
      this.id = this.$route.query.id
      this.proType = this.$route.query.type
      this.url = window.location.origin+'/mob/views/trading/shipDetail.html?id='+this.id
      this.shareUrl = window.location.origin+'/sinovat2/login/register'


      this.getData()
    },
    methods: {
      setType(info){
         this.info.typesNm = info.typesNm
      },
      //提交
      submitForm(formName) {
        this.$refs[formName].validate((valid) => {
          if (valid) {
            this.store.commit('changeLoading',true)
            this.api.login(this.ruleForm).then(res=>{
              this.store.commit('token',res.token)
              this.store.commit('userInfo',res.userInfo)
              this.until.seSave('token',res.token)
              this.until.seSave('userInfo',JSON.stringify(res.userInfo))
              this.$message({
                message: '登录成功',
                type: 'success',
                duration:'1500'
              });
              this.loginShow=false
            })
          } else {
            return false;
          }
        });
      },
      //进店逛逛
      toShop(id){
        //sinovat2/Trade/storeHome?shopId=5022714488525824&tabId=0
        window.open('./Trade/storeHome?shopId='+id)
      },
      //跳转产品详情
      toProDetail(id) {
        let url = '../sinovat2/Trade/productDetail?id='+ id + "&type=product"
        window.open(url)
      },
      //收藏商品
      collectGoods(){
        if(!this.ifLogin){
          this.loginShow=true
          // this.$confirm('您未登录，立即登录?', '提示', {
          //   confirmButtonText: '确定',
          //   cancelButtonText: '取消',
          //   type: 'warning'
          // }).then(() => {
          //   this.$router.push('../login/login')
          // })
          return
        }
        if (this.info.isCollect === 1) {
          this.info.isCollect = 0;
          this.api.delFollow(this.id)
        } else {

          //收藏商品
          let param = {
            targetId: this.id,
            type: this.proType=='ship' ? 2 : 3,
          }
          this.api.followShop(param)
          this.info.isCollect = 1;
        }
      },




      //关注店铺
      follow() {
        //取消关注
        if (this.info.isFollow === 1) {
          this.info.isFollow = 0;
          this.api.delFollow(this.info.shopId)
        } else {
          //关注店铺
          let param = {
            targetId: this.info.shopId,
            type: 4,
          }
          this.api.followShop(param)
          this.info.isFollow = 1;
        }
      },
      //获取产品详情详情
      async getData() {
        if (this.proType === 'ship') {  //在线交易 船舶详情
          this.info = await this.api.tradeShipDetail(this.id)
        } else if (this.proType === 'manage') { //船舶管理 船舶详情
          this.info = await this.api.shipDetail(this.id)
        }else { //产品详情
          this.info = await this.api.productDetail(this.id)
        }
        this.imgList = this.info.imgUrl ? this.info.imgUrl.split(',') : []
        this.imgList.forEach((item, index) => {
          if (!item) {
            this.imgList.splice(index, 1)
          }
        })
        this.ceList = this.info.ce ? this.info.ce.split(',') : []
        this.proType = this.info.ship ? 'ship' : 'product'
        this.specs = this.info.attrs
        this.currentSpecArr = []
        this.specs.forEach(item => {
          this.currentSpecArr.push(item.id + ':' + item.vals[0].id)
        })
        //看了又看
        this.rightList = await this.api.proDetailLook(this.id)
      },
      //获取对应配件
      async getSkuParts(){
        // this.currentInfo.skuId='5329947234341888'
        let data = await this.api.getPartsList(this.currentInfo.skuId)
        this.pratsList=data

        // this.pratWidth = this.$refs.pratthumbImg.offsetWidth
        // this.swiperOptionThumbsPrat.height = this.pratWidth/this.proSize+40
      },

      //更改属性
      toChangeSpecs(index, i) {
        this.currentSpecArr[index] = this.specs[index].id + ':' + this.specs[index].vals[i].id
        this.$set(this.currentSpecArr, index, this.currentSpecArr[index])
      },
      //加入购物车
      toAddCart() {
        if (!this.ifLogin) {
          this.loginShow=true
          // this.$confirm('您未登录，立即登录?', '提示', {
          //   confirmButtonText: '确定',
          //   cancelButtonText: '取消',
          //   type: 'warning'
          // }).then(() => {
          //   this.$router.push('../login/login')
          // })
          return
        }
        if (this.num > this.currentInfo.stock) {
          this.$message({
            message: '当前规格库存不足',
            type: 'warning'
          });
          return
        }
        let param = {
          goodsId: this.id,
          goodsSkuId: this.currentInfo.skuId,
          qty: this.num
        }
        this.api.addCart(param).then(() => {
          this.$message({
            message: '加入购物车成功！',
            type: 'success'
          });
        })
      },
      //立即下单
      toPay() {
        if (!this.ifLogin) {
          this.loginShow=true
          // this.$confirm('您未登录，立即登录?', '提示', {
          //   confirmButtonText: '确定',
          //   cancelButtonText: '取消',
          //   type: 'warning'
          // }).then(() => {
          //   this.$router.push('../login/login')
          // })
          return
        }
        if (this.num > this.currentInfo.stock) {
          this.$message({
            message: '当前规格库存不足',
            type: 'warning'
          });
          return
        }
        let proArr = []
        proArr[0] = {
          nm: this.info.shopNm,
          ship: this.info.ship,
          skuId: this.currentInfo.skuId,
          items: [{
            img: this.info.fmImgUrl,
            goodsNm: this.info.nm,
            attrDesc: this.currentInfo.attrDesc,
            origPrice: this.currentInfo.origPrice,
            qty: this.num,
            gift:this.info.gift,
            giftItems:this.info.gifts,
            leadTime:this.currentInfo.leadTime,
            skuUnitNm:this.currentInfo.skuUnitNm
          }]
        }
        this.until.seSave('cartList', JSON.stringify(proArr))
        this.until.href('/order/payment?payType=1')
      },
      //增加数量
      toAdd() {
        if (this.num < this.currentInfo.stock) {
          this.num++
        }
      },
      openShare(){
        this.showShare = true
      },
      shareSian(title, url, pic){
        var param = {
          url: url || window.location.href,
          type: "3",
          count: "1" /** 是否显示分享数，1显示(可选)*/,
          appkey: "" /** 您申请的应用appkey,显示分享来源(可选)*/,
          title: "分享的标题" /** 分享的文字内容(可选，默认为所在页面的title)*/,
          pic: pic || "" /**分享图片的路径(可选)*/,
          ralateUid: "" /**关联用户的UID，分享微博会@该用户(可选)*/,
          rnd: new Date().valueOf()
        };
        var temp = [];
        for (var p in param) {
          temp.push(p + "=" + encodeURIComponent(param[p] || ""));
        }
        var targetUrl =
          "http://service.weibo.com/share/share.php?" + temp.join("&");
        window.open(targetUrl, "sinaweibo");
      },

      //减少数量
      toLess() {
        if (this.num > 1) {
          this.num--
        }
      },
      toPage(url) {
        this.$router.push(url)
      },
	  toVR(id){
	    window.open('./mframe?typeCd=1&id='+id)
	  }
    }
  }
</script>
<style lang="less">
  #login{
    .el-input__inner{
      border: none;
      border-bottom: 1px solid rgba(0,0,0,0.1);
      border-radius: 0;
      background-color: #FFFFFF;
    }
  }
</style>
<style lang="less" scoped>
  @import url("../../assets/css/init.less");

  .special_item_price{
    width: 100%;
    text-decoration: line-through;
    font-family: sans-serif,serif;
    color: #999;
    font-size: 15px;
    line-height: 14px;
    display: inline;
    margin-left: 10px;
  }
  .red_bg {
    color: #df3033;
    background: 0 0;
    border: 1px solid #df3033;
    padding: 2px 3px;
    margin-right: 5px;
    display: inline-block;
    line-height: 16px;
  }
  .sku-name {
    font: 700 16px Arial,"microsoft yahei";
    color: #666;
    padding-top: 10px;
    line-height: 28px;
    margin-bottom: 5px;
  }
  .dt {
    display: inline;
    color: #999;
  }
  .dt2 {
    margin-top: 8px;
    color: #999;
  }
  .price {
    color: #e4393c;
    font-size: 18px;
    display: inline;
  }
  #home {
    background-color: #F3F3F3;
    background-image: url("../../assets/img/bg.png");
    background-position: center bottom;
    background-repeat: repeat-x;
    padding-bottom: 120px;
  }

  .header {
    background-color: #FFFFFF;
    height: 186px;

    .backgroundByHeader {
      margin: 0 auto;
      height: 186px;

      .textRed {
        color: #FF3E3E;
      }
    }
  }

  .title {
    background-color: #F3F3F3;
    padding: 20px;
    box-sizing: border-box;

    i {
      color: #333333;
    }

    .textRed {
      color: #FF3E3E;
    }
  }

  .product {
    background-color: #FFFFFF;
    border: 1px solid #ECECEC;
    box-sizing: border-box;
    margin: 0 auto;
    padding: 20px;
    display: flex;
    display: -webkit-flex;
    margin-bottom: 40px;

    .productLeft {
      flex: 3;
      width: 500px; /*不加上的话样式错乱，好像是swiper引起的*/

      .swiper {
        width: 100%;

      }

      .share {
        height: 59px;
        display: inline-flex;
        border-bottom: 1px solid #E8E8E8;
        width: 100%;
        margin-bottom: 20px;
        >p{
          margin-right: 31px;
          line-height: 59px;
          font-size: 14px;
          color: #333333;
          cursor: pointer;
          img{
            width: 12px;
            margin-right: 5px;
          }
          i{
            color: @themeColor;
            margin-right: 5px;
          }
        }
      }

      .QRcode {
        display: flex;
        display: -webkit-flex;
        justify-content: space-between;

        .leftButton {
          flex: 1;
          width: 100%;

          .button1 {
            width: 100%;
            height: 59px;
            border: 1px solid #E0E0E0;
            font-size: 16px;
            color: #656565;
            box-sizing: border-box;
            background-color: #F7F7F7;
            display: flex;
            display: -webkit-flex;
            align-items: center;
            justify-content: center;
            margin-bottom: 13px;
            cursor: pointer;

            p {
              width: 35px;
              height: 35px;
              border-radius: 50%;
              border: 1px solid #E3E3E3;
              display: flex;
              display: -webkit-flex;
              margin-right: 10px;
              overflow: hidden;

              img {
                margin: auto;
                width: auto;
                height: auto;
                max-width: 100%;
                max-height: 100%;
              }
            }

          }

          .button2, .button3 {
            cursor: pointer;
            width: 49%;
            font-size: 12px;
            color: #656565;
            height: 30px;
            line-height: 30px;
            border: 1px solid #DFDFDF;
            box-sizing: border-box;
            background-color: #F7F7F7;
            display: flex;
            display: -webkit-flex;
            align-items: center;
            justify-content: center;
            float: left;

            img {
              margin-right: 5px;
              width: 15px;
            }
          }

          .button3 {
            margin-left: 2%;
          }
        }

        .rightButton {
          width: 85px;
          margin-left: 30px;
          text-align: center;

          > img {
            width: 85px;
            border: 1px solid #F3F3F3;
          }

          > p {
            font-size: 12px;
            color: #656565;
          }
        }
      }
    }

    .productCenter {
      flex: 3.5;
      padding-left: 17px;
      margin-right: 6%;

      > div {
        border-bottom: 1px solid #E8E8E8;

        &:last-of-type {
          border-bottom: 0;
        }
      }



      .equipInfo {
        color: #666666;
        line-height: 36px;
        padding: 15px 0;

        p {
          display: flex;
          display: -webkit-flex;
          align-items: center;

          img {
            width: 40px;
            margin-right: 5px;
          }
        }
      }

      .specs {
        padding-top: 30px;

        > div {
          display: flex;
          display: -webkit-flex;
          padding-bottom: 10px;
          line-height: 31px;

          p {
            color: #999999;
            width: 70px;
            text-align: justify;
            text-align-last: justify;
          }

          ul {
            flex: 1;
            padding-left: 5px;

            li {
              display: inline-block;
              height: 31px;
              border: 1px solid #CCCCCC;
              margin-right: 20px;
              margin-bottom: 20px;
              padding: 0 10px;
              cursor: pointer;
            }

            .active, li:hover {
              border: 1px solid @redColor;
            }
          }
        }

      }

      .num {
        .proNum {
          height: 42px;
          display: flex;
          display: -webkit-flex;
          align-items: center;
          margin: 30px 0;

          span {
            color: #999999;
            text-align: justify;
            text-align-last: justify;
            width: 70px;
          }
          /deep/.el-input__inner{
            border-radius: 0;
          }
          input {
            margin-left: 5px;
            height: 42px;
            border: 1px solid #DFDFDF;
            box-sizing: border-box;
            width: 81px;
            margin-right: 3px;
            text-indent: 8px;
          }

          p {
            height: 100%;
            display: flex;
            display: -webkit-flex;
            flex-direction: column;
            justify-content: space-between;

            i {
              display: block;
              border: 1px solid #DFDFDF;
              width: 36px;
              height: 20px;
              text-align: center;
              line-height: 20px;
              cursor: pointer;
            }
          }
        }

        .btn {
          width: 100%;
          padding-left: 90px;

          p {
            /*width: 146px;*/
            height: 42px;
            line-height: 42px;
            text-align: center;
            border-radius: 4px;
            box-sizing: border-box;
            display: inline-block;
            width: 40%;
            cursor: pointer;

            &:first-of-type {
              border: 1px solid @themeColor;
              color: @themeColor;
              margin-right: 5%;
            }

            &:last-of-type {
              background: @themeColor;
              color: #ffffff;
            }
          }
        }
      }
    }

    .productRight {
      flex: 1;
      width: 10%;
      > p {
        color: #333333;
        display: flex;
        display: -webkit-flex;
        align-items: center;
        justify-content: center;
        padding-bottom: 10px;

        span {
          height: 1px;
          flex: 1;
          margin: 0 5px;
          background: rgba(0, 0, 0, 0.1);
          display: inline-block;
        }
      }
    }

  }
  .pratList{
    margin: 0 auto 40px;
    background-color: #FFFFFF;
    // padding-top: 20px;
    // height: 230px;
    .pratTitle{
      font-size: 24px;
      text-align: center;
      padding: 20px 0 30px;
    }
    .gallery-thumbsPrat{
      height: 180px;
      .swiper-slide{
        display: block!important;
        width: 140px;
        box-sizing: border-box;
        font-size: 12px;
        .img{
          border: 1px solid #DDDDDD;
          margin-right: 5px;
          width: 140px;
          height: 140px;
          box-sizing: border-box;
          img{
            width: 100%;
            height: 100%;
            display: block;
          }
        }
      }
      .thumbImg{
        cursor: pointer;
        text-align: center;
        p{
          display: block;
        }
      }
      .swiper-button-prev, .swiper-button-next{
        z-index: 8;
        position: absolute;
        top: 0;
        width: 18px;
        height: 100%;
        background: #e0e0e0;
        cursor: pointer;
        margin: 0;
        display: flex;
        display: -webkit-flex;
        align-items: center;
        justify-content: center;
      }
    }
  }
  .detail {
    background-color: #FFFFFF;
    border: 1px solid #ECECEC;
    box-sizing: border-box;
    padding: 20px;

  }
  #pop,#pops{
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    background: rgba(0,0,0,0.1);
    z-index: 55;
    display: flex;
    display: -webkit-flex;
    .pup{
      margin: auto;
      background: #ffffff;
      border-radius: 5px;
      width: 170px;
      height: 170px;
      padding: 15px;
      position: relative;
      i{
        position: absolute;
        cursor: pointer;
        margin-left: 10px;
        top:3px;
        right: 3px;
      }
    }
    .loginPup{
      margin: auto;
      background: #ffffff;
      border-radius: 5px;
      width: 307px;
      height: 352px;
      padding: 15px;
      position: relative;
      >i{
        position: absolute;
        cursor: pointer;
        top: 15px;
        right: 15px;
        z-index: 2;
      }
      #login{
        position: relative;
        .title1{
          display: inline-flex;
          height: 80px;
          align-items: center;
          justify-content: space-between;
          width: 100%;
          padding: 0 30px;
          p:nth-child(1){
            font-size: 18px;
            color: #333333;
            font-weight: normal;
          }
          p:nth-child(2){
            color: #E4393C;
            font-size: 14px;
            cursor: pointer;
          }
        }
        .demo-ruleForm{
          width: 246px;
          margin: 0 auto;
        }
        span{
          cursor: pointer;
        }
        .blue{
          color: @themeColor;
          font-size: 14px;
          position: absolute;
          bottom: 1px;
          right: 7px;
          cursor: pointer;
        }
        .button1{
          background-color: @themeColor;
          width: 100%;
          border-radius: 3px;
          margin-top: 14%;
        }
      }
    }

  }

</style>
