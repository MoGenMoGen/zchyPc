<template>
  <!--在线交易-->
  <div id="home">
    <design-appo :show="show" @close="close" @submit="submit"></design-appo>
    <!--导航栏、轮播图、平台成交量-->
    <div class="grayBg" :style="{width:bWidth+'px'}">
      <div class="backgroundByImg" :style="{width:width+'px'}">
        <!--导航栏-->
        <div class="classify">
          <classify></classify>
        </div>
        <!--轮播图-->
        <div class="Carousel">
          <el-carousel trigger="click" :height="bannerHeight+'px'">
            <el-carousel-item v-for="item in adertList" :key="item.id">
              <div class="bannerImg" :style="{backgroundImage:'url('+item.img+')'}">
              </div>
            </el-carousel-item>
          </el-carousel>
        </div>
        <!--成交量-->
        <div class="Volume">
          <!--成交量显示-->
          <div class="view" >

            <!--<p>-->
              <!--<span class="line1"></span>-->
              <!--平台交易金额-->
              <!--<span class="line2"></span>-->
            <!--</p>-->

            <div class="tradeTotal">
              <p >交易总额GMV</p>
              <p>
                {{fmoney(volume.total,2)}}<span>万元</span>
              </p>

            </div>

<!--            客户新的界面-->
            <ul class="newTotal" >
              <li v-for="item in totalList">
                <div>
                  <img :src="item.img">
                  <p>
                    <span>{{item.nm}}</span>
                    <span>{{item.nmEn}}</span>
                  </p>
                </div>
                <div>
                  <span>{{fmoney(item.amount)}}</span><span>万元</span>
                </div>
              </li>
            </ul>
