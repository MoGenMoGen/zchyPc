<template>
<div class="processStep">
  <!--弹窗-->
  <div class="dialog" v-show="centerDialogVisible">
    <div class="dialogMain">
      <div class="dialogTitle">
        中创海洋用户免责声明
        <i class="el-icon-circle-close" @click="centerDialogVisible=false"></i>
      </div>
      <el-scrollbar class="content">
        <div v-html="cont"></div>
      </el-scrollbar>
    </div>
  </div>
  <el-form :model="ruleForm" :rules="rules" ref="ruleForm" label-width="100px" class="demo-ruleForm" @submit.native.prevent>
    <el-form-item label="用户名：" prop="username">
      <el-input class="input1" placeholder="请输入用户名" v-model="ruleForm.username" type="text"></el-input>
    </el-form-item>
    <el-form-item label="手机号码：" prop="mob">
      <el-input placeholder="请输入手机号码" v-model="ruleForm.mob" type="tel"></el-input>
    </el-form-item>
    <el-form-item label="验证码：" prop="vcode">
      <el-input placeholder="请输入验证码" v-model="ruleForm.vcode" type="number"></el-input>
      <button class="button1" v-if="!time" @click.stop="getCode">获取验证码</button>
      <button class="button1" v-else>{{time}}S后获取</button>
    </el-form-item>
    <el-form-item label="设置密码：" prop="password">
      <el-input placeholder="请输入密码" v-model="ruleForm.password" type="password"></el-input>
    </el-form-item>
    <el-form-item label="重复密码：" prop="comfirmPassword">
      <el-input placeholder="请再次输入密码" v-model="ruleForm.comfirmPassword" type="password"></el-input>
    </el-form-item>
    <el-form-item>
      <div class="agree">
        <i class="iconfont" :class="{'icon-fangkuang':!agree,'icon-xuanzhong':agree}" @click="agree=!agree"></i> 我已阅读并同意<span @click="centerDialogVisible=true">《中创海洋用户免责声明》</span>
      </div>
    </el-form-item>
    <el-form-item>
      <!--            <el-button class="button2" type="primary" @click="submitForm('ruleForm')">确认提交</el-button>-->
      <el-button class="button2" type="primary" @click="submitForm('ruleForm')">下一步</el-button>
    </el-form-item>
  </el-form>

</div>
</template>

