<template>
  <div class="left1">
    <!--产品订单-->
    <div class="productOrder">
      <div class="title">
        <p><img src="@/assets/img/personal/首页/产品订单.png" alt=""/>产品订单</p>
      </div>
      <div class="content">
        <div class="section">
          <div class="partA" @click="toPage('/personal/myOrder1?cdType=1&statusCd=1')">
            <img src="@/assets/img/personal/首页/待付款.png" alt=""/>
            <span v-if="number != 0" class="span">{{number}}</span>
          </div>
          <p>待付款</p>
        </div>
        <div class="section">
          <div class="partA"  @click="toPage('/personal/myOrder1?cdType=1&statusCd=2')">
            <img src="@/assets/img/personal/首页/待发货.png" alt=""/>
            <span v-if="number != 0" class="span">{{number}}</span>
          </div>
          <p>待发货</p>
        </div>
        <div class="section">
          <div class="partA" @click="toPage('/personal/myOrder1?cdType=1&statusCd=3')">
            <img src="@/assets/img/personal/首页/待收货.png" alt=""/>
            <span v-if="number != 0"  class="span">{{number}}</span>
          </div>
          <p>待收货</p>
        </div>
        <div class="section">
          <div class="partA" @click="toPage('/personal/myOrder1?cdType=1&statusCd=4')">
            <img src="@/assets/img/personal/首页/已完成.png" alt=""/>
            <span v-if="number != 0"  class="span">{{number}}</span>
          </div>
          <p>已完成</p>
        </div>
        <div class="section">
          <div class="partA"  @click="toPage('/personal/myOrder1?cdType=1')">
            <img src="@/assets/img/personal/首页/全部订单.png" alt=""/>
            <span v-if="number != 0"  class="span">{{number}}</span>
          </div>
          <p>全部订单</p>
        </div>
      </div>
    </div>
    <!--船舶订单-->
    <div class="productOrder">
      <div class="title">
        <p><img src="@/assets/img/personal/首页/船舶订单.png" alt=""/>船舶订单</p>
      </div>
      <div class="content">
        <div class="section">
          <div class="partA"  @click="toPage('/personal/myOrder1?cdType=2&statusCd=1')">
            <img src="@/assets/img/personal/首页/待付款.png" alt=""/>
            <span v-if="number != 0"  class="span">{{number}}</span>
          </div>
          <p>待付款</p>
        </div>
        <div class="section">
          <div class="partA"  @click="toPage('/personal/myOrder1?cdType=2&statusCd=2')">
            <img src="@/assets/img/personal/首页/待发货.png" alt=""/>
            <span v-if="number != 0" class="span">{{number}}</span>
          </div>
          <p>待发货</p>
        </div>
        <div class="section">
          <div class="partA"  @click="toPage('/personal/myOrder1?cdType=2&statusCd=3')">
            <img src="@/assets/img/personal/首页/待收货.png" alt=""/>
            <span v-if="number != 0" class="span">{{number}}</span>
          </div>
          <p>待收货</p>
        </div>
        <div class="section">
          <div class="partA"  @click="toPage('/personal/myOrder1?cdType=2&statusCd=4')">
            <img src="@/assets/img/personal/首页/已完成.png" alt=""/>
            <span v-if="number != 0" class="span">{{number}}</span>
          </div>
          <p>已完成</p>
        </div>
        <div class="section">
          <div class="partA"  @click="toPage('/personal/myOrder1?cdType=2')">
            <img src="@/assets/img/personal/首页/全部订单.png" alt=""/>
            <span v-if="number != 0"  class="span">{{number}}</span>
          </div>
          <p>全部订单</p>
        </div>
      </div>
    </div>
    <!--在线留言-->
    <div class="message1" >
      <div class="title">
        <p><img src="@/assets/img/personal/首页/留言.png" alt=""/>在线留言</p>
      </div>
      <div class="content1">
        <el-form ref="ruleForm" :model="ruleForm" label-width="100px" :rules="rules" :label-position="labelPosition">
          <el-form-item label="留言内容：" prop="cont">
            <el-input placeholder="可在线提交您的宝贵意见，我们会尽力做到更好。" type="textarea" row="3" id="resize" v-model="ruleForm.cont"></el-input>
          </el-form-item>
          <el-form-item label="联系人：" prop="linkman">
            <el-input placeholder="请输入联系人" v-model="ruleForm.linkman"></el-input>
          </el-form-item>
          <el-form-item label="联系电话：" prop="phone">
            <el-input placeholder="请输入联系电话"  v-model="ruleForm.phone"></el-input>
          </el-form-item>
          <el-form-item label="联系邮箱：" prop="email">
            <el-input placeholder="请输入联系邮箱" v-model="ruleForm.email"></el-input>
          </el-form-item>
          <el-form-item id="btnBA">
            <el-button id="buttonB" @click="resetForm('ruleForm')">取消</el-button>
            <el-button id="buttonA" type="primary" @click="submitForm('ruleForm')">提交</el-button>
          </el-form-item>
        </el-form>
      </div>
    </div>
  </div>