<!--            饼图，注释掉-->
<!--            <div id="pieEcharts" ></div>-->
            <!--&lt;!&ndash;船舶订单&ndash;&gt;-->
            <!--<div class="order">-->
              <!--<p class="textRed">-->
                <!--{{volume.shipOrder}}万元-->
                <!--&lt;!&ndash;6584.8&ndash;&gt;-->
              <!--</p>-->
              <!--<p>船舶交易总额</p>-->
            <!--</div>-->
            <!--&lt;!&ndash;产品订单&ndash;&gt;-->
            <!--<div class="totalOrder">-->
              <!--<p class="textRed">{{volume.goodsOrder}}万元</p>-->
              <!--<p>产品交易总额</p>-->
            <!--</div>-->
          </div>
          <!--设计院-->
          <!--<div class="design">-->
            <!--<el-scrollbar class="tab">-->
              <!--<p v-for="item in designTabList" :key="item.id" @click="currentDesign=item.cd" :class="{active:item.cd==currentDesign}">-->
                <!--{{item.nm}}-->
              <!--</p>-->
            <!--</el-scrollbar>-->
            <!--<div class="designList">-->
              <!--<swiper class="swiper" :options="swiperOptionA">-->
                <!--<swiper-slide v-for="item in designCompanyList" :key="item.id"><img :src="item.logo" :style="{padding:5 + 'px'}" @click="toDesignDetail(item)"/></swiper-slide>-->
                <!--<div class="swiper-button-prev swiper-button-black" slot="button-prev"></div>-->
                <!--<div class="swiper-button-next swiper-button-black" slot="button-next"></div>-->
              <!--</swiper>-->
            <!--</div>-->
          <!--</div>-->
          <!--设计服务-->
          <div class="reservation">
            <div v-for="(item,index) in designList" :key="item.id2" class="servesA" @click="toPage(item)">
              <img :src="item.img" alt=""/>
              <p>{{item.nm}}</p>
            </div>
          </div>
        </div>
      </div>
    </div>
    <!--重点推荐-->
    <div class="grayBg hotEquip" :style="{width:bWidth+'px'}">
      <div class="background" :style="{width:width+'px'}">
        <!--重点推荐-->
        <div class="begin" v-if="recomdPro.length>0">
          <div class="singleA" @click="toProDetail(recomdPro[0])">
            <div class="img">
              <img :src="recomdPro[0].img" alt=""/>
            </div>
            <p>{{recomdPro[0].nm}}</p>
            <p class="textRed" v-if="recomdPro[0].goodsMinPrice!=price && recomdPro[0].goodsMinPrice !== recomdPro[0].goodsMaxPrice">￥{{fmoney(recomdPro[0].goodsMinPrice)}}-{{fmoney(recomdPro[0].goodsMaxPrice)}}</p>
            <p class="textRed" v-if="recomdPro[0].goodsMinPrice!=price && recomdPro[0].goodsMinPrice === recomdPro[0].goodsMaxPrice">￥{{fmoney(recomdPro[0].goodsMinPrice)}}</p>
            <p class="textRed" v-if="recomdPro[0].goodsMinPrice==price">价格面议</p>
          </div>
        </div>
        <div class="midSwiper">
          <swiper class="swiper" :options="swiperOption">
            <swiper-slide v-for="(item,index) in recomdPro" :key="item.id" v-if="index>0">
              <div class="productSwiper" @click="toProDetail(item)">
                <div style="width: 159px;height: 159px" >
                  <img :src="item.img" />
                </div>
                <el-tooltip :content="item.nm" placement="top" effect="light">
                  <p class="p">{{item.nm}}</p>
                </el-tooltip>


                <div class="productMiniSwiper">
                  <p v-if="item.goodsMinPrice!=price && item.goodsMinPrice!==item.goodsMaxPrice">￥{{fmoney(item.goodsMinPrice)}}-{{fmoney(item.goodsMaxPrice)}}</p>
                  <p v-if="item.goodsMinPrice!=price && item.goodsMinPrice===item.goodsMaxPrice">￥{{fmoney(item.goodsMinPrice)}}</p>
                  <p v-if="item.goodsMinPrice==price">价格面议</p>
                  <img src="../../assets/img/onlineTrade/价格.png"/>
                  <p class="childA"><span><img :src="car"/></span></p>
                </div>
              </div>
            </swiper-slide>
            <!--<div class="swiper-pagination grayColor" slot="pagination"></div>-->
            <div class="swiper-button-prev swiper-button-white" slot="button-prev">
              <img :src="btL" alt=""/>
            </div>
            <div class="swiper-button-next swiper-button-white" slot="button-next">
              <img :src="btR" alt=""/>
            </div>
          </swiper>

        </div>
        <div class="minSwiper">
          <el-carousel trigger="click" :height="247+'px'" arrow="never">
            <el-carousel-item v-for="(item,index) in onlineTrade" :key="item.id3" >
              <!-- < el-image  style="width: 100%; height: 100%"  :src="item.imgUrl"   fit="fit"></el-image> -->
			  <img :src="item.imgUrl"
			       class="el-images"
			       :ref="'img'+index"
				   style="width: 100%; height: 100%"
			       :class="{toScale:item.ifShow}"
			  />
			</el-carousel-item>
          </el-carousel>
        </div>
      </div>
    </div>

    <!--优质供应商-->
    <div class="grayBg hotEquip" :style="{width:bWidth+'px'}">
      <div class="backgroundByGoods" :style="{width:width+'px'}" style="height: 231px;">
        <!--上方标题栏-->
        <div class="titleH">
          <p @click="toQuality" style="cursor: pointer;"><img :src="titleL"/>优质供应商<img :src="titleR"/></p>
        </div>
        <!--下方商品栏-->
        <div class="storeScroll">
          <el-carousel trigger="click" height="150px">
            <el-carousel-item v-for="(item,index) in storeSwiperList" :key="index">
              <div class="storeSwiper">
                <div @click="toShipDetail(i.id)" v-for="(i,k) in item" :key="k" class="swiperItem">
                  <img :src="i.shopImg"
                    class="el-images"
                    :ref="'img'+k"
                  />
                </div>
              </div>
            </el-carousel-item>
          </el-carousel>
        </div>

      </div>
    </div>

    <!-- <div class="grayBg hotEquip" :style="{width:bWidth+'px'}">
      <div class="backgroundByGoods" :style="{width:width+'px'}" style="height: 231px;">
        <div class="titleH">
          <p @click="toQuality" style="cursor: pointer;"><img :src="titleL"/>优质供应商<img :src="titleR"/></p>
        </div>
        <div class="proScroll" @mouseenter="goodStoreScrollState(1)"
             @mouseleave="goodStoreScrollState(0)">
          <el-scrollbar class="textMain" style="height: 135px;">
            <div :class="{scrollToLeftStore:goodStoreScroll}" class="goodProScroll" style="display: flex;">
              <div class="partMain" v-for="(item,index) in goodStoreListAll" :key="index" @click="toShipDetail(item.id)" style="width: 125px;height: 125px;">
                <div class="img" style="border: 1px solid rgba(0,0,0,0.1);width: 125px;height: 125px;overflow: hidden;">
                  <img :src="item.shopImg" :alt="item.nm"/>
                </div>
              </div>
            </div>
          </el-scrollbar>
        </div>
      </div>
    </div> -->

    <!--每日特价、折扣产品-->
    <div class="grayBg hotEquip" :style="{width:bWidth+'px'}">
      <div class="backgroundBySale" :style="{width:width+'px'}">
        <!--每日特价-->
        <div class="price">
          <!--内容部分-->
          <el-tabs v-model="activePrice">
            <!--左侧标题-->
            <el-tab-pane label="特价专区" :disabled="true"></el-tab-pane>
            <!--下面一样，都为tab切換内容-->
            <el-tab-pane :label="item.nm" :name="item.id.toString()" :key="item.id" v-for="item in dailySpecials">
              <!--下部具体内容-->
              <trade-price :list="item.items"></trade-price>
            </el-tab-pane>
            <el-tab-pane :disabled="true">
              <!--<span slot="label" class="more" style="font-weight: 400;">更多<i class="el-icon-arrow-right"></i></span>-->
            </el-tab-pane>
          </el-tabs>
        </div>
        <!--折扣产品-->
        <div class="discount">
          <div class="tabZ">
            <el-tabs v-model="activeDiscount">
              <el-tab-pane label="优惠产品" :disabled="true"></el-tab-pane>
              <el-tab-pane :label="item.nm" :name="item.id.toString()" v-for="item in discountPro" :key="item.id">
                <trade-price :list="item.items"></trade-price>
                <!--<trade-price-sale :list="item.items"></trade-price-sale>-->
              </el-tab-pane>
              <el-tab-pane :disabled="true">
                <!--<span slot="label" class="more" style="font-weight: 400;">更多<i class="el-icon-arrow-right"></i></span>-->
              </el-tab-pane>
            </el-tabs>
          </div>
        </div>
      </div>
    </div>
    <!--发现好货-->
    <div class="grayBg hotEquip" :style="{width:bWidth+'px'}">
      <div class="backgroundByGoods" :style="{width:width+'px'}">
        <!--上方标题栏-->
        <div class="titleH">
          <p><img :src="titleL"/>发现好货<img :src="titleR"/></p>
        </div>
        <!--下方商品栏-->
        <div class="storeScroll">
          <el-carousel trigger="click" height="270px">
            <el-carousel-item v-for="(item,index) in goodProSwiperList" :key="index">
              <div class="storeSwiper">
                <div @click="toProDetail(i)" v-for="(i,k) in item" :key="k" class="proItem">
                  <div class="img">
                    <img :src="i.img"
                      class="el-images"
                      :ref="'img'+k"
                    />
                  </div>
                  <p>{{i.nm}}</p>
                </div>
              </div>
            </el-carousel-item>
          </el-carousel>
        </div>


        <!--下方商品栏-->
        <!-- <div class="proScroll" @mouseenter="goodProScrollState(1)"
             @mouseleave="goodProScrollState(0)">
          <el-scrollbar class="textMain">
            <div :class="{scrollToLeft:goodProScroll}" class="goodProScroll">
              <div class="partMain" v-for="(item,index) in goodPro" :key="index" @click="toProDetail(item)">
                <div class="img">
                  <img :src="item.img" :alt="item.nm"/>
                </div>
                <p>{{item.nm}}</p>
              </div>
            </div>
          </el-scrollbar>
        </div> -->

      </div>
    </div>
    <!--新品首发、排行榜单、优质店铺、领券中心-->
    <div class="grayBg hotEquip" :style="{width:bWidth+'px'}">
      <div class="backgroundByAllOfThese" :style="{width:width+'px'}">
        <!--新品首发-->
        <div class="part1">
          <div class="upUp">
            <p>新品首发</p>
            <p @click="toStarting">更多<i class="el-icon-arrow-right"></i></p>
          </div>
          <div class="main1" v-if="newPro.length>0" @click="toProDetail(newPro[0])">
            <div class="img">
              <img :src="newPro[0].img" :alt="newPro[0].nm"/>
            </div>
            <p>{{newPro[0].nm}}</p>
            <p class="textRed" v-if="newPro[0].goodsMinPrice!=price && newPro[0].goodsMinPrice!==newPro[0].goodsMaxPrice">￥{{fmoney(newPro[0].goodsMinPrice)}}-{{fmoney(newPro[0].goodsMaxPrice)}}</p>
            <p class="textRed" v-if="newPro[0].goodsMinPrice!=price && newPro[0].goodsMinPrice===newPro[0].goodsMaxPrice">￥{{fmoney(newPro[0].goodsMinPrice)}}</p>
            <p class="textRed" v-if="newPro[0].goodsMinPrice==price">价格面议</p>
          </div>
        </div>
        <!--排行榜单-->
        <div class="part2">
          <div class="upUp">
            <p>排行榜单</p>
            <p @click="toRank">更多<i class="el-icon-arrow-right"></i></p>
          </div>
          <div class="main2">
            <div class="section1" v-for="item in rankList" @click="toProDetail(item)">
              <img :src="item.rankImg" alt="1"/>
              <div class="centerImg">
                <img :src="item.img" alt="2"/>
              </div>
              <p>{{item.nm}}</p>
            </div>
          </div>
        </div>
        <!--优质店铺-->
        <div class="part3">
          <div class="upUp">
            <p>优质供应商</p>
            <p @click="toQuality">更多<i class="el-icon-arrow-right"></i></p>
          </div>
          <div class="main3">
            <div class="piece1" v-for="item in goodStoreList.slice(0 , 2)" @click="toShipDetail(item.id)">
              <div class="shareA">
                <p>{{item.nm}}</p>
                <p>{{item.follow}}人关注</p>
              </div>
              <div class="shareB">
                <img :src="item.shopImg" alt="1"/>
              </div>
            </div>
          </div>
        </div>
        <!--领券中心-->
        <div class="part4">
          <el-carousel>
            <el-carousel-item v-for="(item,index) in adertList2" :key="index">
              <el-image
                @click="toHref(item.locUrl)"
                style="width: 100%; height: 100%"
                :src="item.imgUrl"
                fit="cover"></el-image>
            </el-carousel-item>
          </el-carousel>
          <!--<div class="upUp">-->
            <!--<p>领券中心</p>-->
            <!--<p @click="toCoupon">更多<i class="el-icon-arrow-right"></i></p>-->
          <!--</div>-->
          <!--<div class="main4">-->
            <!--<div class="sectionA" v-for="(item,index) in couponList" :key="index" @click="toCoupon">-->
              <!--<img src="../../assets/img/onlineTrade/左边.png" alt=""/>-->

              <!--&lt;!&ndash;满减券&ndash;&gt;-->
              <!--<div class="core" v-if="item.discType === 0">-->
                <!--<p>￥<span>{{item.discAmt}}</span> 满减券</p>-->
                <!--<p>满{{item.payAmt}}元可用</p>-->
                <!--<p v-if="item.couponType === 1" style="font-size: 12px;color: #ffffff">限购[<span>{{item.shopNm}}</span>]店内产品</p>-->
              <!--</div>-->
              <!--<div class="core" v-if="item.discType === 1">-->
                <!--<p><span>{{item.discRate*10}} 折扣券</span></p>-->
                <!--<p v-if="item.couponType === 1" style="font-size: 12px;color: #ffffff">限购[<span>{{item.shopNm}}</span>]店内产品</p>-->
              <!--</div>-->

              <!--<div class="core" v-if="item.discType === 2">-->
                <!--<p>￥<span>{{item.discAmt}} </span>代金券</p>-->
                <!--<p v-if="item.couponType === 1" style="font-size: 12px;color: #ffffff">限购[<span>{{item.shopNm}}</span>]店内产品</p>-->
              <!--</div>-->

              <!--<img src="../../assets/img/onlineTrade/右边.png" alt="1"/>-->
            <!--</div>-->
          <!--</div>-->
        </div>
      </div>
    </div>
    <!--为您推荐-->
    <div class="backgroundByRecommend hotEquip" :style="{width:width+'px'}">
      <!--为您推荐标题栏-->
      <div class="mainTitle">
        <p><img :src="titleL"/>为您推荐<img :src="titleR"/></p>
      </div>
      <!--Tab标签栏-->
      <div class="mainTabA">
        <div class="tabOption" v-for="(item,index) in tjList" :key="index" @click="changeActive(index,item)" :class="{activeTab : item.isActive}">
          <p>{{item.nm}}</p>
          <!--<p>{{item.pnm}}</p>-->
        </div>
      </div>
      <!--标签下方显示栏-->
      <div class="tabDown">
        <product-list :list="forYouList" v-show="tjList.length>0 && tjList[0].isActive"></product-list>
        <product-list :list="forYouList" ifShip="true" v-show="tjList.length>0 && !tjList[0].isActive"></product-list>
      </div>
      <!--<div class="footer">-->
        <!--<p>点击查看更多<i class="el-icon-arrow-right"></i></p>-->
      <!--</div>-->
    </div>
  </div>

