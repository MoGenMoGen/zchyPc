<template>
  <div id="app">
    <div class="header">
      <p><span class="lineC"></span>投标报名</p>
      <span @click="back">
        < 返回</span>
    </div>
    <div class="body">
      <div class="top-menu">
        <div v-for="(item,index) in info" :key="index" class="top-menu-item" @click="selectThis(index)">
          <img :src="item.selectImgUrl" v-if="selectIndex==index">
          <img :src="item.imgUrl" v-else>
          <div>
            <span style="color: #0064B2;">{{item.afficheTypeNm}}</span>
            <span :style="{color:selectIndex==index?'#0064B2':''}">{{item.releTm}}</span>
          </div>
          <img :src="jt1" v-if="selectIndex>=index&&info.length - 1>index">
          <img :src="jt2" v-if="selectIndex<index&&info.length - 1>index">
        </div>
      </div>
      <div class="title">{{title}}</div>
      <div class="cont-body" v-html="cont">

      </div>
    </div>
  </div>
</template>

<script>
  import {
    mapState
  } from "vuex";
  import cg1 from '@/assets/img/bidAffiche/采购公告.png'
  import cg2 from "@/assets/img/bidAffiche/采购公告-灰.png";
  import gz1 from '@/assets/img/bidAffiche/更正公告.png'
  import gz2 from '@/assets/img/bidAffiche/更正公告-灰.png'
  import jg1 from '@/assets/img/bidAffiche/结果公告.png'
  import jg2 from '@/assets/img/bidAffiche/结果公告-灰.png'
  import jt1 from '@/assets/img/bidAffiche/箭头.png'
  import jt2 from '@/assets/img/bidAffiche/箭头-灰.png'
  export default {
    components: {

    },
    layout: 'person',
    name: "bidAfficeDeatil",
    data() {
      return {
        jt1,
        jt2,
        info:[],
        selectIndex: 0,
        cont: '',
        infoId: '',
        title: ''
      }
    },
    computed: {
      ...mapState([
        'bWidth',
        'width',
      ])
    },
    watch: {
      $route(){
        this.infoId = this.$route.query.id
      },
    },
    mounted() {
      this.infoId = this.$route.query.id
      this.getData()
    },
    methods: {
      back() {
        this.$router.go(-1)
      },
      getData(){
        let nowTm = this.until.formatTime(new Date())
        let qry = this.query.new()
        this.query.toW(qry,'bidId',this.infoId,'EQ')
        this.query.toO(qry,'afficheTypeCd','esc')
        this.query.toW(qry, 'audit', '2', 'EQ')
        this.query.toW(qry, 'releTm', nowTm, 'LT')
        this.api.getBidAfficheList2(this.query.toEncode(qry)).then(res => {
          res.data.list.forEach(item => {
            item.releTm = item.releTm.substring(0,10)
            if (item.afficheTypeCd == "5635882628584448") {
              item.selectImgUrl = cg1;
              item.imgUrl = cg2;
            } else if (item.afficheTypeCd == "5635883070706688") {
              item.selectImgUrl = gz1;
              item.imgUrl = gz2;
            } else if (item.afficheTypeCd == "5635883361522688") {
              item.selectImgUrl = jg1;
              item.imgUrl = jg2;
            }
            item.cont = this.until.imgStyle(item.cont)
          })
          this.info = res.data.list
          this.selectIndex = this.info.length - 1
          this.cont = this.info[this.selectIndex].cont
          this.title = this.info[this.selectIndex].title
        })
      },
      selectThis(index) {
        this.selectIndex = index
        this.cont = this.info[this.selectIndex].cont
        this.title = this.info[this.selectIndex].title
      },
    },
  }
</script>
<style lang="less" scoped>
  @import url("../../assets/css/init.less");

  #app {
    .header {
      display: flex;
      justify-content: space-between;
      align-items: center;
      height: 59px;
      width: 100%;
      padding: 0 24px 0 30px;
      border-bottom: 1px solid rgba(0, 0, 0, 0.1);
      ;

      >p {
        display: flex;
        align-items: center;
        font-size: 20px;
        /*font-family:Microsoft YaHei;*/
        font-weight: 400;
        color: #333333;
        height: 18px;

        .lineC {
          display: block;
          height: 18px;
          width: 3px;
          background-color: #2778BE;
          margin-right: 8px;
        }
      }

      >span {
        display: block;
        height: 12px;
        font-size: 14px;
        /*font-family:Microsoft YaHei;*/
        font-weight: 400;
        color: #666666;
        cursor: pointer;
      }
    }

    .body {
      .top-menu {
        width: 80%;
        margin: 20px auto;
        display: flex;
        align-items: center;
        .top-menu-item {
          display: flex;
          align-items: center;
          margin-right: 40px;
          cursor: pointer;
          img:first-child {
            width: 40px;
            height: 40px;
            margin-right: 10px;
          }
          div {
            display: flex;
            flex-direction: column;
            font-size: 12px;
            color: #909090;
            span:first-child {
              font-size: 16px;
            }
          }
          img:last-child {
            width: 7px;
            height: 13px;
            margin-left: 40px;
          }
        }
      }
      .title {
        width: 100%;
        text-align: center;
        color: #333333;
        font-size: 20px;
        line-height: 30px;
      }
      .cont-body{
        width: 80%;
        margin: 0 auto;
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
  }
</style>
