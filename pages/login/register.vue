<template>
<!--注册页面-->
  <div id="home" :style="{width:bWidth + 'px'}">
    <!--头部-->
    <div class="header1">
      <div class="content" :style="{width:width + 'px'}">
        <img :src="logo" alt="" @click="toPage('/')"/>
        <p v-if="this.ifLogin == false" @click="toPage('login')">已有账号？<span class="textRed">请登录<i class="el-icon-arrow-right"></i></span></p>
      </div>
    </div>
    <div class="header">
      <img src="@/assets/img/personal/左.png" alt=""/>
      <p>免费注册</p>
      <img src="@/assets/img/personal/右.png" alt=""/>
    </div>
    <div class="body main" :style="{width:width + 'px'}">

      <div id="register1"  class="main">
        <div class="process">
          <div v-for="(item,index) in prcessList" :key="index" :class="{acitve:currentIndex>=index}">
            <img :src="currentIndex>=index ? arrowa : arrow" v-if="index>0"/>
            <p>
              <img :src="currentIndex>=index ? item.imgA : item.img"/>
              {{index+1+'.'+item.nm}}
            </p>
          </div>
        </div>
        <step-one :type="roleCd" @setInfo="setInfo" :info="ruleForm" v-show="currentIndex==0"></step-one>
        <step-two :type="roleCd" @setInfo="setInfo" :info="ruleForm" v-show="currentIndex==1 || currentIndex==2"></step-two>
      </div>

    </div>
    <!--底部图片-->
    <div class="footerImg" :style="{width:bWidth+'px'}">
      <img src="../../assets/img/footer.png" alt=""/>
    </div>
  </div>
</template>

<script>

import addr from '@/components/addr'
import {mapState} from "vuex";
import Step1 from "../settleIn/component/step1";
import process1 from './component/img/process1.png'
import process1a from './component/img/process1a.png'
import process2 from './component/img/process2.png'
import process2a from './component/img/process2a.png'
import process4 from './component/img/process4.png'
import process4a from './component/img/process4a.png'
import arrow from './component/img/arrow.png'
import arrowa from './component/img/arrowa.png'
import StepOne from "./component/stepOne";
import StepTwo from "./component/stepTwo";
import logo from '@/assets/img/logo.png'
export default {
  head () {
    return {
      title: '注册',
    }
  },
  data(){
    return{
      // logo:'https://sinovat.oss-cn-shanghai.aliyuncs.com/0875a132ec924b17b73aae3d9c644a81_logo.png',//聚联logo
      logo:logo,
      time:0,
      roleCd:'',
      arrow,
      arrowa,
      currentIndex:0,
      prcessList:[
        {
          nm:'验证手机号',
          img:process1,
          imgA:process1a
        },{
          nm:'填写账号信息',
          img:process2,
          imgA:process2a
        },{
          nm:'注册成功',
          img:process4,
          imgA:process4a
        }
      ],
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
        referrerId:'',
        inviCd:'',
      },

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
  components: {
    StepTwo,
    StepOne,
    Step1,
    addr
  },
  mounted(){
     if(this.until.getQueryString('inviCd')){
       this.ruleForm.inviCd = this.until.getQueryString('inviCd')
     }else if(this.until.seGet('inviCd')){
       this.ruleForm.inviCd = this.until.seGet('inviCd')
     }

  },
  methods:{
    //提交
    submitForm(formName) {
      console.log('提交')
      if(this.ruleForm.password!=this.ruleForm.comfirmPassword){
        this.$message({
          message: '两次密码不一致',
          type: 'warning'
        });
        return
      }
      console.log(this.$refs[formName])
      this.$refs[formName].validate((valid) => {
        console.log(valid)
        if(!this.agree){
          this.$message({
            message: '请阅读并同意《中创海洋用户免责声明》',
            type: 'warning'
          });
          return
        }
        if (valid) {
          console.log('==============')
          this.store.commit('changeLoading',true)
          this.api.register(this.ruleForm).then(res=>{
            this.$message({
              message: '注册成功，请登录',
              type: 'success',
              duration:'1500'
            });
            setTimeout(()=>{
              this.toPage('./login')
            },1500)
          })
        } else {
          console.log('error submit!!');
          return false;
        }
      });
    },
    //上一步、下一步
    setInfo(data){
      this.currentIndex = data.nextIndex
      this.ruleForm = data.info
    },
    //取消
    resetForm(formName) {
      this.$refs[formName].resetFields();
    },
    //页面跳转
    toPage(url){
      this.$router.push(url)
    }
  }
}
</script>
<style lang="less">
#register1{
  input{
    width: 358px;
    height: 42px;
    border-radius: 0;
  }
  #input2{
    width: 100%;
    display: table-caption;
    /*input{*/
    /*  height: 42px;*/
    /*  width: 107px;*/
    /*}*/
  }
}
</style>
<style lang="less" scoped>
  @import url("../../assets/css/init.less");
 #register1{
   width: 100%;
   display: flex;
   flex-wrap: wrap;
   align-items: center;
   justify-content: center;
   .process{
     width: 100%;
     background: #F6F6F6;
     border-bottom: 1px solid #DFDFDF;
     display: flex;
     display: -webkit-flex;
     align-items: center;
     justify-content: center;
     padding: 23px 0;
     margin-bottom: 60px;
     >div{
       display: flex;
       display: -webkit-flex;
       align-items: center;
       >img{
         width: 7px;
         margin: 0 40px;
       }
       p{
         display: flex;
         display: -webkit-flex;
         align-items: center;
         img{
           width: 40px;
           margin-right: 13px;
         }
       }
     }
     .acitve{
       color: @themeColor;
     }
   }

 }
  #home{
    background-color: #FFFFFF;

  }
  .header1{
    box-shadow: 0px 6px 10px 0px rgba(13, 4, 8, 0.1);
    width: 100%;
    display: flex;
    .content{
      display: inline-flex;
      align-items: center;
      justify-content: space-between;
      height: 100px;
      margin: auto;
      img{
        width: 450px;
      }
      >p{
        font-size: 16px;
        cursor: pointer;
        color: #333333;
      }
      .textRed{
        color: #FF3E3E;
      }
    }
  }
  .header{
    display: inline-flex;
    height: 120px;
    align-items: center;
    justify-content: center;
    width: 100%;
    >p{
      font-size: 24px;
      color: #333333;
      margin: 0 30px;
      /*font-weight: bold;*/
    }
    img{
      width: 148px;
    }
  }
  .body{
    border: 1px solid #DFDFDF;
    display: flex;
    align-items: center;
    justify-content: center;
    position: relative;
    .button2{
      width: 358px;
      height: 52px;
      border-radius: 0;
      background-color: @themeColor;
      font-size: 16px;
      color: #FFFFFF;
    }
    /*.title{*/
    /*  width: 100%;*/
    /*  height: 100px;*/
    /*  display: inline-flex;*/
    /*  margin-bottom: 16px;*/
    /*  p{*/
    /*    font-size: 20px;*/
    /*    color: #333333;*/
    /*    font-weight: bold;*/
    /*    display: flex;*/
    /*    align-items: center;*/
    /*    .lineC{*/
    /*      width: 4px;*/
    /*      height: 17px;*/
    /*      background-color: #E4393C;*/
    /*      display: block;*/
    /*      margin-right: 14px;*/
    /*    }*/
    /*  }*/
    /*}*/
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
  .footerImg{
    width: 100%;
    margin-top: 53px;
  }
</style>
