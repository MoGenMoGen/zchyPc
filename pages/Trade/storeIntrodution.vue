<template>
  <!--店铺简介-->
  <div id="shopInfo">
    <!--  <div class="header">-->
    <!--    <store-title></store-title>-->
    <!--  </div>-->
    <div class="content" :style="{width:bWidth+'px'}">
      <div class="storeContent" :style="{width:width+'px'}">
        <div class="up">
          <p>店铺简介：</p>
          <p v-html="info.synopsis"></p>
        </div>
        <div class="upp">
          <!--店铺名称-->
          <p class="child1"></p>
          <!--供应产品-->
          <p><span>供应产品：</span>{{info.mainCore}}</p>
          <!--ISO9001-->
          <p><span>ISO9001：</span></p>
          <!--公司地址-->
          <p><span>公司地址：</span>{{info.areaDetails}}</p>
          <!--联系人-->
          <!--<p><span>联系人：</span>{{info.contacts}}</p>-->
          <!--联系电话-->
          <!--<p><span>联系电话：</span>{{info.contactPhone}}</p>-->
          <!--联系邮箱-->
          <p><span>联系邮箱：</span>{{info.email}}</p>
        </div>
        <div class="down">
          <!--证照信息-->
          <span>证照信息：</span><img  alt=""/>
          <!--开店时间-->
          <p><span>开店时间：</span>{{info.openTm}}</p>
          <!--店铺主营-->
          <p><span>店铺主营：</span>{{info.mainGoods}}</p>
          <!--所在地区-->
          <p><span>所在地区：</span>{{info.area}}</p>
          <!--销售品牌-->
          <p><span>销售品牌：</span>{{info.brand}}</p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
  // import StoreTitle from "../../components/onlineTrading/storeTitle";
  import {mapState} from "vuex";
  import rz1 from "../../assets/img/proDetail/rz.png";
  export default {
    name: "storeIntrodution",
    props: ['info'],
    data() {
      return {
       // detail: '捷胜海洋专业从事海洋装备系列产品的研发、生产、销售和服务，产业主要涉及海洋渔业设备、海洋科考装备、海事海工装备、及玻璃钢船舶技术及装备，是海洋装备领域的高新技术企业',
        // uppList: [{
        //   nm: '捷胜海洋有限公司',
        //   fd1: '木材；泡沫塑料；树脂；纤维增强材料；混凝土；纤维绳；舵机',
        //   fd2: '2000质量体系认证，实验手段齐全，配有200吨拉力试验台，有员工120名，技术力量雄厚，高级工程师、工程师、高级技师、技师达30余人。',
        //   fd3: '江苏省常州市新北区春江镇魏村工业园西区滨德路',
        //   fd4: '谢静怡',
        //   fd5: '13961417298',
        //   fd6: 'cnczfrp@163.com'
        // }],
        // downList: [{
        //   fd1: '',
        //   fd2: '2020-05-20',
        //   fd3: '电气设备、金属材料、轮机设备...',
        //   fd4: '浙江 宁波',
        //   fd5: '捷胜海洋'
        // }],
      }
    },
    mounted() {
      //console.log(this.info)
    },
    computed: {
      ...mapState([
        'bWidth',
        'width'
      ])
    },
    components: {},
  }
</script>

<style lang="less" scoped>
  @import url("../../assets/css/init.less");

  #home {

  }

  .header {

  }

  .content {
    background-image: url("../../assets/img/onlineTrade/bg.png");
    background-size: 100%;
    padding: 40px 0;
    margin: 0;

    .storeContent {
      margin: 0 auto;
      display: flex;
      flex-wrap: wrap;
      background-color: #FFFFFF;
      padding: 31px;

      .up {
        p:nth-child(1) {
          color: #303030;
          font-size: 16px;
          line-height: 18px;
          font-weight: bold;
          padding-bottom: 23px;
        }

        p:nth-child(2) {
          color: #333333;
          font-size: 14px;
        }
      }

      .ad {
        margin-top: 50px;
        border: 1px solid #F3F3F3;
        width: 100%;
        height: 400px;
      }

      .upp {
        width: 100%;
        border-bottom: 1px solid #E9E9E9;
        margin-top: 50px;
        padding-bottom: 30px;
        span {
          color: #2F2F2F;
          font-size: 16px;
          line-height: 40px;
          font-weight: bold;
        }

        p {
          font-size: 16px;
          overflow: hidden;
          text-overflow: ellipsis;
          white-space: nowrap;
          line-height: 40px;
        }

        .child1 {
          font-size: 24px;
          color: #333333;
          margin: 5px 0 22px 0;
        }
      }

      .down {
        width: 100%;
        padding-top: 34px;
        padding-bottom: 30px;
        p {
          font-size: 16px;
          overflow: hidden;
          text-overflow: ellipsis;
          white-space: nowrap;
          line-height: 50px;
        }

        span {
          color: #2F2F2F;
          font-size: 16px;
          line-height: 50px;

        }

        img {
          width: 22px;
          vertical-align: middle;
        }
      }
    }
  }
</style>
