<template>
<div class="processStep">
  <el-form :model="ruleForm" :rules="rules" ref="ruleForm" label-width="100px" class="demo-ruleForm" @submit.native.prevent>
              <el-form-item label="真实姓名：" prop="realNm">
                <el-input placeholder="请输入真实姓名" v-model="ruleForm.realNm" type="text"></el-input>
              </el-form-item>
              <el-form-item label="公司名称：" prop="company">
                <el-input placeholder="请输入公司名称" v-model="ruleForm.company" type="text"></el-input>
              </el-form-item>
              <el-form-item label="所属地区：" id="input2">
                <addr @changeAddr="changeAddr" ref="addrC"></addr>
              </el-form-item>
              <el-form-item label="详细地址：" prop="address">
                <el-input placeholder="请输入详细地址" v-model="ruleForm.address" type="text"></el-input>
              </el-form-item>
              <el-form-item label=" 推荐码：" prop="inviCd">
                <el-input v-model="ruleForm.inviCd" placeholder="请输入推荐码"></el-input>
<!--                <p>请输入推荐码；如果没有推荐码，则不用填写</p>-->
              </el-form-item>
              <el-form-item id="bt">
                <!--            <el-button class="button2" type="primary" @click="submitForm('ruleForm')">确认提交</el-button>-->
                <el-button id="button1" @click="nextStep()">上一步</el-button>
                <el-button id="button2" type="primary" @click="submitForm('ruleForm')">提交</el-button>
              </el-form-item>
  </el-form>

</div>
</template>

<script>
    import Addr from "../../../components/addr";
    import {mapState} from "vuex";
    export default {
      props:['info'],
      name: "stepTwo",
      components: {Addr},
      data(){
          return{
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
            rules: {
              company: [
                { required: true, message: '请输入公司名称', trigger: 'blur' }
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
        this.$refs.addrC.getProvice()
      },
      methods:{
        //获取地区信息
        changeAddr(info){
          let data = JSON.parse(info)
          this.ruleForm.addrNm = data.province+'-'+data.city+'-'+data.region
          this.ruleForm.addrCd = data.provinceId+'-'+data.cityId+'-'+data.regionId
        },
        nextStep(){
          let data = {}
          data.info = this.ruleForm  //当前步骤的信息
          data.nextIndex = 0 //下一步
          this.$emit('setInfo',data)
        },
        //提交
        submitForm(formName) {
          this.$refs[formName].validate((valid) => {
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
        toPage(url){
          this.$router.push(url)
        }
      }
    }
</script>

<style lang="less">
    #input2{
      width: 100%;
      display: table-caption;
      input{
        height: 42px;
        width: 107px;
      }
    }
  #addr{
    width: 358px;
    input{
      width: 100%;
    }
  }
    #register1 #input2 input{
      width: 100%;
    }
  #bt{
    display: flex;
    flex: 1;
    #button1{
      width: 107px;
      height: 52px;
      border-radius: 0;
    }
    #button2{
      width: 238px;
      height: 52px;
      border-radius: 0;

    }
  }
</style>
<style lang="less" scoped>
  @import url("../../../assets/css/init.less");
  .processStep{
    width: 100%;
    .demo-ruleForm{
      width: 460px;
      margin: 0 auto;
    }
    span{
      cursor: pointer;
    }

    //.button1{
         //  width: 107px;
         //  height: 52px;
       //  }
    //.button2{
   //   width: 240px;
    //  height: 52px;
   //   border-radius: 0;
   //   background-color: @themeColor;
    //  font-size: 16px;
   //   color: #FFFFFF;
  //    display: flex;
    //  align-items: center;
   //   justify-content: center;
  //  }

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
