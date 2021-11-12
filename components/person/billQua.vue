<template>
  <!-- 增票资质 -->
  <div>
    <div class="top-img">
      <img :src="imgUrl">
    </div>
    <div class="tip-box" v-show="audit==1">
      <span>您的增票资质：</span>
      <span style="color: #909090;" v-show="!isPass">未添加</span>
      <!-- <span v-show="isPass">已添加</span> -->
      <p class="add-btn" v-show="!isPass" @click="addQua">添加增票资质</p>
      <!-- <p class="upd-btn" v-show="isPass" @click="update">修改</p>
      <p class="del-btn" v-show="isPass" @click="del">删除</p> -->
    </div>
    <div class="submit-box" v-show="audit==2">
      <div class="submit-title">
        <span>填写增票资质信息<span style="color: #E4393C;font-size: 14px;"> （所有信息均为必填）</span></span>
        <div></div>
      </div>
      <div class="submit-line">
        <div class="submit-item">
          <span>单位名称：</span>
          <el-input placeholder="请输入单位名称" v-model="info.nm" clearable></el-input>
        </div>
        <div class="submit-item">
          <span>纳税人识别码：</span>
          <el-input placeholder="请输入纳税人识别码" v-model="info.taxNum" clearable></el-input>
        </div>
        <div class="submit-item">
          <span>注册地址：</span>
          <el-input placeholder="请输入注册地址" v-model="info.address" clearable></el-input>
        </div>
        <div class="submit-item">
          <span>注册电话：</span>
          <el-input placeholder="请输入注册电话" v-model="info.tel" clearable></el-input>
        </div>
        <div class="submit-item">
          <span>开户银行：</span>
          <el-input placeholder="请输入开户银行" v-model="info.bank" clearable></el-input>
        </div>
        <div class="submit-item">
          <span>银行账户：</span>
          <el-input placeholder="请输入银行账户" v-model="info.account" clearable></el-input>
        </div>
      </div>
      <div class="submit-item">
        <span></span>
        <el-checkbox v-model="checked"><span style="color: #606060;">我已阅读并同意<span style="color: #E4393C;">《增票资质确认书》</span></span></el-checkbox>
      </div>
      <div class="submit-item">
        <span></span>
        <div class="submit-btn">
          <el-button type="primary" style="margin-right: 30px;" @click="submit">提交</el-button>
          <el-button plain @click="cancel">取消</el-button>
        </div>
      </div>
    </div>
    <div class="submit-box" v-if="audit==3&&isPass">
      <div class="submit-title">
        <span>增票资质信息</span>
        <div></div>
        <span style="color: #2778BE;margin-left: 20px;cursor: pointer;" @click="update">修改</span>
        <span style="color: #2778BE;margin-left: 20px;cursor: pointer;" @click="del">删除</span>
      </div>
      <div class="submit-line">
        <p>单位名称：{{info.nm}}</p>
        <p>纳税人识别码：{{info.taxNum}}</p>
        <p>注册地址：{{info.address}}</p>
        <p>注册电话：{{phone}}</p>
        <p>开户银行：{{info.bank}}</p>
        <p>银行账户：{{account}}</p>
      </div>
    </div>
    <div class="submit-box" v-show="audit==3&&isPass">
      <div class="submit-title">
        <span>增票收票地址</span>
        <div></div>
        <span style="color: #2778BE;margin-left: 20px;cursor: pointer;" v-show="audit2==3" @click="update2">修改</span>
        <span style="color: #2778BE;margin-left: 20px;cursor: pointer;" v-show="audit2==3" @click="del2">删除</span>
      </div>
      <div class="set-line" v-show="audit2==1">您还未设置收票地址：<div class="set-btn" @click="set()">立即设置</div></div>
      <div v-show="audit2==2" class="submit-line">
        <div class="submit-item">
          <span>收票人姓名：</span>
          <el-input placeholder="请输入收票人姓名" v-model="recInfo.linkman" clearable></el-input>
        </div>
        <div class="submit-item">
          <span>收票人手机号：</span>
          <el-input placeholder="请输入收票人手机号" v-model="recInfo.phone" clearable></el-input>
        </div>
        <div class="submit-item">
          <span>收票人地区：</span>
          <addr @changeAddr="changeAddr" ref="addrChoose" style="width: 300px;"></addr>
        </div>
        <div class="submit-item">
          <span>收票人地址：</span>
          <el-input placeholder="请输入收票人地址" v-model="recInfo.addrDetail" clearable></el-input>
        </div>
        <div class="submit-item">
          <span>收票人邮箱：</span>
          <el-input placeholder="请输入收票人邮箱" v-model="recInfo.email" clearable></el-input>
        </div>
      </div>
      <div v-show="audit2==2" class="submit-line">
        <div class="submit-item">
          <span></span>
          <div class="submit-btn">
            <el-button type="primary" style="margin-right: 30px;" @click="submit2">提交</el-button>
            <el-button plain @click="cancel2">取消</el-button>
          </div>
        </div>
      </div>
      <div v-if="audit2==3&&isPass2" class="submit-line">
        <p>收票人姓名： {{recInfo.linkman}}</p>
        <p>收票人手机号：{{phone2}}</p>
        <p>收票人地区：{{recInfo.addrNm}}</p>
        <p>收票人地址：{{recInfo.addrDetail}}</p>
        <p>收票人邮箱：{{recInfo.email}}</p>
      </div>
    </div>
  </div>
