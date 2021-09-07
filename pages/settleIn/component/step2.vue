<template>
  <div class="processStep">
    <div class="title">
      <span></span>入驻基本信息
    </div>
    <el-form :model="ruleForm" :rules="rules" ref="ruleForm" label-width="100px" class="demo-ruleForm">
      <el-form-item label="机构名称：" prop="company" v-if="ruleForm.identityCd=='identity100'">
        <el-input v-model="ruleForm.company" placeholder="请输入机构名称"></el-input>
      </el-form-item>
      <el-form-item label="个人姓名：" prop="company" v-else-if="ruleForm.identityCd=='identity10'&&ruleForm.type=='2'">
        <el-input v-model="ruleForm.company" placeholder="请输入个人姓名"></el-input>
      </el-form-item>
      <el-form-item label="单位名称：" prop="company" v-else>
        <el-input v-model="ruleForm.company" placeholder="请输入单位名称"></el-input>
      </el-form-item>
      <el-form-item label="所属地区：" prop="addrCd">
        <addr @changeAddr="changeAddr"  ref="addrC"></addr>
      </el-form-item>
      <el-form-item label="详细地址：" prop="address">
        <el-input v-model="ruleForm.address" placeholder="请输入详细地址"></el-input>
      </el-form-item>
      <el-form-item label="联系人：" prop="linkMan">
        <el-input v-model="ruleForm.linkMan" placeholder="请输入单位联系人姓名"></el-input>
      </el-form-item>
      <el-form-item label="联系手机：" prop="mob">
        <el-input v-model="ruleForm.mob" type="tel" placeholder="请输入单位联系人手机号码"></el-input>
      </el-form-item>
      <el-form-item label=" 电子邮箱：" prop="email">
        <el-input v-model="ruleForm.email" placeholder="请输入电子邮箱"></el-input>
      </el-form-item>

      <el-form-item>
        <el-button @click="nextStep()">上一步</el-button>
        <!--政府机构提交，其他身份下一步-->
        <el-button type="primary" @click="submitForm('ruleForm')">{{ruleForm.identityCd!='identity100'? '下一步' : '提交'}}</el-button>
      </el-form-item>
    </el-form>
  </div>
</template>

<script>
  import addr from '@/components/addr'
  import {mapState} from "vuex";
  export default {
    props:['info'],

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
              return callback()
            }
          }, 1000);
        }

      };
      return{
        ruleForm: {},
        rules: {
          company:[{
            required: true, message: '请输入单位名称', trigger: 'blur'
          }],
          address:[{
            required: true, message: '请输入详细地址', trigger: 'blur'
          }],
          linkMan:[{
            required: true, message: '请输入联系人', trigger: 'blur'
          }],
          mob: [
            { required: true, validator: checkTel, trigger: 'blur' },
          ],
          addrCd:[{ required: true, message: '请选择地区', trigger: 'blur'}],

        },
        userInfo:{},
      }
    },
    computed: {
      ...mapState([
        'bWidth',
        'width'
      ])
    },
    components: {
      addr
    },
    watch:{
      info:{
        handler:function () {
          this.ruleForm = this.info
          this.ruleForm.company=""
          if(this.ruleForm.identityCd=='identity100'){
            this.rules.company[0].message = '请输入机构名称'
          }else if(this.ruleForm.identityCd=='identity10'&&this.ruleForm.type=='2'){
            console.log('大大大')
            this.rules.company[0].message = '请输入个人姓名'
            this.ruleForm.company=this.userInfo.nickname
          }else{
            this.rules.company[0].message = '请输入单位名称'
          }
        },
        immediate:true,
      },
      'ruleForm.type'(){
        // console.log('变化了====================')
        this.ruleForm.company=""
        if(this.ruleForm.identityCd=='identity100'){
          this.rules.company[0].message = '请输入机构名称'
        }else if(this.ruleForm.identityCd=='identity10'&&this.ruleForm.type=='2'){
          console.log('大大大222')
          this.rules.company[0].message = '请输入个人姓名'
          this.ruleForm.company=this.userInfo.nickname
        }else{
          this.rules.company[0].message = '请输入单位名称'
        }
        // if(this.ruleForm.type==2){
          // this.rules.company[0].required = false
        // }else {
          // this.rules.company[0].required = true
        // }
      }
    },
    mounted(){
      this.userInfo=JSON.parse(this.until.seGet('userInfo'))
      this.$refs.addrC.getProvice()
      // if(this.ruleForm.identityCd=='identity10'){
      //   console.log()
      // }
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
      //下一步
      submitForm(formName) {

        this.$refs[formName].validate((valid) => {
          if (valid) {
            let data = {}
            data.info = this.ruleForm  //当前步骤的信息
            if(this.ruleForm.identityCd=='identity100'){//政府直接提交了
              data.nextIndex = 3 //下一步
              this.store.commit('changeLoading',true)
              this.ruleForm.audit = 1  //状态参数1待审核
              console.log(this.ruleForm)
              this.api.settleInAdd(this.ruleForm).then(()=>{
                this.$message({
                  message: '提交成功，等待审核',
                  type: 'success',
                  duration:'1500'
                });
                this.$emit('setInfo',data)//提交成功后参数变下
                setTimeout(()=>{
                  this.$router.push('./')
                },1500)
              })
            }else {
              data.nextIndex = 2 //下一步 //非政府下一步
              this.$emit('setInfo',data)
            }


          } else {
            console.log('error submit!!');
            return false;
          }
        });
      },
      //页面跳转
      toPage(url){

      }
    }
  }
</script>
<style lang="less">
  @import url("../../../assets/css/init.less");
  .processStep{

    .el-form-item__content{
      width: 398px;
      >p{
        color: #999999;
        font-size: 12px;
      }
    }
    .el-form-item{
      padding-bottom: 10px;
    }
    .el-form-item__label{
      text-align: left;
    }
    .el-form-item__label:before{
      width: 8px;
      content: '';
      display: inline-block;
      margin-right: 4px;
    }
    .el-button{
      text-align: center;
      line-height: 52px;
      padding: 0;
      border-radius: 0;
      border: 1px solid #DFDFDF;
      box-sizing: border-box;
      width: 109px;
    }

    .el-button--primary{
      width: 269px;
      text-align: center;
      line-height: 52px;
      background: @themeColor;
      padding: 0;
      border-radius: 0;
      border: 0;
    }
  }
</style>
<style lang="less" scoped>
  @import url("../../../assets/css/init.less");
  .processStep{
    width: 60%;
    margin: 0 auto;
    padding-bottom: 20px;
    .title{
      font-size: 20px;
      color: #333333;
      padding-top: 50px;
      font-weight: bold;
      padding-bottom: 50px;
      span{
        display: inline-block;
        width: 4px;
        height: 17px;
        background: #E4393C;
        margin-right: 10px;
      }
    }
  }
</style>
