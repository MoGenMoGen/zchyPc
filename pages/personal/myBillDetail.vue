<template>
  <div class="main">
    <div style="padding:0 30px">
      <tobbar :title="title" :showAll="false"></tobbar>
    </div>
    <p style="border-bottom: 1px solid rgba(0,0,0,0.1);"></p>
    <div class="steps-box">
      <div class="steps-img">
        <img :src="im1">
        <img :src="im2" class="steps-line">
        <img :src="im1">
        <img :src="im2" class="steps-line" v-if="isOver">
        <img :src="im1" v-if="isOver">
        <img :src="im4" class="steps-line" v-if="!isOver">
        <img :src="im3" v-if="!isOver">
      </div>
      <div class="steps-p">
        <p>订单下单</p>
        <p>订单完成</p>
        <p>发票开具</p>
      </div>
    </div>
    <div class="table-box">
      <div class="table-title">订单信息</div>
      <div class="table-item">
        <span>订单状态</span>
        <span>已完成</span>
      </div>
      <div class="table-item">
        <span>订单编号</span>
        <span>2065562515212</span>
      </div>
      <div class="table-item">
        <span>下单时间</span>
        <span> 2021-06-20  12:00:20</span>
      </div>
    </div>
    <div class="table-box">
      <div class="table-title">发票信息</div>
      <div class="table-item">
        <span>发票状态</span>
        <span style="color: #42AB3C;" v-if="isOver">已开票</span>
        <span style="color: #F56C6C;" v-if="!isOver">未开票</span>
      </div>
      <!-- <div class="table-item" v-if="isOver">
        <span>发票内容</span>
        <span>商品明细</span>
      </div> -->
	  <div class="table-item" v-show="isOver">
	    <span>发票类型</span>
	    <span>电子普通发票</span>
	  </div>
      <div class="table-item" v-if="isOver">
        <span>发票抬头</span>
        <span>个人</span>
      </div>
    </div>
    <div class="table-box2" v-if="isOver">
      <div class="table-title">
        <p>发票代码</p>
        <p>发票号码</p>
        <p>微信扫码获取电子普通发票</p>
        <p>下载电子普通发票</p>
      </div>
      <div class="table-item">
        <div>0330021110211</div>
        <div>42815277</div>
        <div><vue-qr text="01,10,131880930821,82139728,98.23,20210926,75208378462510177130,882D," :size="200"></vue-qr></div>
        <div style="cursor: pointer;" @click="downLoadImg()"><img :src="download" class="download">下载发票</div>
      </div>
    </div>
    <div class="set-btn" v-if="isOver">发送邮箱</div>
	<div class="set-btn" v-if="!isOver">申请开票</div>
  </div>
</template>

<script>
  import tobbar from "../../components/person/tobbar";
  import im1 from "../../assets/img/personal/勾选.png";
  import im2 from "../../assets/img/personal/进度.png";
  import im3 from "../../assets/img/personal/灰钩.png";
  import im4 from "../../assets/img/personal/箭头.png";
  import download from "../../assets/img/personal/下载.png";
  import vueQr from 'vue-qr'
  import {
    mapState
  } from "vuex";
  export default {
    name: "allShip",
    components: {
      tobbar,vueQr
    },
    data() {
      return {
        im1,
        im2,
        im3,
        im4,
        download,
        title: '发票详情',
        id: '',
        isOver: true
      }
    },
    layout: 'person',
    async mounted() {
      this.id = this.until.getQueryString('id')
      console.log(this.id)
      this.api.getInvoiceResult({orderNo:id}).then(res => {
        console.log(res)
      })
    },
    computed: {
      ...mapState([
        'currentRole'
      ])
    },
    watch: {
    },
    methods: {
      downLoadImg(url) {
        window.open(url)
      }
    },
  }
</script>

<style scoped lang="less">
  .main {
    width: 100%;
    .steps-box {
      display: flex;
      flex-direction: column;
      align-items: center;
      margin-top: 60px;
      .steps-img {
        display: flex;
        align-items: center;
        font-size: 14px;
        margin-bottom: 10px;
        img {
          width: 28px;
          height: 28px;
        }
        .steps-line {
          width: 157px;
          height: 11px;
          margin: 0 14px;
        }
      }
      .steps-p {
        width: 484px;
        display: flex;
        align-items: center;
        justify-content: space-between;
        font-size: 14px;
        color: #333;
      }
    }
    .table-box {
      width: calc(100% - 60px);
      margin: 30px auto;
      border: 1px solid #E5E5E5;
      display: flex;
      flex-direction: column;
      .table-title {
        width: 100%;
        background: #F5F5F5;
        height: 40px;
        padding-left: 30px;
        box-sizing: border-box;
        line-height: 40px;
      }
      .table-item {
        display: flex;
        align-items: center;
        border-bottom: 1px solid #E5E5E5;
        span:first-child {
          width: 13%;
          display: flex;
          justify-content: center;
          align-items: center;
          border-right: 1px solid #E5E5E5;
          padding: 18px 0;
        }
        span:last-child {
          width: 87%;
          display: flex;
          align-items: center;
          padding-left: 50px;
          box-sizing: border-box;
        }
      }
      .table-item:last-child {
        border-bottom: none;
      }
    }
    .table-box2 {
      width: calc(100% - 60px);
      margin: 30px auto;
      border: 1px solid #E5E5E5;
      display: flex;
      flex-direction: column;
      .table-title {
        width: 100%;
        background: #F5F5F5;
        height: 40px;
        display: flex;
        align-items: center;
        p {
          flex: 1;
          text-align: center;
        }
      }
      .table-item {
        display: flex;
        align-items: center;
        div {
          height: 130px;
          flex: 1;
          border-right: 1px solid #E5E5E5;
          display: flex;
          justify-content: center;
          align-items: center;
          .erCode {
            width: 97px;
            height: 97px;
          }
          .download {
            width: 19px;
            height: 19px;
            margin-right: 10px;
          }
        }
        div:last-child {
          border-right: none;
        }
      }
    }
    .set-btn {
      width: 130px;
      height: 40px;
      border: 1px solid #E4393C;
      display: flex;
      justify-content: center;
      align-items: center;
      font-size: 14px;
      color: #E4393C;
      margin-left: 30px;
      border-radius: 5px;
      cursor: pointer;
    }
  }
</style>