</template>

<script>
  import {mapState} from "vuex";

  import title1 from '@/assets/img/onlineTrade/设计预约.png';
  import title2 from '@/assets/img/onlineTrade/DIV报价.png';
  import title3 from '@/assets/img/onlineTrade/我要出售.png';
  import title4 from '@/assets/img/onlineTrade/船舶管理.png';
  import title5 from '@/assets/img/onlineTrade/金融服务.png';
  import title6 from '@/assets/img/onlineTrade/保险服务.png';
  import icRight1 from '@/assets/img/onlineTrade/商城船舶销售.png';
  import icRight2 from '@/assets/img/onlineTrade/商城产品销售.png';
  import icRight3 from '@/assets/img/onlineTrade/商城船舶销售.png';
  import icBj from '@/assets/img/onlineTrade/商城背景.png';
  import btL from '@/assets/img/onlineTrade/左.png';
  import btR from '@/assets/img/onlineTrade/右.png';
  import titleL from '@/assets/img/onlineTrade/titleLeft.png';
  import titleR from '@/assets/img/onlineTrade/titleRight.png';
  import TradePrice from "@/components/onlineTrading/tradePrice";
  import TradePriceSale from "@/components/onlineTrading/tradePriceSale";

  import ProductList from "@/components/onlineTrading/productList";
  import classify from '@/components/classify'
  import DesignAppo from "@/components/designAppo";
  import first from "@/assets/img/onlineTrade/first.png";
  import second from "@/assets/img/onlineTrade/second.png";
  import third from "@/assets/img/onlineTrade/third.png";
  import car from '../../assets/img/car.png'
  import login from "../login/login";
  import btR1 from '@/assets/img/personal/rightArrow.png';
  import * as echarts from 'echarts';
  export default {
    name: 'swiper-example-virtual-slides',
    title: 'Virtual slides',
    data() {
      return {
        car,
        btL,
        btR,
        btR1,
        titleL,
        titleR,
        first,
        second,
        third,
          icRight1,
          icBj,

        show: false,
        // currentDesign: 1,
        // designTabList: [],
        designCompanyList: [],
        adertList: [],//轮播图
        adertList2:[],
        recomdPro: [],//重点推荐
        onlineTrade: [],//广告位
        activePrice: 'first',
        activeDiscount: 'first',
        volume: {
          shipOrder:'',
          goodsOrder:'',
          other:'',
          total:'',
        },//平台成交总量
        dailySpecials: [],//每日特价
        discountPro: [],//折扣商品
        newPro: [],//新品首发
        goodStoreList: [],//优质店铺列表
        goodStoreListAll: [],//所有优质店铺列表
        storeSwiperList:[],//优质店铺列表轮播用
        goodPro: [], //发现好货
        goodProSwiperList:[],//发现好货列表轮播用
        rankList: [],//排行榜单列表
        forYouList:[],//为你推荐商品列表

        swiperOption: {
          slidesPerView: 4,
          spaceBetween: 1,
          freeMode: true,
          navigation: {
            nextEl: '.swiper-button-next',
            prevEl: '.swiper-button-prev'
          }
        },
        swiperOptionA: {
          slidesPerView: 3,
          spaceBetween: 0,
          loop: true,
          navigation: {
            nextEl: '.swiper-button-next',
            prevEl: '.swiper-button-prev'
          }
        },
        swiperOptionScroll: {
          autoplay: {
            delay: 3000,
            stopOnLastSlide: false,
            disableOnInteraction: true,
          },
          slidesPerView: 'auto',
          spaceBetween: 30,
          scrollbar: {
            draggable: true,

          }
          // loop: true,
        },
        goodStoreScrollTime: null,
        goodStoreScroll: true,//优质供应商是否滚动
        goodProScrollTime: null,
        goodProScroll: true,//发现好货是否滚动
        couponList: [],//优惠券列表
        bannerHeight: 440,

        //设计服务列表
        designList: [
        // {
        //  nm: '设计预约',
        //  img: title1,
        //  href: '',
        // },
        {
          nm: 'DIY报价',
          img: title2,
          href: '../shipDisplay/shipDIY'
        }, {
          nm: '我要出售',
          img: title3,
          href: '/personal/allShipList',
        }, {
          nm: '船舶管理',
          img: title4,
          href: '../shipDisplay/shipMan'
        },
        // {
        //   nm: '金融服务',
        //   img: title5,
        //   href: '/safe/safe?pos=jrfw',
        // }, {
        //   nm: '保险服务',
        //   img: title6,
        //   href: '/safe/safe',
        // }
        ],
        tjList: [],
          //汇总列表
          totalList:[
              {
                  id:1,
                  img:icRight1,
                  nm:"船舶销售",
                  nmEn:"Ship sales",
                  amount:0
              },
              {
                  id:2,
                  img:icRight2,
                  nm:"产品销售",
                  nmEn:"Product sales",
                  amount:0
              },
              {
                  id:3,
                  img:icRight3,
                  nm:"其他业务",
                  nmEn:"Other business",
                  amount:0
              },

          ],



      }
    },
    computed: {
      ...mapState([
        'bWidth',
        'width',
        'price'
      ]),
    },
    components: {
      DesignAppo,
      ProductList,
      TradePrice,
      TradePriceSale,
      classify
    },
    onload() {
    },
    created() {
    },
    mounted() {
      this.getData()
    },
    watch: {
      currentDesign() {
        let qry = this.query.new()
        this.query.toW(qry, 'identityCd', this.currentDesign, 'EQ')
        this.query.toP(qry, 1, 8)
        this.api.getRoleAll(this.query.toEncode(qry)).then(res => {
          this.designCompanyList = res
        })

      }
    },
    methods: {
        fmoney(s, n) {
            n = n > 0 && n <= 20 ? n : 2;
            s = parseFloat((s + "").replace(/[^\d\.-]/g, "")).toFixed(n) + "";
            var l = s.split(".")[0].split("").reverse(), r = s.split(".")[1];
            var t = "";
            for (let i = 0; i < l.length; i++) {
                t += l[i] + ((i + 1) % 3 == 0 && (i + 1) != l.length ? "," : "");
            }
            return t.split("").reverse().join("") + "." + r;
        },
        toRank(){
          this.$router.push({
              path:'/Trade/ranking'
          })
        },
        toQuality(){
            this.$router.push({
                path:'/Trade/qualityStore'
            })
        },
        toStarting(){
            this.$router.push({
                path:'/Trade/starting'
            })
        },
        toCoupon(){
            // this.$router.push({
            //     path:'/Trade/coupon'
            // })
          window.open('./Trade/coupon')

        },
      toHref(url){
        window.location.href = url
      },
      formatNum(num)
      {
        if(!/^(\+|-)?(\d+)(\.\d+)?$/.test(num)){  return   num;}
        var   a   =   RegExp.$1,   b   =   RegExp.$2,   c   =   RegExp.$3;
        var   re   =   new   RegExp().compile("(\\d)(\\d{3})(,|$)");
        while(re.test(b))   b   =   b.replace(re,   "$1,$2$3");
        return   a   +""+   b   +""+   c;
      },

      initTotal(){

        this.totalList[0].amount=  parseFloat(this.volume.shipOrder).toFixed(2)

          this.totalList[1].amount=  parseFloat(this.volume.goodsOrder).toFixed(2)
          this.totalList[2].amount= parseFloat(this.volume.other).toFixed(2)
      },
      echarts(){
        var chartDom = document.getElementById('pieEcharts');
        var myChart = echarts.init(chartDom);
        let option = {
          tooltip: {
            show:false,//关闭鼠标移上去后显示浮框信息
            trigger: 'item'
          },

          series: [
            {
              name: '访问来源',
              type: 'pie',
              center:['20%','50%'],
              radius: ['45%', '52%'],
              avoidLabelOverlap: false,
              hoverAnimation:false,　　//鼠标悬浮是否有区域弹出动画，false:无 true:有
              label: {
                show: true,
                position: 'center',
                color:'#333333',
                formatter: [
                  '{a|船舶销售}',
                  '{b|'+this.formatNum(Math.round(this.volume.shipOrder))+'万元}'
                ].join('\n'),
                rich: {
                  a: {
                    padding:[3,0,0,0],
                    color: '#333333',
                  },

                  b: {
                    color: 'red',
                  },
                }
              },
              labelLine: {
                show: false
              },
              data: [
                {value: this.volume.shipOrder, name: '船舶销售',
                  "itemStyle": {
                    "normal": {
                      "color": "#009cff"
                    }
                  }
                },
                {value: this.volume.total-this.volume.shipOrder, name: '',
                  "itemStyle": {
                    "normal": {
                      "color": "#e5e5e5"
                    },
                    emphasis:{color:'#e5e5e5'}
                  }
                },

              ]
            },{
              name: '访问来源',
              type: 'pie',
              center:['50%','50%'],
              radius: ['45%', '52%'],
              avoidLabelOverlap: false,
              hoverAnimation:false,　　//鼠标悬浮是否有区域弹出动画，false:无 true:有
              label: {
                show: true,
                position: 'center',
                color:'#333333',
                formatter: [
                  '{a|产品销售}',
                  '{b|'+this.formatNum(Math.round(this.volume.goodsOrder))+'万元}'
                ].join('\n'),
                rich: {
                  a: {
                    padding:[3,0,0,0],
                    color: '#333333',
                  },

                  b: {
                    color: 'red',
                  },
                }
              },

              labelLine: {
                show: false
              },
              data: [
                {value: this.volume.goodsOrder, name: '产品销售',
                  "itemStyle": {
                    "normal": {
                      "color": "#ff7600"
                    }
                  }
                },
                {value: this.volume.total-this.volume.goodsOrder, name: '',
                  "itemStyle": {
                    "normal": {
                      "color": "#e5e5e5"
                    },
                    emphasis:{color:'#e5e5e5'}
                  }
                },

              ]
            },
            {
              name: '访问来源',
              type: 'pie',
              center:['80%','50%'],
              radius: ['45%', '52%'],
              avoidLabelOverlap: false,
              hoverAnimation:false,　　//鼠标悬浮是否有区域弹出动画，false:无 true:有
              label: {
                show: true,
                position: 'center',
                color:'#333333',
                formatter: [
                  '{a|其他业务}',
                  '{b|'+this.formatNum(Math.round(this.volume.other))+'万元}'
                ].join('\n'),
                rich: {
                  a: {
                    padding:[3,0,0,0],
                    color: '#333333',
                  },

                  b: {
                    color: 'red',
                  },
                }
              },
              labelLine: {
                show: false
              },
              data: [
                {value: this.volume.other, name: '其他业务',
                  "itemStyle": {
                    "normal": {
                      "color": "#02b059"
                    }
                  }
                },
                {value: this.volume.total-this.volume.other, name: '',
                  "itemStyle": {
                    "normal": {
                      "color": "#e5e5e5"
                    },
                    emphasis:{color:'#e5e5e5'}
                  }
                },

              ]
            }
          ]
        };
        myChart.setOption(option)
      },
      async getData() {
        this.adertList = await this.api.shopHomeAdert() //广告
        // let designTabList = await this.api.getRoleList()//入驻机构
        // this.designTabList = []
        // designTabList.forEach(item => {
        //   if (item.cd === 'identity20' || item.cd === 'identity30' || item.cd === 'identity40' || item.cd === 'identity50') {
        //     this.designTabList.push(item)
        //   }
        // })
        // this.currentDesign = this.designTabList[0].cd
        this.recomdPro = await this.api.shopRecomdPro() //重点推荐
        this.onlineTrade = await this.api.getAdert('onlinetrad') //广告位
        this.onlineTrade.forEach((item,index)=>{
          this.objectFitImages(this.$refs['img'+index])
        })
          this.volume.shipOrder = await this.api.volumeShip() //平台成交总量 船舶

          this.volume.goodsOrder = await this.api.volumePro() //平台成交总量 产品
        this.volume.total = await this.api.volumeTotal() //总销售额
        this.volume.total = parseFloat(this.volume.total).toFixed(2)
        this.volume.other = await this.api.volumeOther()  //其他业务
          this.initTotal()
        // this.echarts()


        this.dailySpecials = await this.api.shopDailySpecials() //每日特价
        this.activePrice = this.dailySpecials[0].id.toString()
        this.discountPro = await this.api.shopDiscountPro() //折扣产品
        this.activeDiscount = this.discountPro[0].id.toString()
        this.goodPro = await this.api.shopGoodPro() //发现好货

        for(var i=0;i<this.goodPro.length;i+=5){
          this.goodProSwiperList.push(this.goodPro.slice(i,i+5))
        }

        this.newPro = await this.api.shopNewPro() //新品首发
        let data = await this.api.shopGoodStore()  // 优质店铺
        // console.log('店铺列表')
        data.forEach((item,index) => {
          item.follow = item.follow > 10000 ? (item.follow / 10000).toFixed(1) + '万' : item.follow
          if(index<2){
            this.goodStoreList.push(item)
          }
          this.goodStoreListAll.push(item)
        })

        for(var i=0;i<this.goodStoreListAll.length;i+=7){
          this.storeSwiperList.push(this.goodStoreListAll.slice(i,i+7))
        }
        // console.log(this.storeSwiperList)

        this.adertList2 = await this.api.getAdert('onlineAder2')

        this.rankList = await this.api.goodsRank()
        this.rankList[0].rankImg = this.first
        this.rankList[1].rankImg = this.second
        this.rankList[2].rankImg = this.third
        // this.couponList = await this.api.couponList()
        // console.log(this.couponList)
        this.tjList = await this.api.forYouGoods();
        this.forYouList = this.tjList[0].items

        // this.goodProScroll = this.goodPro.length > 5 ? true : false
        // let flagS=0
        // if (this.goodProScroll) {
        //   this.goodProScroll=false
        //   this.goodProScrollTime = setInterval(() => {
        //     if(flagS==0){
        //       flagS++
        //       this.goodProScroll=true
        //     }
        //     this.goodPro.push(this.goodPro.shift())
        //  }, 3000)
        // }

        // this.goodStoreScroll = this.goodStoreListAll.length > 5 ? true : false
        // let flagS2=0
        // if (this.goodStoreScroll) {
        //   this.goodStoreScroll=false
        //   this.goodStoreScrollTime = setInterval(() => {
        //     if(flagS2==0){
        //       flagS2++
        //       this.goodProScroll=true
        //     }
        //     this.goodStoreScroll=true
        //     this.goodStoreListAll.push(this.goodStoreListAll.shift())
        //  }, 2500)
        // }
      },
      //跳到设计或船厂详情
      toDesignDetail(item) {
        let url = ''
        if (item.identityCd === 'identity20') {//设计院
          url = '/shipDisplay/design?id=' + item.id
        }else if (item.identityCd === 'identity30') {//船厂
          url = '/shipDisplay/shipyard?id=' + item.id
        }else if (item.identityCd === 'identity40') {//监理单位
          url = '/shipDisplay/JLDetail?id=' + item.id
        }
        this.$router.push(url)
      },
      //跳转产品详情
      toProDetail(item) {
        let url = '/Trade/productDetail?id='+item.goodsId + "&type=" + (item.ship ? 'ship' : 'product')
        this.$router.push(url)
        // window.open(url)
      },
      //跳转店铺详情
      toShipDetail(id) {
        window.open('/sinovat/Trade/storeHome?shopId='+id)
      },

      changeActive(index,item) {
        this.tjList.forEach(item => {
          if (item.isActive === true) {
              item.isActive = false
          } else {

          }
        });
        this.tjList[index].isActive = true;
        this.forYouList = item.items;
      },
      //优质供应商
      goodStoreScrollState(type) {
        // if (this.goodStoreListAll.length <= 5) {
        //   return
        // }
        // if (type) { //鼠标滑入，滚动停止
        //   this.goodStoreScroll = false
        //   clearInterval(this.goodStoreScrollTime)

        // } else {
        //   this.goodStoreScroll = true
        //   this.goodStoreScrollTime = setInterval(() => {
        //     this.goodStoreListAll.push(this.goodStoreListAll.shift())
        //   }, 2500)
        // }
      },
      //发现好货滚动状态
      goodProScrollState(type) {
        // if (this.goodPro.length <= 5) {
        //   return
        // }
        // if (type) { //鼠标滑入，滚动停止
        //   this.goodProScroll = false
        //   clearInterval(this.goodProScrollTime)

        // } else {
        //   this.goodProScroll = true
        //   this.goodProScrollTime = setInterval(() => {
        //     this.goodPro.push(this.goodPro.shift())
        //   }, 3000)
        // }
        // console.log(this.goodProScroll)
      },
      toPage(item) {
        if (item.nm === '设计预约') {
          this.show = true
        } else {
          this.$router.push(item.href)
        }
      },
      close() {
        // 取消弹窗回调
        this.show = false
      },
      submit() {
        // 确认弹窗回调
        this.show = false
      },
    },
    async asyncData(context){
      if(process.client){
        return
      }
      let arr = []
        arr = await Promise.all([
          context.app.api.shopHomeAdert(),
          context.app.api.getRoleList(),
          context.app.api.shopRecomdPro(),
          context.app.api.getAdert('onlinetrad'),
          context.app.api.shopDailySpecials(),
          context.app.api.shopDiscountPro(),
          context.app.api.shopGoodPro(),
          context.app.api.shopNewPro(),
          context.app.api.shopGoodStore(),
          context.app.api.goodsRank(),
          context.app.api.forYouGoods()
        ])
      return {
        arr:arr,
      }
    },
  }
