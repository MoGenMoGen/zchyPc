<template>
  <div class="processStep">
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

      <div class="title">
        <span></span>选择入驻身份
      </div>
    <el-form :model="ruleForm" :rules="rules" ref="ruleForm" label-width="100px" class="demo-ruleForm">
      <el-form-item label="入驻身份：" prop="identityCd">
        <el-select v-model="ruleForm.identityCd" placeholder="请选择入驻身份">
          <el-option :label="item.nm" :value="item.cd" v-for="item in roleList" :key="item.cd"></el-option>
        </el-select>
      </el-form-item>
      <el-form-item label="身份属性：" prop="roleType">
        <el-radio-group v-model="ruleForm.type">
          <el-radio label="1" v-show="ruleForm.identityCd!='identity100'">单位</el-radio>
          <!--仅船东和设计院可以选择个人，其他仅有单位一个选项-->
          <el-radio label="2" v-show="ruleForm.identityCd=='identity10' || ruleForm.identityCd=='identity20'">个人</el-radio>
          <el-radio label="3" v-show="ruleForm.identityCd=='identity100'">政府机构</el-radio>
        </el-radio-group>
      </el-form-item>
      <div class="tips">
        <span>*</span>  基本信息、单位资料：入驻单位账号为必填，个人账号为选填 。
      </div>
      <div class="agree">
        <i class="iconfont" :class="{'icon-fangkuang':!agree,'icon-xuanzhong':agree}" @click="agree=!agree"></i> 我已阅读并同意<span @click="centerDialogVisible=true">《中创海洋用户免责声明》</span>
      </div>
      <el-form-item>
        <el-button type="primary" @click="submitForm('ruleForm')">下一步</el-button>
      </el-form-item>
    </el-form>
  </div>
</template>

<script>
import {mapState} from "vuex";
export default {
  props:['type','info'],

  data(){
    return{
      centerDialogVisible:false,
      agree:false,
      cont:'',
      roleList:[{
        label:'船东',
        value:'chuandong',
      },{
        label:'船厂',
        value:'chuanchang',
      },{
        label:'设计院',
        value:'shejiyuan',
      },{
        label:'供应商',
        value:'gongyingshang',
      },{
        label:'检验检测机构',
        value:'jiancijigou',
      },{
        label:'建造监理机构',
        value:'jianlijigou',
      },{
        label:'代理商',
        value:'dailishang',
      },{
        label:'经销商',
        value:'jingxiaoshang',
      },{
        label:'服务商',
        value:'fuwushang',
      }],
      ruleForm: {},
      rules: {
        identityCd: [
          { required: true, message: '请选择入驻角色', trigger: 'change' },
        ],
        type: [
          { required: true, message: '请选择身份属性', trigger: 'change' }
        ],
      }
    }
  },
  computed: {
    ...mapState([
      'bWidth',
      'width'
    ])
  },
  components: {
  },
  watch:{
    info:{
      handler:function () {
        this.ruleForm = this.info
      },
      immediate:true
    },
    type:{
      handler:function () {
        this.ruleForm.identityCd = this.type
      },
      immediate:true
    },
    'ruleForm.identityCd':{
      handler:function () {
          if(this.ruleForm.identityCd=='identity100'){
            this.ruleForm.type = '3'
          }else {
            this.ruleForm.type = '1'
          }
      },
      immediate:true
    }
  },
  async mounted(){
      this.roleList = await this.api.getRoleList()
      this.getCont()
  },
  methods:{
    getCont(){ //声明
      this.api.ruleDetail('5028236608967680').then(res=>{
        this.cont = res.cont
      })
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
    toRead(){

    },
    //页面跳转
    toPage(url){

    }
  }
}
</script>
<style lang="less">
  @import url("../../../assets/css/init.less");
  .el-select-dropdown__wrap{
    margin-bottom: 0!important;
  }
  .processStep{
    .el-select{
      width: 398px;
    }
    .el-form-item{
      padding-bottom: 10px;
    }
    .el-form-item__label{
      text-align: left;
    }
    .el-button--primary{
      width: 398px;
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
    /deep/.el-select{
      width: 398px;
    }
    /deep/.el-form-item{
      padding-bottom: 10px;
    }
    /deep/.el-form-item__label{
      text-align: left;
    }
    /deep/.el-button--primary{
      width: 398px;
      text-align: center;
      line-height: 52px;
      background: @themeColor;
      padding: 0;
      border-radius: 0;
      border: 0;
    }
  }
  .processStep{
     width: 60%;
     margin: 0 auto;
    padding-bottom: 20px;
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
        }
      }
    }
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
    .tips{
      color: #FF3232;
      font-size: 12px;
      span{
        font-size: 14px;
      }
    }
    .agree{
      font-size: 12px;
      padding-left: 100px;
      padding-top: 59px;
      padding-bottom: 38px;
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