</template>

<script>
  import addr from "../addr.vue";
  import {
    mapState
  } from "vuex";
  export default {
    name: "billQua",
    props: {

    },
    data() {
      return {
        imgUrl: '',
        isPass:false,
        audit:1, //1未填写，2填写中（填写或修改），3填写完成
        info: {
          orgEnterId: '',
          nm: "", //名字
          taxNum: "", //税号
          tel: "", //电话号码
          address: "", //地址
          bank: "", //开户行
          account: "", //账户
        },
        recInfo: {
          orgEnterId: '', //机构id
          linkman: "", //联系人
          phone: "", //联系电话
          addrNm: "", //地址名称
          addrDetail: "", //发票寄送地址
          email: "", //电子邮箱，比如 example@qq.com
        },
        phone: '',
        account: '',
        id: '',
        phone2: '',
        checked: false,
        isPass2: false,
        audit2: 1, //1未填写，2填写中（填写或修改），3填写完成
        currentRole: ''
      }
    },
    mounted() {
      this.api.getAdert('billAd').then(res => {
        this.imgUrl=res[1].imgUrl
      })
      this.currentRole=JSON.parse(this.until.seGet('currentRole'))
      this.getInfo()
      this.$refs.addrChoose.getProvice()
    },
    computed: {
      ...mapState([
        'currentRole'
      ])
    },
    components: {
      addr
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
        this.info.orgEnterId = this.currentRole.id
        if(this.info.nm=='') {
          this.$message.error('请输入单位名称');
          return
        }
        if(this.info.taxNum=='') {
          this.$message.error('请输入纳税人识别码');
          return
        }
        if(this.info.address=='') {
          this.$message.error('请输入注册地址');
          return
        }
        if(this.reg.checkPhone(this.info.tel)!='ok') {
          this.$message.error(this.reg.checkPhone(this.info.tel));
          return
        }
        if(this.info.bank=='') {
          this.$message.error('请输入开户银行');
          return
        }
        if(this.info.account=='') {
          this.$message.error('请输入银行账户');
          return
        }
        if(!this.checked) {
          this.$message.error('请阅读并同意《增票资质确认书》');
          return
        }
        if(!this.isPass) {
          this.api.qualiAdd(this.info).then(res => {
            if(res.msg=='成功') {
              this.$message({
                message:'增票资质添加成功！',
                type: 'success'
              })
              this.audit = 3
              this.isPass = true
              this.getInfo()
            }
          })
        } else {
          this.api.qualiUpd(this.info).then(res => {
            if(res.msg=='成功') {
              this.$message({
                message:'增票资质修改成功！',
                type: 'success'
              })
              this.audit = 3
              this.getInfo()
            }
          })
        }
      },
      update() {
        this.audit = 2
      },
      del() {
        this.$confirm('确认删除增票资质信息?', '提示', {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning'
        }).then(() => {
          this.api.qualiDel({ids:this.id}).then(res => {
            if(res.msg=='成功') {
              this.$message({
                message:'增票资质删除成功！',
                type: 'success'
              })
              this.audit = 1
              this.audit2 = 1
              this.isPass = false
              this.isPass2 = false
            }
          })
        }).catch(() => {
          this.$message({
            type: 'info',
            message: '已取消删除'
          });
        });
      },
      del2() {
         this.$confirm('确认删除增票收票地址?', '提示', {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning'
        }).then((data) => {
          this.api.qualiAddrDel({ids:this.recInfo.id}).then(res => {
            if(res.msg=='成功') {
              this.$message({
                message:'增票收票地址删除成功！',
                type: 'success'
              })
              this.audit2 = 1
              this.isPass2 = false
            }
          })
        }).catch((err) => {
          this.$message({
            type: 'info',
            message: '已取消删除'
          });
        });
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
        this.recInfo.orgEnterId = this.currentRole.id
        if(this.recInfo.linkman=='') {
          this.$message.error('请输入收票人姓名');
          return
        }
        if(this.reg.checkPhone(this.recInfo.phone)!='ok') {
          this.$message.error(this.reg.checkPhone(this.recInfo.phone));
          return
        }
        if(this.recInfo.addrNm=='') {
          this.$message.error('请选择收票人地区');
          return
        }
        if(this.recInfo.addrDetail=='') {
          this.$message.error('请输入收票人地址');
          return
        }
        if(this.reg.checkMail(this.recInfo.email)!='ok') {
          this.$message.error(this.reg.checkMail(this.recInfo.email));
          return
        }
        if(!this.isPass2) {
          this.api.qualiAddrAdd(this.recInfo).then(res => {
            if(res.msg=='成功') {
              this.$message({
                message:'收票地址设置成功！',
                type: 'success'
              })
              this.audit2 = 3
              this.isPass2 = true
              this.getRecInfo()
            }
          })
        } else {
          this.api.qualiAddrUpd(this.recInfo).then(res => {
            if(res.msg=='成功') {
              this.$message({
                message:'收票地址修改成功！',
                type: 'success'
              })
              this.audit2 = 3
            }
            this.getRecInfo()
          })
        }
      },
      update2() {
        this.audit2 = 2
      },
      //获取地区信息
      changeAddr(info){
          let data = JSON.parse(info)
          this.recInfo.addrNm = data.province+'-'+data.city+'-'+data.region
      },
      getInfo() {
        let param={
          orgEnterId:this.currentRole.id
        }
        this.api.getQualiInfo(param).then(res => {
          if(res.data) {
            this.audit=3
            this.isPass = true
            this.info = res.data
            this.id = res.data.id
            this.phone = this.info.tel
            this.account = this.info.account
            this.getRecInfo()
          } else {
            this.audit=1
          }
        })
      },
      getRecInfo() {
        let param={
          orgEnterId:this.currentRole.id
        }
        this.api.getQualiAddrInfo(param).then(res => {
          if(res.data.list.length>0) {
            this.audit2 = 3
            this.isPass2 = true
            this.recInfo = res.data.list[0]
            // this.phone2 = this.recInfo.phone.replace(/.(?=.{4})/g, '*')
            this.phone2 = this.recInfo.phone
            this.$refs.addrChoose.getProvice(this.recInfo.addrNm)
          } else {
            this.audit2 = 1
          }
        })
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
      margin-bottom: 30px;
      div {
        flex: 1;
        height: 1px;
        background-color: #000;
        opacity: .1;
        margin-top: 3px;
      }
    }
    .submit-line {
      display: flex;
      flex-wrap: wrap;
      justify-content: space-between;
      p {
        width: 50%;
        margin: 20px 0;
        padding-left: 35px;
        box-sizing: border-box;
      }
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
      .el-input {
        width: 300px;
      }
      .submit-btn {
        display: flex;
        align-items: center;
        margin-top: 20px;
      }
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