</script>
<style lang="less">
  #home {
    //轮播图组件
    .Carousel{
      .el-carousel__item h3 {
        color: #475669;
        font-size: 14px;
        opacity: 0.75;
        line-height: 200px;
        margin: 0;
      }

      .p {
        width: 159px;
        white-space: nowrap;
        text-overflow: ellipsis;
        overflow: hidden;
        word-break: break-all;
      }
      .el-carousel__item:nth-child(2n) {
        /*background-color: #99a9bf;*/
      }

      .el-carousel__item:nth-child(2n+1) {
        /*background-color: #d3dce6;*/
      }
    }

  }
  .storeScroll{
    .el-carousel__item{
      background: #ffffff !important;
    }
  }
  .el-tabs__nav.is-top {
    border-bottom: 2px solid #FFFFFF;
  }

  .el-tabs.el-tabs--top {
    width: 100%;
  }

  .el-tabs__content {
    width: 100%;
  }

  .swiper-container {
    width: 100%;
    height: 100%;
    /*background-color: #F3F3F3;*/
    --swiper-theme-color: #999999; /* 设置Swiper风格 */
    --swiper-navigation-color: #999999;
  }

  .el-tabs__nav-wrap::after {
    background-color: #FFFFFF;
  }
  .midSwiper .swiper-slide{
    border-right: 1px solid;
    border-image: -webkit-linear-gradient(to top,#ffffff , rgba(213,213,213,0.6) , #ffffff)1 10 1; /* 控制边框颜色渐变 */
    border-image: -moz-linear-gradient(to top,#ffffff , rgba(213,213,213,0.6) , #ffffff)1 10 1;
    border-image: linear-gradient(to top,#ffffff , rgba(213,213,213,0.6) , #ffffff)1 10 1; /* 标准的必须写在最后 */
  }


  #tab-0 {
    color: #121214;
    font-size: 20px;
    font-weight: bold;
    flex: 1;
    padding-left: 20px;
  }

  .el-tabs__nav.is-top {
    width: 100%;
    display: flex;
    align-items: center;
    padding-top: 8px;
    height: 50px;
    line-height: 46px;
  }

  #tab-4 {
    color: #999999;
    flex: 0;
    padding-right: 20px;
  }

  #tab-5 {
    color: #999999;
    flex: 0;
    padding-right: 20px;
  }

  .tabAAA {
    width: 100%;
    //设计院广告筛选图下划线 白色
    display: flex;
    justify-content: center;
    flex-wrap: wrap;

    p {
      color: #9A9A9A;
    }

    .el-tabs__active-bar {
      background-color: #FFFFFF;
    }

    // .el-tabs__item.is-active {
    //   color: #0082F7;
    // }
    #pane-first.el-tab-pane {
      height: 70px;
    }

    #pane-second.el-tab-pane {
      height: 70px;
    }

    #pane-third.el-tab-pane {
      height: 70px;
    }

    #pane-fourth.el-tab-pane {
      height: 70px;
    }

    .el-tabs__header {
      margin: 0;
    }
  }

  .grayColor {
    background-color: #999999;
    color: #999999;
    opacity: 100%;
  }

  .swiper-button-black {
    opacity: 0.3;
    background-size: contain;
    width: 13px;
  }

  .el-tabs__item.is-active {
    color: #FF3E3E;
  }

  .el-tabs__active-bar {
    background-color: #FF3E3E;
  }

  .price .is-top:hover, .discount .is-top:hover {
    color: #FF3E3E !important;
  }
  .price,.tabZ{
    .el-tabs__item{
      font-weight: 600;
    }
  }

