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
          <el-input v-model="orderCd" disabled></el-input>
        </div>
        <div class="submit-item">
          <span><span style="color: #E4393C;">*</span>发票类型：</span>
          <el-select v-model="invoice" placeholder="请选择">
            <el-option v-for="item in invoiceType" :key="item.cd" :label="item.nm" :value="item.cd"></el-option>
          </el-select>
        </div>
        <!-- <div class="submit-item">
          <span>发票内容：</span>
          <div class="submit-item-choose">
            <div class="submit-item-choose-item" @click="chooseThis(index)" v-for="(item,index) in 2" :key="index" :class="{active:selectIndex==index}">商品明细</div>
          </div>
        </div> -->
        <div class="submit-item">
          <span><span style="color: #E4393C;">*</span>抬头类型：</span>
          <el-select v-model="headUp" placeholder="请选择">
            <el-option v-for="item in headUpType" :key="item.value" :label="item.label" :value="item.label"></el-option>
          </el-select>
        </div>
        <div class="submit-item" v-if="headUp=='公司'">
          <span><span style="color: #E4393C;">*</span>单位名称：</span>
          <el-input placeholder="请输入单位名称" v-model="buyerName" clearable></el-input>
        </div>
        <div class="submit-item" v-if="headUp=='公司'">
          <span>发票税号：</span>
          <el-input placeholder="请输入发票税号" v-model="buyerTaxNum" clearable></el-input>
        </div>
        <div class="submit-item" v-if="headUp=='个人'">
          <span>发票抬头：</span>
          <el-input v-model="buyerName" clearable></el-input>
        </div>
        <div class="submit-item">
          <span>备注：</span>
          <el-input type="textarea" :rows="2" placeholder="请输入备注" v-model="rmks" resize='none'></el-input>
        </div>
      </div>
    </div>
    <div class="submit-box" v-if="headUp=='公司'">
      <div class="submit-title">
        <span>资质信息</span>
        <div></div>
      </div>
      <div class="submit-line">
        <div class="submit-item">
          <span><span style="color: #E4393C;">*</span>注册地址：</span>
          <el-input placeholder="请输入注册地址" v-model="buyerAddress" clearable></el-input>
        </div>
        <div class="submit-item">
          <span><span style="color: #E4393C;">*</span>注册电话：</span>
          <el-input placeholder="请输入注册电话" v-model="buyerTel" clearable></el-input>
        </div>
        <div class="submit-item">
          <span><span style="color: #E4393C;">*</span>开户银行：</span>
          <el-input placeholder="请输入开户银行" v-model="bank" clearable></el-input>
        </div>
        <div class="submit-item">
          <span><span style="color: #E4393C;">*</span>银行账号：</span>
          <el-input placeholder="请输入银行账号" v-model="account" clearable></el-input>
        </div>
      </div>
    </div>
    <div class="submit-box" v-if="headUp=='公司'">
      <div class="submit-title">
        <span>收票信息</span>
        <div></div>
      </div>
      <div class="submit-line">
        <div class="submit-item">
          <span><span style="color: #E4393C;">*</span>收票人姓名：</span>
          <el-input placeholder="请输入收票人姓名" v-model="linkman" clearable></el-input>
        </div>
        <div class="submit-item">
          <span><span style="color: #E4393C;">*</span>收票人手机号：</span>
          <el-input placeholder="请输入收票人手机号" v-model="phone" clearable></el-input>
        </div>
        <div class="submit-item">
          <span><span style="color: #E4393C;">*</span>收票人地区：</span>
          <addr @changeAddr="changeAddr" ref="addrChoose" style="width: 300px;"></addr>
        </div>
        <div class="submit-item">
          <span><span style="color: #E4393C;">*</span>收票人地址：</span>
          <el-input placeholder="请输入收票人地址" v-model="address" clearable></el-input>
        </div>
        <div class="submit-item">
          <span><span style="color: #E4393C;">*</span>收票人邮箱：</span>
          <el-input placeholder="请输入收票人邮箱" v-model="email" clearable></el-input>
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
  import addr from "../../components/addr";
  import {
    mapState
  } from "vuex";
  export default {
    name: "allShip",
    components: {
      tobbar,addr
    },
    data() {
      return {
        title: '申请发票',
        id: '',
        orderCd: '',
        invoice: '',
        headUp: '公司',
        rmks: '',
        buyerName: '',
        buyerTaxNum: '',
        buyerTel: '',
        bank: '',
        account: '',
        buyerAddress: '',
        linkman: '',
        phone: '',
        address: '',
        addrNm:"",
        email: '',
        type: 1,
        invoiceType: [],
        headUpType: [{
          value: '1',
          label: '公司'
        },{
          value: '2',
          label: '个人'
        }],
        selectIndex: 0,
        quaName: ''
      }
    },
    layout: 'person',
    async mounted() {
      this.id = this.until.getQueryString('id')
      this.orderCd = this.until.getQueryString('orderCd')
      this.api.dataDictionary('INVOICE_TYPE').then(res => {
        this.invoiceType = res
      })
      // this.$refs.addrChoose.getProvice()
      this.getDefault()
    },
    computed: {
      ...mapState([
        'currentRole'
      ])
    },
    watch: {
      headUp(val) {
        if(val=='个人') {
          this.buyerName = '个人'
        } else {
          this.buyerName = this.quaName
        }
      }
    },
    methods: {
      getDefault() {
        let param={
          orgEnterId: JSON.parse(this.until.seGet('currentRole')).id
        }
        this.api.getQualiInfo(param).then(res => {
          if(res.data) {
            this.quaName = res.data.nm
            this.buyerName = res.data.nm
            this.buyerTel = res.data.tel
            this.buyerTaxNum = res.data.taxNum
            this.buyerAddress = res.data.address
            this.bank = res.data.bank
            this.account = res.data.account
          }
        })
        this.api.getQualiAddrInfo(param).then(res => {
          if(res.data.list.length>0) {
            this.linkman = res.data.list[0].linkman
            this.phone = res.data.list[0].phone
            this.address = res.data.list[0].addrDetail
            this.addrNm = res.data.list[0].addrNm
            this.$refs.addrChoose.getProvice(res.data.list[0].addrNm)
            this.email = res.data.list[0].email
          } else {
            this.$refs.addrChoose.getProvice()
          }
        })
      },
      chooseThis(index) {
        this.selectIndex = index
      },
      cancel() {
        this.until.back()
      },
      //获取地区信息
      changeAddr(info){
          let data = JSON.parse(info)
          this.addrNm = data.province+'-'+data.city+'-'+data.region
      },
      submit() {
        let param
        if(this.invoice=="") {
          this.$message.error('请选择发票类型!')
          return
        }
        if(this.headUp=='公司') {
          this.type = 1
          if(this.buyerName=='') {
            this.$message.error('请输入单位名称!')
            return
          }
          if(this.buyerAddress=="") {
            this.$message.error('请输入注册地址!')
            return
          }
          if(this.buyerTel=="") {
            this.$message.error('请输入注册电话!')
            return
          }
          if(this.bank=="") {
            this.$message.error('请输入开户银行!')
            return
          }
          if(this.account=="") {
            this.$message.error('请输入银行账户!')
            return
          }
          if(this.linkman=="") {
            this.$message.error('请输入收票人姓名!')
            return
          }
          if(this.phone=="") {
            this.$message.error('请输入收票人手机号!')
            return
          }
          if(this.address=="") {
            this.$message.error('请输入收票人地址!')
            return
          }
          if(this.email=="") {
            this.$message.error('请输入收票人邮箱!')
            return
          }
          param = {
            orgEnterId: JSON.parse(this.until.seGet('currentRole')).id,
            orderId: this.id,
            orderNo: this.orderCd,
            type: this.type,
            invoiceTypeCd: this.invoice,
            buyerName: this.buyerName,
            buyerTaxNum: this.buyerTaxNum,
            buyerTel: this.buyerTel,
            buyerAddress: this.buyerAddress,
            buyerAccount: this.bank + this.account,
            linkman: this.linkman,
            phone: this.phone,
            email: this.email,
            rmks: this.rmks,
            invoiceAddr: this.addrNm.replace(/-/g,'') + this.address
          }
        } else {
          this.type = 2
          param = {
            orgEnterId: JSON.parse(this.until.seGet('currentRole')).id,
            orderId: this.id,
            orderNo: this.orderCd,
            type: this.type,
            invoiceTypeCd: this.invoice,
            buyerName: this.buyerName,
          }
        }
        console.log(param)
        this.api.invoiceApply(param).then(res => {
          console.log(res)
          if(res.msg=='成功') {
            this.$message.success('申请成功')
            setTimeout(()=> {
              this.until.back()
            },1000)
          }
        })
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
        .el-textarea {
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