<script>
  import {mapState} from "vuex";
  export default {
    props:['info'],
    name: "stepOne",
        data(){
          //手机号验证
          var checkTel = (rule, value, callback) => {
            let regPhone = /^(((13[0-9]{1})|(14[0-9]{1})||(15[0-9]{1})|17[0-9]{1}|(18[0-9]{1}))+\d{8})$/;
            if (!value) {
              return callback(new Error('手机号码不能为空！'));
            }else {
              setTimeout(() => {
                if (!regPhone.test(value)) {
                  return callback(new Error('请填写正确的手机号！'));
                }else {
                  this.api.checkMob(value).then(res=>{
                    console.log(res)
                    return callback()

                  }).catch(err=>{
                    console.log(err)
                    return callback(new Error(err));
                  })
                }
              }, 1000);
            }

          };
          //用户名验证
          var checkUser  = (rule, value, callback) => {
            if (!value) {
              return callback(new Error('用户名不能为空！'));
            }else if(value.length<2 || value.length > 20){
              return callback(new Error('请输入长度为2-20个的用户名'));
            }else {
              this.api.checkUser(value).then(res=>{
                console.log(res)
                return callback()
              }).catch(err=>{
                console.log(err)
                return callback(new Error(err));
              })
            }

          };
             return{
               cont:'',
               ruleForm:{
                 username:'',
                 mob:'',
                 vcode:'',
                 password:'',
                 comfirmPassword:'',
                 realNm: "",
                 company: "",
                 addrCd: "",
                 addrNm: "",
                 address: "",
                 referrerId:''
               },
               labelPosition: 'left',
               centerDialogVisible:false,
               agree:false,
               time:0,
               rules: {
                 username: [
                   { required: true, message: '请输入用户名', trigger: 'blur' },
                   { min: 2, max: 20, validator: checkUser, trigger: 'blur' }
                 ],
                 mob: [
                   { required: true, validator: checkTel, trigger: 'blur' },
                 ],
                 vcode: [
                   { required: true, message: '请输入验证码', trigger: 'blur' },
                 ],
                 password: [
                   { required: true,message: '请输入密码', trigger: 'blur' },
                 ],
                 comfirmPassword: [
                   { required: true,message: '请输入确认密码', trigger: 'blur' },
                 ],
               }
             }
        },
    computed: {
      ...mapState([
        'bWidth',
        'width',
        'ifLogin',
        'loading'
      ])
    },
    watch:{
      info:{
        handler:function () {
          this.ruleForm = this.info
        },
        immediate:true
      },
    },
    mounted(){
      this.getCont()
    },
      methods:{
        getCont(){ //声明
          this.api.ruleDetail('5028235019260928').then(res=>{
            this.cont = res.cont
          })
        },
        getCode(){
          // return
          if(this.reg.checkPhone(this.ruleForm.mob)!='ok'){
            this.$message({
              message: this.reg.checkPhone(this.ruleForm.mob),
              type: 'warning'
            });
            return
          }
          this.api.getCode(this.ruleForm.mob).then(()=>{
            this.$message({
              message: '验证码发送成功',
              type: 'success',
            });
          })
          this.time = 60
          let timer = setInterval(()=>{
            if(this.time){
              this.time--
            }else {
              clearInterval(timer)
            }
          },1000)
        },
        //页面跳转
        toPage(url){
          this.$router.push(url)
        },
        //下一步
        submitForm(formName) {
          if(!this.agree){
            this.$message({
              message: '请阅读并同意《中创海洋用户免责声明》',
              type: 'warning'
            });
            return
          }
          this.$refs[formName].validate((valid) => {
            if (valid) {
              // alert('submit!');
              let data = {}
              data.info = this.ruleForm  //当前步骤的信息
              data.nextIndex = 1 //下一步
              this.$emit('setInfo',data)
            } else {
              console.log('error submit!!');
              return false;
            }
          });
        },
      },
    }
</script>

<style lang="less" scoped>
  @import url("../../../assets/css/init.less");
  .processStep{
    width: 100%;
    .dialog{
      z-index: 999;
      position: fixed;
      left: 0;
      top:0;
      background: rgba(0,0,0,0.7);
      height: 100%;
      width: 100%;
      display: flex;
      display: -webkit-flex;
      .dialogMain{
        background: #ffffff;
        box-sizing: border-box;
        width: 80%;
        height: 80%;
        margin: auto;
        border-radius: 10px;
        overflow-x: hidden;
        display: flex;
        display: -webkit-flex;
        flex-direction: column;
        .dialogTitle{
          width: 100%;
          position: relative;
          height: 80px;
          border-bottom: 1px solid @borderColor;
          line-height: 80px;
          text-align: center;
          font-size: 18px;
          font-weight: bold;
          i{
            position: absolute;
            right: 10px;
            top:50%;
            transform: translateY(-50%);
            font-size: 28px;
            color: #999999;
            cursor: pointer;
          }
        }
        .content{
          flex: 1;
          margin: 20px 5px 20px 20px;
          overflow-y: auto;
          img{
            cursor: pointer;
          }
        }
      }
    }
    .demo-ruleForm{
      margin: 0 auto;
      width: 460px;
    }
    span{
      cursor: pointer;
    }

    .button1{
      height: 42px;
      width: 107px;
      background-color: @themeColor;
      border: none;
      color: #FFFFFF;
      position: absolute;
      right: 0;
      top: 0;
    }
    .button2{
      width: 358px;
      height: 42px;
      border-radius: 0;
      background-color: @themeColor;
      font-size: 16px;
      color: #FFFFFF;
      display: flex;
      align-items: center;
      justify-content: center;
    }

    .agree{
      font-size: 12px;
      display: flex;
      display: -webkit-flex;
      align-items: center;
      span{
        color: @themeColor;
        cursor: pointer;
      }
      i{
        margin-right: 5px;
        font-size: 12px;
        cursor: pointer;
      }
      .icon-xuanzhong{
        color: @themeColor;
      }
    }
  }
</style>
