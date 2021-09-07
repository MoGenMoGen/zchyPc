<template>
  <!--登录页面-->
  <div id="home" :style="{width:bWidth + 'px'}">
    <!--首部标题-->
    <div class="header" :style="{width:width + 'px'}">
      <div class="title">
        <img :src="logo" alt="" @click="toPage('../')"/>
        <p>欢迎登录</p>
      </div>
    </div>
    <div class="body">
      <div id="register">
        <!--上部标题-->
        <div class="title1">
          <p>账号登录</p>
          <p @click="toPage('./register')">免费注册<i class="el-icon-arrow-right"></i></p>
        </div>
        <!--下部表单-->
        <el-form :model="ruleForm" :rules="rules" ref="ruleForm" class="demo-ruleForm">
          <el-form-item  prop="username" @keyup.enter="submitForm('ruleForm')">
            <el-input class="input1" placeholder="请输入手机号码或用户名" v-model="ruleForm.username" type="text" prefix-icon="el-icon-user"></el-input>
          </el-form-item>
          <el-form-item prop="password" @keyup.enter="submitForm('ruleForm')">
            <el-input class="input1" placeholder="请输入密码" maxlength="24" v-model="ruleForm.password" type="password" prefix-icon="el-icon-lock"></el-input>
            <p class="blue" @click="toPage('./changePWD')">忘记密码</p>
          </el-form-item>
          <el-form-item>
            <el-button class="button1" type="primary" @click="submitForm('ruleForm')" @keyup.enter="submitForm('ruleForm')">登录</el-button>
          </el-form-item>
        </el-form>
      </div>
    </div>
    <div class="footer" :style="{width:bWidth+'px'}">
        <p>Copyright © 国家渔业装备科技创新版权所有    <a href="" target="_blank" class="cursor">浙ICP备20025027号-7</a> </p>
        <p>聚联科技有限公司技术支持</p>
    </div>
  </div>
</template>

<script>
  import {mapState} from "vuex";
  import logo from '@/assets/img/logo.png'
  export default {
    head () {
      return {
        title: '登录',
      }
    },
    data(){

      return{
        // logo:'https://sinovat.oss-cn-shanghai.aliyuncs.com/0875a132ec924b17b73aae3d9c644a81_logo.png',//聚联logo
        logo:logo,
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
        }
      }
    },
    computed: {
      ...mapState([
        'bWidth',
        'width',
        'loading'
      ])
    },
    components: {
    },
    mounted(){
      this.enterKeyup();
    },
    destroyed() {
      // 销毁enter事件
      this.enterKeyupDestroyed();
    },
    methods:{

      //提交
      submitForm(formName) {
        this.$refs[formName].validate((valid) => {
          console.log(valid)
          if (valid) {
            console.log('==============')
            this.store.commit('changeLoading',true)
            this.api.login(this.ruleForm).then(res=>{
              console.log(res)
              this.store.commit('token',res.token)
              this.store.commit('userInfo',res.userInfo)
              this.until.seSave('token',res.token)
              this.until.seSave('userInfo',JSON.stringify(res.userInfo))
              //获取上次登录最后的角色
              this.api.toGet('currentInfo').then(data=>{
                if(data){
                  this.store.dispatch('save',data)
                }
              })

              this.$message({
                message: '登录成功',
                type: 'success',
                duration:'1500'
              });
              setTimeout(()=>{
                this.toPage('../')
              },1500)
            })
          } else {
            console.log('error submit!!');
            return false;
          }
        });
      },
      //页面跳转
      toPage(url){
        this.$router.push(url)
      },
      enterKey(event) {
          const code = event.keyCode
            ? event.keyCode
            : event.which
              ? event.which
              : event.charCode;
          if (code == 13) {
            this.submitForm('ruleForm');
          }
      },
      enterKeyupDestroyed() {
        document.removeEventListener("keyup", this.enterKey);
      },
      enterKeyup() {
        document.addEventListener("keyup", this.enterKey);
      },
    }
  }
</script>
<style lang="less">
  #register{
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
  #register{
    width: 307px;
    height: 352px;
    background-color: #FFFFFF;
    margin-left: 65%;
    position: relative;
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
  #home{
    background-color: #FFFFFF;
    .header{
      display: flex;
      display: -webkit-flex;
      height: 101px;
      justify-content: center;
      margin: 0 auto;
      .title{
        width: 100%;
        display: inline-flex;
        align-items: center;
        img{
          cursor: pointer;
          width: 450px;
        }
        >p{
          color: #404040;
          font-size: 30px;
          font-weight: normal;
          line-height: 101px;
          padding-left: 25px;
        }
      }
    }
    .body{
      height: 550px;
      display: flex;
      background-image: url("../../assets/img/登录bg.png");
      background-size: cover;
      background-repeat: no-repeat no-repeat;
      background-position: center;
      width: 100%;
      align-items: center;
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
    }
    .footer{
      width: 100%;
      text-align: center;
      font-size: 14px;
      color: #999999;
      padding: 10px 0;
      line-height: 28px;
    }
  }
</style>
