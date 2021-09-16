<template>
  <!-- 增票资质 -->
  <div>
    <div class="top-img">
      <img :src="ts">
    </div>
    <div class="tip-box" v-if="audit!=2">
      <span>您的增票资质：</span>
      <span style="color: #909090;" v-if="!isPass">未添加</span>
      <span style="color: #529F2D;" v-if="isPass">已通过审核</span>
      <p class="add-btn" v-if="!isPass" @click="addQua">添加增票资质</p>
      <p class="upd-btn" v-if="isPass" @click="update">修改</p>
      <p class="del-btn" v-if="isPass" @click="del">删除</p>
    </div>
    <div class="submit-box" v-if="audit==2">
      <div class="submit-title">
        <span>填写增票资质信息<span style="color: #E4393C;font-size: 14px;"> （所有信息均为必填）</span></span>
        <div></div>
      </div>
      <div class="submit-item">
        <span>单位名称：</span>
        <el-input placeholder="请输入单位名称" v-model="unit" clearable></el-input>
      </div>
      <div class="submit-item">
        <span>纳税人识别码：</span>
        <el-input placeholder="请输入纳税人识别码" v-model="identifier" clearable></el-input>
      </div>
      <div class="submit-item">
        <span>注册地址：</span>
        <el-input placeholder="请输入注册地址" v-model="address" clearable></el-input>
      </div>
      <div class="submit-item">
        <span>注册电话：</span>
        <el-input placeholder="请输入注册电话" v-model="phone" clearable></el-input>
      </div>
      <div class="submit-item">
        <span>开户银行：</span>
        <el-input placeholder="请输入开户银行" v-model="bank" clearable></el-input>
      </div>
      <div class="submit-item">
        <span>银行账户：</span>
        <el-input placeholder="请输入银行账户" v-model="bankAccount" clearable></el-input>
      </div>
      <div class="submit-item">
        <span></span>
        <el-checkbox v-model="checked"><span style="color: #606060;">我已阅读并同意<span style="color: #E4393C;">《增票资质确认书》</span></span></el-checkbox>
      </div>
      <div class="submit-item">
        <span></span>
        <div class="submit-btn">
          <el-button type="primary" size="small" style="margin-right: 30px;" @click="submit">提交</el-button>
          <el-button plain size="small" @click="cancel">取消</el-button>
        </div>
      </div>
    </div>
    <div class="submit-box" v-if="audit==3&&isPass">
      <div class="submit-title">
        <span>填写增票资质信息</span>
        <div></div>
      </div>
      <p>单位名称：宁波聚联科技有限公司</p>
      <p>纳税人识别码：91330212316963997H</p>
      <p>注册地址：宁波市镇海区骆驼街道文成西路南侧</p>
      <p>注册电话：0***********0</p>
      <p>开户银行： 宁波银行总行</p>
      <p>银行账户：1***************3</p>
    </div>
    <div class="submit-box" v-if="audit==3&&isPass">
      <div class="submit-title">
        <span>增票收票地址</span>
        <div></div>
        <span style="color: #2778BE;margin-left: 20px;cursor: pointer;" v-if="audit2==3" @click="update2">修改</span>
      </div>
      <div class="set-line" v-if="audit2==1">您还未设置收票地址：<div class="set-btn" @click="set()">立即设置</div></div>
      <div v-if="audit2==2">
        <div class="submit-item">
          <span>收票人姓名：</span>
          <el-input placeholder="请输入收票人姓名" v-model="name" clearable></el-input>
        </div>
        <div class="submit-item">
          <span>收票人手机号：</span>
          <el-input placeholder="请输入收票人手机号" v-model="mob" clearable></el-input>
        </div>
        <div class="submit-item">
          <span>收票人省份：</span>
          <el-input placeholder="请输入收票人省份" v-model="province" clearable></el-input>
        </div>
        <div class="submit-item">
          <span>收票人地址：</span>
          <el-input placeholder="请输入收票人地址" v-model="address2" clearable></el-input>
        </div>
        <div class="submit-item">
          <span></span>
          <div class="submit-btn">
            <el-button type="primary" size="small" style="margin-right: 30px;" @click="submit2">提交</el-button>
            <el-button plain size="small" @click="cancel2">取消</el-button>
          </div>
        </div>
      </div>
      <div v-if="audit2==3&&isPass2">
        <p>收票人姓名： 李雯文</p>
        <p>收票人手机号：120****1230</p>
        <p>收票人省份：浙江宁波市镇海区骆驼街道</p>
        <p>收票人地址：329创业社区212室</p>
      </div>
    </div>
  </div>