</style>
<style lang="less" scoped>
  @import url("../../assets/css/init.less");

  #home {
    background-color: #F3F3F3;
    background-image: url("../../assets/img/footer.png");
    background-position: center bottom;
    background-repeat: repeat-x;
    padding-bottom: 80px;
  }
/deep/ .swiper-slide {
  text-align: center;
  font-size: 12px;
  background: #FFFFFF;
  height: 100%;

  /* Center slide text vertically */
  display: -webkit-box;
  display: -ms-flexbox;
  display: -webkit-flex;
  display: flex;
  -webkit-box-pack: center;
  -ms-flex-pack: center;
  -webkit-justify-content: center;
  justify-content: center;
  -webkit-box-align: center;
  -ms-flex-align: center;
  -webkit-align-items: center;
  align-items: center;
}
  .whiteBg {
    background: #ffffff;
  }

  .grayBg {
    /*background: #F5F5F5;*/
  }

  //导航栏、轮播图、交易量
  .backgroundByImg {
    padding-bottom: 20px;
    padding-top: 10px;
    /*background-color: #F3F3F3;*/
    margin: 0 auto;
    text-align: center;
    display: flex;
    justify-content: space-between;

    .classify {
      /*background: red;*/
      width: 18.35%;
    }

    //轮播图
    .Carousel {
      width: 55.5%;
      display: inline-block;
      vertical-align: top;
      height: 100%;
      /*cursor: pointer;*/

      .bannerImg {
        background-repeat: no-repeat;
        background-position: center;
        background-size: cover;
        width: 100%;
        height: 100%;
      }

    }

    //平台交易总量
    .Volume {
      width: 25.25%;
      display: inline-flex;
      vertical-align: top;
      height: 440px;
      background-color: #ffffff;
      flex-direction: column;

      .view {
        width: 100%;
        display: flex;
        flex-direction: column;
        align-items: center;
        /*flex-wrap: wrap;*/
        height: 320px;
        /*justify-content: space-around;*/
        border-bottom: 1px solid #F3F3F3;
        /*position: relative;*/
        #pieEcharts{
          height: 150px;
          width: 100%;
        }


        > p {
          font-size: 17px;
          color: #343434;
          width: 100%;
          padding: 30px 0 70px 20px;
          text-align: left;
          span{
            color: @redColor;
          }
          /*padding-top: 10px;*/
          /*padding-bottom: 20px;*/
          /*line-height: 17px;*/
          /*display: flex;*/
          /*align-items: center;*/
          /*justify-content: space-between;*/
          /*.line1 {*/
            /*width: 49px;*/
            /*height: 1px;*/
            /*background: #000;*/
            /*opacity: 0.1;*/
            /*margin-left: 18px;*/
          /*}*/

          /*.line2 {*/
            /*width: 49px;*/
            /*height: 1px;*/
            /*background: #000;*/
            /*opacity: 0.1;*/
            /*margin-right: 20px;*/
          /*}*/
        }

        //交易字体
        .textRed {
          font-weight: 600;
          color: #FF3E3E;
        }

        //船舶订单
        .order {
          color: #999999;
          width: 46%;
          display: flex;
          flex-wrap: wrap;
          flex-direction: column;

          > p {
            line-height: 30px;
          }
        }

        //订单总量
        .totalOrder {
          color: #999999;
          padding-bottom: 10px;
          width: 46%;
          display: flex;
          flex-wrap: wrap;
          flex-direction: column;

          > p {
            line-height: 30px;
          }
        }
      }

      //设计院
      .design {
        height: 130px;
        width: 100%;

        .tab {
          width: 100%;
          text-align: left;
          height: 37px;
          white-space: nowrap;
          padding: 0 0px 0 5px;
          box-sizing: border-box;

          p {
            display: inline-block;
            color: #999999;
            height: 35px;
            line-height: 35px;
            margin: 0 5px;
            cursor: pointer;
          }

          p.active {
            color: #333333;
            border-bottom: 2px solid @themeColor;
          }
        }

        .designList {
          height: 93px;
          padding: 20px 0;
          box-sizing: border-box;

          img {
            cursor: pointer;
          }
        }

        .swiper {
          width: 100%;
        }
      }

      .reservation {
        cursor: pointer;
        flex: 1;

        .servesA {
          width: 33.3%;
          height: 120px;
          margin: 0;
          float: left;
          display: inline-flex;
          flex-wrap: wrap;
          flex-direction: column;
          justify-content: center;
          border-top: 1px solid #F3F3F3;
          border-right: 1px solid #F3F3F3;
          align-items: center;

          &:nth-of-type(3n+3) {
            border-right: 0;
          }

          &:nth-of-type(2) {
            border-bottom: 1px solid #F3F3F3;
          }
          &:nth-of-type(3) {
            border-bottom: 1px solid #F3F3F3;
          }

          > p {
            margin: 0 auto;
            width: 100%;
            margin-bottom: 5px;
          }

          > img {
            width: 22%;
            margin-top: 10px;
            margin-bottom: 10px;
          }
        }
      }
    }
  }

  /*重点推荐*/
  .hotEquip {
    padding-bottom: 20px;

  }

  //白色背景
  .background {
    background-color: #FFFFFF;
    margin: 0 auto;
    display: flex;
    justify-content: space-between;

    .begin {
      height: 250px;
      display: inline-block;
      vertical-align: top;
      width: 16%;
      background: #FFFFFF;

      .singleA {
        width: 93%;
        height: 240px;
        margin: 0 auto;
        display: flex;
        align-items: center;
        flex-wrap: wrap;
        flex-direction: column;
        justify-content: center;
        cursor: pointer;
        background: -moz-linear-gradient(top, #ffffff 0%, #EFF1F6 100%);
        background: -webkit-gradient(linear, left top, left bottom, color-stop(0%, #ffffff), color-stop(100%, #EFF1F6));
        background: -webkit-linear-gradient(top, #ffffff 0%, #EFF1F6 100%);
        background: -o-linear-gradient(top, #ffffff 0%, #EFF1F6 100%);
        background: -ms-linear-gradient(top, #ffffff 0%, #EFF1F6 100%);
        background: linear-gradient(to bottom, #ffffff 0%, #EFF1F6 100%);

        .img {
          width: 132px;
          height: 132px;
          display: flex;
          display: -webkit-flex;

          > img {
            margin: auto;
          }
        }

        > p {
          color: #666666;
          line-height: 30px;
        }

        .textRed {
          color: #FF3E3E;
          font-size: 16px;
        }
      }
    }

    //中部
    .midSwiper {
      height: 250px;
      display: inline-block;
      vertical-align: top;
      width: 66.6%;
      background: #FFFFFF;
      /*border: 1px solid #F3F3F3;*/
      border-left: 1px solid;
      border-right: 1px solid;
      border-image: -webkit-linear-gradient(to top,#ffffff , rgba(213,213,213,0.6) , #ffffff)1 10 1; /* 控制边框颜色渐变 */
      border-image: -moz-linear-gradient(to top,#ffffff , rgba(213,213,213,0.6) , #ffffff)1 10 1;
      border-image: linear-gradient(to top,#ffffff , rgba(213,213,213,0.6) , #ffffff)1 10 1; /* 标准的必须写在最后 */
      cursor: pointer;

      > p {
        text-align: center;
      }

      .swiper {
        display: flex;
        flex-direction: column;
        justify-content: space-around;

        .swiper-button-prev {
          left: 0;
          background-image: none!important;
        }

        .swiper-button-next {
          right: 0;
          background-image: none!important;
        }

        .productSwiper {
          width: 159px;
          .img {
            height: 118px;
            width: 159px;
            margin-bottom: 10px;
            display: flex;
            display: -webkit-flex;

            > img {
              margin: auto;
            }
          }

          > p {
            font-size: 14px;
            color: #666666;
            text-align: left;
            display: -webkit-box;
            -webkit-box-orient: vertical;
            -webkit-line-clamp:1;
            overflow: hidden;
          }

          .productMiniSwiper {
            display: flex;
            display: -webkit-flex;
            align-items: center;
            justify-content: center;
            width: 159px;
            margin-bottom: 10px;
            margin-top: 10px;

            p:nth-child(1) {
              background-color: #FF3E3E;
              color: #FFFFFF;
              text-align: center;
              line-height: 31px;
              height: 31px;
              flex: 1;
            }

            .childA {
              height: 31px;
              color: #999999;
              font-size: 12px;
              border-top: 1px solid #FF3E3E;
              border-right: 1px solid #FF3E3E;
              border-bottom: 1px solid #FF3E3E;
              display: flex;
              display: -webkit-flex;
              padding-right: 5px;
              box-sizing: border-box;
              align-items: center;
              justify-content: center;
              span{
                margin: auto;
              }
              img{
                width: 18px;
                margin: auto;
              }
            }
          }
        }
      }
    }

    .minSwiper {
      height: 250px;
      display: inline-block;
      vertical-align: top;
      width: 16.6%;
      background: #FFFFFF;
      border: 1px solid #F3F3F3;
      cursor: pointer;

      > p {
        text-align: center;

      }
    }
  }

  //每日特价、折扣产品
  .backgroundBySale {
    background-color: #F3F3F3;
    // height: 313px;
    display: flex;
    margin: 0 auto;
    justify-content: space-between;
    //每日特价
    .price {
      width: 49.6%;
      background-color: #FFFFFF;

      .more {
        cursor: pointer;
      }
    }

    //折扣产品
    .discount {
      width: 49.6%;
      background-color: #FFFFFF;

      .more {
        cursor: pointer;
      }

      .tabZ {
        width: 100%;
        display: flex;
      }
    }
  }

  //发现好货
  .backgroundByGoods {
    background-color: #FFFFFF;
    height: 356px;
    margin: 0 auto;
    //上部标题
    .titleH {
      text-align: center;
      height: 84px;
      width: 100%;

      > p {
        display: flex;
        display: -webkit-flex;
        align-items: center;
        justify-content: center;

        img {
          width: 28px;
          margin: 0 10px;
        }

        font-size: 22px;
        color: #333333;
        line-height: 84px;
        font-weight: bold;
      }
    }

    //下部内容
    .textMain {
      width: 100%;
      height: 270px;

      box-sizing: border-box;
      white-space: nowrap;
      overflow-y: hidden;

      .goodProScroll {
        margin-left: 0;
      }

      .partMain {
        margin-left: 20px;
        height: 260px;
        width: 190px;
        text-align: center;
        display: inline-block;
        cursor: pointer;

        .img {
          width: 190px;
          height: 190px;
          display: flex;
          display: -webkit-flex;

          img {
            margin: auto;
          }
        }

        > p {
          width: 190px;
          color: #333333;
          margin-top: 23px;
          font-size: 14px;
          line-height: 14px;
          overflow: hidden;
          text-overflow: ellipsis;
          white-space: nowrap;
          cursor: pointer;

        }

      }
    }
    .storeScroll{
      .storeSwiper{
        width: 100%;
        display: flex;
        justify-content: space-around;
        .swiperItem{
          .el-images{
            width: 125px;
            height: 125px;
          }
        }
        .proItem{
          text-align: center;
          .img {
            width: 190px;
            height: 190px;
            display: flex;
            display: -webkit-flex;

            img {
              margin: auto;
            }
          }
          > p {
            width: 190px;
            color: #333333;
            margin-top: 23px;
            font-size: 14px;
            line-height: 14px;
            overflow: hidden;
            text-overflow: ellipsis;
            white-space: nowrap;
            cursor: pointer;

          }
        }
      }

    }

    /*滚动*/
    .scrollToLeftStore{
      animation: toScrollStore 2.5s linear infinite;
      -webkit-animation: toScrollStore 2.5s linear infinite; /* Safari 和 Chrome */
      margin-left: -145px;
    }
    @keyframes toScrollStore {
      from {
        margin-left: 0px;
      }
      to {
        margin-left: -145px;
      }
    }

    @-moz-keyframes toScrollStore /* Firefox */ {
      from {
        margin-left: 0px;
      }
      to {
        margin-left: -145px;
      }
    }

    @-webkit-keyframes toScrollStore /* Safari 和 Chrome */ {
      from {
        margin-left: 0px;
      }
      to {
        margin-left: -145px;
      }
    }

    @-o-keyframes toScrollStore /* Opera */ {
      from {
        margin-left: 0px;
      }
      to {
        margin-left: -145px;
      }
    }

    .scrollToLeft {
      animation: toScroll 3s linear infinite;
      -webkit-animation: toScroll 3s linear infinite; /* Safari 和 Chrome */
      margin-left: -210px;
    }

    @keyframes toScroll {
      from {
        margin-left: 0px;
      }
      to {
        margin-left: -210px;
      }
    }

    @-moz-keyframes toScroll /* Firefox */ {
      from {
        margin-left: 0px;
      }
      to {
        margin-left: -210px;
      }
    }

    @-webkit-keyframes toScroll /* Safari 和 Chrome */ {
      from {
        margin-left: 0px;
      }
      to {
        margin-left: -210px;
      }
    }

    @-o-keyframes toScroll /* Opera */ {
      from {
        margin-left: 0px;
      }
      to {
        margin-left: -210px;
      }
    }
  }

  //新品首发，排行榜单，优质店铺，领券中心
  .backgroundByAllOfThese {
    background-color: #F3F3F3;
    height: 352px;
    margin: 0 auto;
    display: flex;
    justify-content: space-between;

    .part1 {
      height: 352px;
      width: 24.33%;
      background-color: #FFFFFF;

      .upUp {
        display: inline-flex;
        height: 62px;
        width: 100%;
        justify-content: space-between;

        p:nth-child(1) {
          font-size: 20px;
          line-height: 62px;
          padding-left: 20px;
          color: #333333;
          font-weight: bold;
        }

        p:nth-child(2) {
          font-size: 14px;
          padding-top: 26px;
          color: #999999;
          padding-right: 14px;
          cursor: pointer;

        }
      }

      //Part1下部内容
      .main1 {
        display: flex;
        flex-direction: column;
        cursor: pointer;

        .img {
          margin: 0 auto;
          height: 217px;

          > img {
            margin: auto;
          }
        }

        > p {
          padding-top: 10px;
          text-align: center;
          color: #333333;
          cursor: pointer;
        }

        .textRed {
          font-size: 16px;
          color: #FF3E3E;
          cursor: pointer;
        }
      }
    }

    //part2内容
    .part2 {
      height: 352px;
      width: 24.33%;
      background-color: #FFFFFF;
      //头部导航栏
      .upUp {
        display: inline-flex;
        justify-content: space-between;
        height: 62px;
        width: 100%;

        p:nth-child(1) {
          font-size: 20px;
          line-height: 62px;
          padding-left: 20px;
          color: #333333;
          font-weight: bold;
        }

        p:nth-child(2) {
          font-size: 14px;
          padding-top: 26px;
          color: #999999;
          padding-right: 14px;
          cursor: pointer;
        }
      }

      //下部内容栏
      .main2 {
        display: flex;
        flex-direction: column;
        justify-content: center;
        cursor: pointer;
        //第一部分
        .section1 {
          width: 100%;
          /*width: 256px;*/
          padding-left: 18px;
          padding-right: 14px;
          margin: 0 auto;
          display: inline-flex;
          justify-content: space-around;
          padding-bottom: 10px;
          align-items: center;

          > img:first-child {
            width: 34px;
            height: 40px;
          }

          .centerImg {
            border: 1px solid #F3F3F3;
            width: 85px;
            height: 85px;
          }

          > p {
            width: 32.9%;
            font-size: 14px;
            color: #666666;
            overflow: hidden;
            text-overflow: ellipsis;
            display: -webkit-box;
            -webkit-line-clamp: 2;
          }
        }

      }
    }

    //part3 内容
    .part3 {
      height: 352px;
      width: 24.33%;
      background-color: #FFFFFF;
      //头部导航栏
      .upUp {
        display: inline-flex;
        height: 62px;
        width: 100%;
        justify-content: space-between;

        p:nth-child(1) {
          font-size: 20px;
          line-height: 62px;
          padding-left: 20px;
          color: #333333;
          font-weight: bold;
        }

        p:nth-child(2) {
          font-size: 14px;
          padding-top: 26px;
          color: #999999;
          padding-right: 14px;
          cursor: pointer;
        }
      }

      //下部内容栏
      .main3 {
        width: 86.4%;
        margin: 0 auto;
        cursor: pointer;
        //一
        > div {
          display: inline-flex;
          height: 128px;
          width: 100%;
          background-color: #F1F5F0;
          margin-bottom: 14px;
          cursor: pointer;
          //内部一
          .shareA {
            display: flex;
            justify-content: space-around;
            flex-direction: column;
            width: 52.4%;

            p:nth-child(1) {
              width: 112px;
              padding-left: 20px;
              font-size: 14px;
              color: #666666;
            }

            p:nth-child(2) {
              width: 112px;
              padding-left: 20px;
              font-size: 12px;
              color: #999999;
            }
          }

          //内部二
          .shareB {
            display: flex;
            align-items: center;

            > img {
              width: 87px;
              height: 87px;
              border: 1px solid #F3F3F3;
              margin-right: 13px;
            }
          }

          &:nth-of-type(2) {
            background-color: #F1F8F7;
          }
        }
      }
    }

    //第四部分
    .part4 {
      height: 352px;
      width: 24.33%;
      background-color: #FFFFFF;

      /deep/.el-carousel--horizontal{
        height: 100%;
      }
      /deep/.el-carousel__container{
        height: 100%;
      }
      //顶部导航栏
      .upUp {
        display: inline-flex;
        height: 62px;
        width: 100%;
        justify-content: space-between;

        p:nth-child(1) {
          font-size: 20px;
          line-height: 62px;
          padding-left: 20px;
          color: #333333;
          font-weight: bold;
        }

        p:nth-child(2) {
          font-size: 14px;
          padding-top: 26px;
          padding-right: 14px;
          color: #999999;
          cursor: pointer;
        }
      }

      //下部内容
      .main4 {
        padding: 0 20px;
        //以下为优惠券部分 左右为图片 中部可编辑
        .sectionA {
          cursor: pointer;
          margin-bottom: 6px; //设计图间距为10px
          /*justify-content: space-around;*/
          display: flex;
          display: -webkit-flex;

          > img {
            width: 8px;
            height: 85px;

            &:last-of-type {
              width: 50px;
              height: 85px;
            }
          }

          .core {
            flex: 1;
            background-color: #FF3E3E;
            height: 85px;
            display: flex;
            flex-direction: column;
            justify-content: center;
            padding-left: 10px;

            p:nth-child(1) {
              color: #FFFFFF;

              > span {
                font-size: 30px;
                font-weight: bold;
              }
            }

            p:nth-child(2) {
              color: #FEFEFE;
              opacity: 0.5;
            }
          }
        }
      }
    }
  }

  //为您推荐
  .backgroundByRecommend {
    width: 100%;
    display: flex;
    justify-content: center;
    flex-direction: column;
    margin: 0 auto;
    text-align: center;
    //为您推荐标题
    .mainTitle {
      width: 100%;
      display: flex;
      justify-content: center;
      margin-top: 15px;

      > p {
        display: flex;
        display: -webkit-flex;
        align-items: center;
        color: #333333;
        font-weight: bold;
        font-size: 22px;
      }

      img {
        width: 28px;
        margin: 0 10px;
      }
    }

    //导航栏
    .mainTabA {
      display: flex;
      width: 100%;
      margin-top: 30px;
      height: 93px;
      background-color: #FFFFFF;
      justify-content: space-around;

      .tabOption {
        display: flex;
        flex-direction: column;
        justify-content: space-around;
        padding-top: 20px;
        padding-bottom: 20px;
        flex:1;

        &:not(:last-of-type){
          border-right: 1px solid;
          border-image: -webkit-linear-gradient(#ffffff, rgba(213, 213, 213, 0.6), #ffffff) 1 10 1; /* 控制边框颜色渐变 */
          border-image: -moz-linear-gradient(#ffffff, rgba(213, 213, 213, 0.6), #ffffff) 1 10 1;
          border-image: linear-gradient(#ffffff, rgba(213, 213, 213, 0.6), #ffffff) 1 10 1; /* 标准的必须写在最后 */
        }
        p:nth-child(1) {
          line-height: 31px;
          font-size: 16px;
          cursor: pointer;
          color: #343434;
          margin: 0 auto;
          width: 91px;
        }

        p:nth-child(2) {
          line-height: 31px;
          font-size: 14px;
          color: #999999;
          cursor: pointer;
          margin: 0 auto;
          width: 91px;
        }

        /*&:first-of-type{*/
        /*  p:nth-child(1){*/
        /*    line-height: 31px;*/
        /*    font-size: 16px;*/
        /*    cursor: pointer;*/
        /*    color: #FFFFFF;*/
        /*    background-color: #FF3E3E;*/
        /*    border-radius: 15px;*/
        /*  }*/
        /*  p:nth-child(2){*/
        /*    line-height: 31px;*/
        /*    font-size: 14px;*/
        /*    color: #FF3E3E;*/
        /*    cursor: pointer;*/
        /*  }*/
        /*}*/
      }

      .activeTab {
        p:nth-child(1) {
          line-height: 31px;
          font-size: 16px;
          cursor: pointer;
          color: #FFFFFF;
          background-color: #FF3E3E;
          border-radius: 15px;
        }

        p:nth-child(2) {
          line-height: 31px;
          font-size: 14px;
          color: #FF3E3E;
          cursor: pointer;
        }

        /*p:nth-child(1){*/
        /*  line-height: 31px;*/
        /*  font-size: 16px;*/
        /*  cursor: pointer;*/
        /*  color: #FF3E3E;*/
        /*  font-weight: bold;*/
        /*}*/
        /*p:nth-child(2){*/
        /*  line-height: 31px;*/
        /*  font-size: 14px;*/
        /*  color: #FF3E3E;*/
        /*  cursor: pointer;*/
        /*  font-weight: bold;*/
        /*}*/
      }
    }

    //下方商品
    .tabDown {

    }
  }

  .footer {
    margin-top: 50px;

    > p {
      color: #999999;
      margin-bottom: 20px;
      cursor: pointer;
      display: inline-block;
    }
  }

  .footerImg {
    width: 100%;
    background-color: #F3F3F3;
  }

  .newTotal{
    width: 100%;
    padding: 4px 18px 4px 6px;
    box-sizing: border-box;
    li{
      display: flex;
      align-items: center;
      justify-content: space-between;
      padding: 10px 0;
      border-bottom: 1px solid rgba(0,0,0,0.1);
      &:last-child{
        border-bottom: none;
      }
      div:nth-of-type(1){
        width: 120px;
        display: flex;
        align-items: center;
        justify-content: center;
        img{
          display: none;
          width: 41px;
          height: 41px;
          margin-right: 8px;
        }
        p{
          span{
            display: block;
          }
          span:nth-of-type(1){
            font-weight: bold;
            color: #2778BE;
            font-size: 16px;
            margin-bottom: 2px;
            white-space: nowrap;
          }
          span:nth-of-type(2){
            color: #747474;
            font-size:14px;
            white-space: nowrap;
            font-weight: bold;
          }
        }
      }
      div:nth-of-type(2){
        color: #FF0000;
        font-size: 22px;
        display: flex;

        justify-content: space-between;
        flex-wrap: nowrap;
        span:nth-of-type(1){
          font-weight: bold;
          display: block;
          text-align: left;

        }
        span:nth-of-type(2){
          padding-top: 2px;
          font-weight: bold;
          margin-left: 5px;font-size: 8px
        }
      }
    }
  }
  .tradeTotal{
    width: 100%;
    box-sizing: border-box;
    background: url('../../assets/img/onlineTrade/商城背景.png');
    height: 100%;
    padding: 20px;
    display: flex;
    align-items: center;
    justify-content: center;
    flex-direction: column;
    p{
      width: 100%;
      /*text-align: left;*/
      color: white;

    }
    p:nth-of-type(1){
      margin-bottom: 20px;
      font-size: 19px;
      font-weight: bold
    }
    p:nth-of-type(2){
      font-weight: bold;
      font-size: 32px;
      span{
        margin-left: 5px;font-size: 19px
      }
    }
  }
</style>