</template>

<script>
  import {mapState} from "vuex";
  export default {
    name: "shipowner",
    components: {},
    data(){
      var checkTel = (rule, value, callback) => {
        let regPhone = /^(((13[0-9]{1})|(14[0-9]{1})||(15[0-9]{1})|17[0-9]{1}|(18[0-9]{1}))+\d{8})$/;
        if (!value) {
          return callback(new Error('手机号码不能为空！'));
        }else {
          setTimeout(() => {
            if (!regPhone.test(value)) {
              return callback(new Error('请填写正确的手机号！'));
            }else {
              return callback()
            }
          }, 1000);
        }
      };
      return {
        number: 0,
        labelPosition: 'left',
        ruleForm: {
          cont: '',
          linkman: '',
          phone: '',
          email: '',
        },
        rules: {
          linkman: [
            {required: true, message: '请输入联系人', trigger: 'blur'},
          ],
          phone: [
            {required: true, validator: checkTel, trigger: 'blur'},
          ],
          email: [
            {required: true, message: '请输入联系邮箱', trigger: 'blur'},
          ],
          cont: [
            {required: true, message: '请填写留言内容', trigger: 'blur'}
          ]
        },

      }

    },
    computed:{
      ...mapState([
        'loading',
      ])
    },
    watch:{
    },
    mounted(){

    },
    methods:{
      toPage(url){
        if(url){
          this.$router.push(url)
        }
      },
      resetForm(formName) {
        this.$refs[formName].resetFields();
      },
      submitForm(formName){
        this.$refs[formName].validate((valid) => {
          console.log(valid)
          if (valid) {
            this.store.commit('changeLoading',true)

            this.api.msgAdd(this.ruleForm).then(res=>{
              console.log(res)
              this.$message({
                message: '留言成功',
                type: 'success',
                duration:'1500'
              });
              setTimeout(()=>{
                this.toPage('./')
              },1500)
            })
          } else {
            console.log('error submit!!');
            return false;
          }
        });

      },
    },
  }
</script>

<style lang="less">

    #resize{
      resize: none !important;
      height: 92px;
    }
    #btnBA{
      display: inline-flex;
      width: 100%;
      .el-form-item__content{
        display: inline-flex;
        justify-content: center;
        width: 100%;
      }
      #buttonA{
        width: 46%;
        height: 42px;
        border: 1px solid #2778BE;
        color: #FFFFFF;
        font-size: 16px;
        background-color: #2778BE;
        cursor: pointer;
        margin-left: 30px;
      }
      #buttonB{
        width: 46%;
        height: 42px;
        border: 1px solid #DDDDDD;
        color: #666666;
        font-size: 16px;
        background-color: #FFFFFF;
        cursor: pointer;
      }
    }

</style>
<style lang="less" scoped>
  @import url("../../assets/css/init.less");
  .left1 {
    width: 67.3%;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    padding-top: 31px;
    //产品订单
    .productOrder {
      width: 100%;
      /*height: 200px;*/
      background-color: #FFFFFF;
      margin-bottom: 24px;
      .title {
        width: 100%;
        height: 56px;
        border-bottom: 1px solid #F3F3F3;
        > p {
          font-size: 16px;
          color: #333333;
          line-height: 56px;
          margin-left: 20px;

        }
        img {
          padding-right: 10px;
          height: 19px;
          vertical-align: middle;
        }
      }
      //内容
      .content {
        width: 100%;
        display: inline-flex;
        .section {
          flex: 1;
          display: flex;
          flex-direction: column;
          justify-content: center;
          align-items: center;
          height: 162px;
          .partA {
            position: relative;
            .span {
              position: absolute;
              padding: 0 3px;
              height: 14px;
              background-color: #E4393C;
              border-radius: 8px;
              left: 26px;
              color: #FFFFFF;
              font-size: 12px;
              text-align: center;
              line-height: 14px;
            }
            > img {
              cursor: pointer;
              width: 35px;
              padding-bottom: 10px;
            }
          }
          > p {
            font-size: 12px;
            color: #333333;
            cursor: pointer;
          }
        }
      }
    }

    //在线留言板块
    .message1 {
      width: 100%;
      background-color: #FFFFFF;
      .title {
        width: 100%;
        height: 56px;
        border-bottom: 1px solid #F3F3F3;
        .more {
          float: right;
          font-size: 12px;
          color: #666666;
          margin-right: 30px;
          cursor: pointer;

        }
        > p {
          font-size: 16px;
          color: #333333;
          line-height: 56px;
          margin-left: 20px;
        }
        img {
          padding-right: 10px;
          height: 19px;
          vertical-align: sub;
        }
      }
      .content1 {
        height: 100%;
        padding: 20px 110px 30px 80px;
      }
    }

  }


</style>
