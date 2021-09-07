<template>
  <div class="processStep">
    <div class="title">
      <span></span>单位入驻资料
    </div>
    <el-form :model="ruleForm" :rules="rules" ref="ruleForm" label-width="130px" class="demo-ruleForm">
      <el-form-item label="单位营业执照：" prop="businessLicense" v-show="ruleForm.type!=2">
        <div class="uploadImg">
          <div class="img">
            <img :src="ruleForm.businessLicense ? ruleForm.businessLicense : yyzz"/>
            <p v-show="!ruleForm.businessLicense">示例图</p>
          </div>
          <div class="button">
            <div class="upload">
              上传营业执照
              <input type="file" @change="upImg($event,1)"/>
            </div>
            <p><span>*</span>
              文件大小不能大于1M<br/>
              文件格式支持jpg、png、pdf
            </p>
          </div>
        </div>
      </el-form-item>
      <el-form-item label="法人身份证：" prop="roleCd" class="cardId">
        <div class="uploadImg" style="padding-bottom: 30px;">
          <div class="imgCard">
            <img :src="ruleForm.idCard ? ruleForm.idCard : id1"/>
            <p v-show="!ruleForm.idCard">示例图</p>
          </div>
          <div class="button">
            <div class="upload">
              上传身份证正面
              <input type="file" @change="upImg($event,2)"/>
            </div>
            <p><span>*</span>
              文件大小不能大于1M<br/>
              文件格式支持jpg、png、pdf
            </p>
          </div>
        </div>
        <div class="uploadImg">
          <div class="imgCard">
            <img :src="ruleForm.idCardReverse ? ruleForm.idCardReverse : id2"/>
            <p v-show="!ruleForm.idCardReverse">示例图</p>
          </div>
          <div class="button">
            <div class="upload">
              上传身份证反面
              <input type="file" @change="upImg($event,3)"/>
            </div>
            <p><span>*</span>
              文件大小不能大于1M<br/>
              文件格式支持jpg、png、pdf
            </p>
          </div>
        </div>

      </el-form-item>
      <el-form-item label="单位资质证明：" prop="qualification" v-show="ruleForm.type!=2">
        <div class="uploadImg">
          <div class="img">
            <img :src="ruleForm.qualification ? ruleForm.qualification : zzzs"/>
            <p v-show="!ruleForm.qualification">示例图</p>
          </div>
          <div class="button">
            <div class="upload">
              上传单位资质
              <input type="file" @change="upImg($event,4)"/>
            </div>
            <!--<span>*</span>-->
            <p>
              文件大小不能大于1M<br/>
              文件格式支持jpg、png、pdf
            </p>
          </div>
        </div>
      </el-form-item>
      <el-form-item label="单位简介：" prop="summery">
        <el-input type="textarea" v-model="ruleForm.summery"></el-input>
      </el-form-item>
      <el-form-item label="供应商品：" prop="roleCd">
        <el-input v-model="ruleForm.business" placeholder="请输入可供应商品的名称"></el-input>
      </el-form-item>
      <el-form-item>
        <el-button @click="nextStep()">上一步</el-button>
        <el-button type="primary" @click="submitForm('ruleForm')">提交</el-button>
      </el-form-item>
    </el-form>
  </div>
</template>

<script>
  import {mapState} from "vuex";
  import yyzz from './img/yyzz.png'
  import id1 from './img/id1.png'
  import id2 from './img/id2.png'
  import zzzs from './img/zzzs.png'
  export default {
    props:['info'],
    data(){
      return{
        yyzz,
        id1,
        id2,
        zzzs,
        ruleForm: {
          idCardReverse:'',
          businessLicense:'',
          qualification:'',
          img2:'',
          img3:'',
          img4:'',
          roleCd:'',
          roleType:'',

          desc:'',
        },
        rules: {

          businessLicense: [
            { required: true, message: '请上传营业执照', trigger: 'change' }
          ],
          // qualification: [
          //   { required: true, message: '请上传资质证书', trigger: 'change' }
          // ],
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
    watch:{
      info:{
        handler:function () {
          this.ruleForm = this.info
        },
        immediate:true
      },
      'ruleForm.type'(){
        console.log('变化了====================')
        if(this.ruleForm.type==2){
          this.rules.businessLicense[0].required = false
        }else {
          this.rules.businessLicense[0].required = true
        }
      }
    },
    mounted(){


    },
    methods:{
      //上传图片
      async upImg(e,imgType) {
        // console.log(imgType)
        console.log(e)
        if(e.target.files.length===0){
          return
        }
        this.store.commit('changeLoading',true)

        let imgUrl = await this.api.uploadImg(e)

        if(imgType==1){
          this.ruleForm.businessLicense = imgUrl
          // console.log(this.ruleForm.businessLicense)
        }
        if(imgType==2){
          this.ruleForm.idCard = imgUrl
        }
        if(imgType==3){
          this.ruleForm.idCardReverse = imgUrl
          console.log(this.ruleForm.idCardReverse)
          this.$set(this.ruleForm,'idCardReverse',imgUrl)
        }
        if(imgType==4){
          this.ruleForm.qualification = imgUrl
        }
      },
      //上一步
      nextStep(){
        let data = {}
        data.info = this.ruleForm  //当前步骤的信息
        data.nextIndex = 1 //下一步
        this.$emit('setInfo',data)
      },
      //提交
      submitForm(formName) {
        this.$refs[formName].validate((valid) => {
          if (valid) {
            this.store.commit('changeLoading',true)
            this.ruleForm.audit = 1  //状态参数1待审核
            console.log(this.ruleForm)
            this.api.settleInAdd(this.ruleForm).then(()=>{
              this.$message({
                message: '提交成功，等待审核',
                type: 'success',
                duration:'1500'
              });
              setTimeout(()=>{
                this.$router.push('./')
              },1500)
            })
          } else {
            console.log('error submit!!');
            return false;
          }
        });
      },

    }
  }
</script>
<style lang="less">
  @import url("../../../assets/css/init.less");
  .processStep{
    .el-form-item__content{
      width: 75%;
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
      margin-right: 4px;
      display: inline-block;
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
    .uploadImg{
      display: flex;
      display: -webkit-flex;
      width: 100%;
    }
    .img{
       border-radius: 8px;
       width: 295px;
       background: #F5F5F5;
      text-align: center;
      padding: 20px 20px 20px 20px;
      box-sizing: border-box;
      margin-right: 22px;
      p{
        line-height: 10px;
      }
    }
    .imgCard{
      width: 255px;
      background: #EAEAEA;
      border-radius: 8px;
      text-align: center;
      padding: 20px 20px 20px 20px;
      box-sizing: border-box;
      margin-right: 22px;
      p{
        line-height: 10px;
      }
    }
    .button{
      .upload{
        cursor: pointer;
        width: 130px;
        height: 34px;
        text-align: center;
        line-height: 34px;
        background: @themeColor;
        color: #ffffff;
        font-size: 12px;
        border-radius: 3px;
        border: none;
        margin-bottom: 18px;
        margin-top: 20px;
        position: relative;
        input{
          position: absolute;
          width: 100%;
          height: 100%;
          z-index: 8;
          left: 0;
          top:0;
          cursor: pointer;
          opacity: 0;
        }
      }

      p{
        color: #999999;
        font-size: 12px;
        line-height: 16px;
        display: flex;
        display: -webkit-flex;
        span{
          display: inline-block;
          color: @redColor;
          margin-right: 5px;
        }
      }
    }
  }
</style>
