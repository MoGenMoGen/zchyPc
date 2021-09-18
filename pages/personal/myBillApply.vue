<template>
  <div class="main">
    <div style="padding:0 30px">
      <tobbar :title="title" :showAll="false"></tobbar>
    </div>
    <p style="border-bottom: 1px solid rgba(0,0,0,0.1);"></p>
    <div class="submit-box">
      <div class="submit-title">
        <span>电子发票信息</span>
        <div></div>
      </div>
      <div class="submit-line">
        <div class="submit-item">
          <span>订单编号：</span>
          <el-input v-model="id" disabled></el-input>
        </div>
        <div class="submit-item">
          <span><span style="color: #E4393C;">*</span>发票类型：</span>
          <el-select v-model="invoice" placeholder="请选择">
            <el-option v-for="item in invoiceType" :key="item.value" :label="item.label" :value="item.value"></el-option>
          </el-select>
        </div>
        <div class="submit-item">
          <span>发票内容：</span>
          <div class="submit-item-choose">
            <div class="submit-item-choose-item" @click="chooseThis(index)" v-for="(item,index) in 2" :key="index" :class="{active:selectIndex==index}">商品明细</div>
          </div>
        </div>
        <div class="submit-item">
          <span><span style="color: #E4393C;">*</span>抬头类型：</span>
          <el-select v-model="headUp" placeholder="请选择">
            <el-option v-for="item in headUpType" :key="item.value" :label="item.label" :value="item.value"></el-option>
          </el-select>
        </div>
        <div class="submit-item">
          <span><span style="color: #E4393C;">*</span>单位名称：</span>
          <el-input placeholder="请输入单位名称" v-model="unitName" clearable></el-input>
        </div>
        <div class="submit-item">
          <span>发票税号：</span>
          <el-input placeholder="请输入发票税号" v-model="invoiceTaxNumber" clearable></el-input>
        </div>
      </div>

    </div>
    <div class="submit-box">
      <div class="submit-title">
        <span>资质信息</span>
        <div></div>
      </div>
      <div class="submit-line">
        <div class="submit-item">
          <span><span style="color: #E4393C;">*</span>注册地址：</span>
          <el-input placeholder="请输入注册地址" v-model="address" clearable></el-input>
        </div>
        <div class="submit-item">
          <span><span style="color: #E4393C;">*</span>注册电话：</span>
          <el-input placeholder="请输入注册电话" v-model="address" clearable></el-input>
        </div>
        <div class="submit-item">
          <span><span style="color: #E4393C;">*</span>开户银行：</span>
          <el-input placeholder="请输入开户银行" v-model="address" clearable></el-input>
        </div>
        <div class="submit-item">
          <span><span style="color: #E4393C;">*</span>银行账号：</span>
          <el-input placeholder="请输入银行账号" v-model="address" clearable></el-input>
        </div>
      </div>
    </div>
    <div class="submit-box">
      <div class="submit-title">
        <span>收票信息</span>
        <div></div>
      </div>
      <div class="submit-line">
        <div class="submit-item">
          <span><span style="color: #E4393C;">*</span>收票人姓名：</span>
          <el-input placeholder="请输入收票人姓名" v-model="address" clearable></el-input>
        </div>
        <div class="submit-item">
          <span><span style="color: #E4393C;">*</span>收票人手机号：</span>
          <el-input placeholder="请输入收票人手机号" v-model="address" clearable></el-input>
        </div>
        <div class="submit-item">
          <span><span style="color: #E4393C;">*</span>收票人地区：</span>
          <el-input placeholder="请输入收票人地区" v-model="address" clearable></el-input>
        </div>
        <div class="submit-item">
          <span><span style="color: #E4393C;">*</span>收票人地址：</span>
          <el-input placeholder="请输入收票人地址" v-model="address" clearable></el-input>
        </div>
      </div>
    </div>
    <div class="submit-box">
      <div class="submit-item">
        <span></span>
        <div class="submit-btn">
          <el-button type="primary" style="margin-right: 30px;" @click="submit">提交</el-button>
          <el-button plain @click="cancel">取消</el-button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
  import tobbar from "../../components/person/tobbar";
  import {
    mapState
  } from "vuex";
  export default {
    name: "allShip",
    components: {
      tobbar
    },
    data() {
      return {
        title: '申请发票',
        id: '',
        invoice: '',
        headUp: '',
        unitName: '',
        invoiceTaxNumber: '',
        invoiceType: [{
          value: '1',
          label: '增值税发票'
        },{
          value: '2',
          label: '普通电子发票'
        }],
        headUpType: [{
          value: '1',
          label: '单位'
        },{
          value: '2',
          label: '个人'
        }],
        selectIndex: 0
      }
    },
    layout: 'person',
    async mounted() {
      this.id = this.until.getQueryString('id')
      console.log(this.id)
    },
    computed: {
      ...mapState([
        'currentRole'
      ])
    },
    watch: {
    },
    methods: {
      chooseThis(index) {
        this.selectIndex = index
      },
      cancel() {
        this.until.back()
      }
    },
  }
</script>

<style scoped lang="less">
  .main {
    width: 100%;
    .submit-box {
      width: calc(100% - 60px);
      margin: 20px auto;
      .submit-title {
        display: flex;
        align-items: center;
        font-size: 16px;
        color: #333333;
        margin-bottom: 20px;
        div {
          flex: 1;
          height: 1px;
          background-color: #000;
          opacity: .1;
          margin-top: 3px;
          margin-left: 10px;
        }
      }
      .submit-line {
        display: flex;
        flex-wrap: wrap;
        justify-content: space-between;
      }
      .submit-item {
        width: 48%;
        display: flex;
        align-items: center;
        font-size: 14px;
        color: #666;
        margin: 10px 0;
        span {
          width: 120px;
        }
        .submit-item-choose {
          display: flex;
          align-items: center;
          .submit-item-choose-item {
            width: 75px;
            height: 32px;
            margin-right: 20px;
            border: 1px solid #DDDDDD;
            color: #999999;
            font-size: 12px;
            display: flex;
            justify-content: center;
            align-items: center;
            cursor: pointer;
          }
          .active {
            border: 1px solid #2778BE;
            color: #2778BE;
          }
        }
        .el-input {
          width: 300px;
        }
        .el-select {
          width: 300px;
        }
        .submit-btn {
          display: flex;
          align-items: center;
        }
      }
    }
  }
</style>