</template>

<script>
  import ts from "../../assets/img/personal/提示1.png";
  export default {
    name: "billQua",
    props: {

    },
    data() {
      return {
        ts,
        isPass:false,
        audit:1, //1未审核，2审核中（填写或修改），3审核通过
        unit: '',
        identifier: '',
        address: '',
        phone: '',
        bank: '',
        bankAccount: '',
        checked: false,
        isPass2: false,
        audit2: 1, //1未审核，2审核中（填写或修改），3审核通过
        name: '',
        mob: '',
        province: '',
        address2: ''
      }
    },
    methods: {
      addQua() {
        this.audit = 2
        console.log(this.audit)
      },
      cancel() {
        if(this.isPass) {
          this.audit = 3
        } else {
          this.audit = 1
        }
      },
      submit() {
        this.$message({
          message:'增票资质添加成功！',
          type: 'success'
        })
        this.audit = 3
        this.isPass = true
      },
      update() {
        this.audit = 2
      },
      del() {
        this.audit = 1
        this.audit2 = 1
        this.isPass = false
        this.isPass2 = false
      },
      set() {
        this.audit2 = 2
      },
      cancel2() {
        if(this.isPass2) {
          this.audit2 = 3
        } else {
          this.audit2 = 1
        }
      },
      submit2() {
        this.audit2 = 3
        this.isPass2 = true
      },
      update2() {
        this.audit2 = 2
      }
    }
  }
</script>

<style scoped lang="less">
  .top-img {
    width: 100%;
    padding: 20px 30px;
    box-sizing: border-box;
    img {
      width: 100%;
      height: 100%;
    }
  }
  .tip-box {
    width: calc(100% - 60px);
    height: 100px;
    display: flex;
    align-items: center;
    font-size: 14px;
    color: #303030;
    padding: 0 23px;
    box-sizing: border-box;
    background-color: #FFFAF4;
    margin: 0 auto;
    .add-btn {
      width: 133px;
      height: 37px;
      display: flex;
      align-items: center;
      justify-content: center;
      color: #F05511;
      border: 1px solid #F05511;
      margin-left: 40px;
      border-radius: 4px;
      cursor: pointer;
    }
    .upd-btn {
      width: 83px;
      height: 37px;
      display: flex;
      align-items: center;
      justify-content: center;
      color: #606060;
      border: 1px solid #CECECE;
      background-color: #fff;
      margin-left: 60px;
      border-radius: 4px;
      cursor: pointer;
    }
    .del-btn {
      width: 83px;
      height: 37px;
      display: flex;
      align-items: center;
      justify-content: center;
      color: #909090;
      border: 1px solid #CECECE;
      background-color: #fff;
      margin-left: 20px;
      border-radius: 4px;
      cursor: pointer;
    }
  }
  .submit-box {
    width: calc(100% - 60px);
    margin: 20px auto;
    .submit-title {
      display: flex;
      align-items: center;
      font-size: 16px;
      color: #333333;
      div {
        flex: 1;
        height: 1px;
        background-color: #000;
        opacity: .1;
        margin-top: 3px;
      }
    }
    .submit-item {
      width: 100%;
      display: flex;
      align-items: center;
      font-size: 14px;
      color: #666;
      margin: 20px 0;
      span {
        width: 100px;
      }
      .el-input {
        width: 400px;
      }
      .submit-btn {
        display: flex;
        align-items: center;
      }
    }
    p {
      margin: 20px 0;
      padding-left: 35px;
    }
    .set-line {
      margin: 20px 0;
      padding-left: 35px;
      display: flex;
      align-items: center;
      .set-btn {
        width: 83px;
        height: 37px;
        border: 1px solid #2778BE;
        display: flex;
        justify-content: center;
        align-items: center;
        font-size: 14px;
        color: #2778BE;
        margin-left: 10px;
        border-radius: 4px;
        cursor: pointer;
      }
    }
  }
</style>
