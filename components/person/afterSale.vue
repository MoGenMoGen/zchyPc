<template>
  <div class="afterSale">
    <div class="container">
      <div class="header">
          申请
        <img :src="closeImg" @click="close"/>
      </div>
      <el-scrollbar class="main">
        <div class="proList">
          <p>退款商品：</p>
          <ul>
            <li v-for="item in list" :key="item.id">
              <img :src="item.goodsImgUrl"/>
              <p>{{item.goodsNm}}</p>
            </li>
          </ul>
        </div>
        <el-form :model="ruleForm" :rules="rules" ref="ruleForm" label-width="100px" class="demo-ruleForm">
          <el-form-item label="服务类型： " prop="type">
            <el-radio-group v-model="ruleForm.type">
              <el-radio label="1">仅退款</el-radio>
              <el-radio label="0">退货退款</el-radio>
            </el-radio-group>
          </el-form-item>
          <el-form-item label="退款原因：" prop="applyReas">
            <el-input type="textarea" v-model="ruleForm.applyReas"></el-input>
          </el-form-item>
          <!--<el-form-item label="退款金额：" prop="name">-->
            <!--<el-input v-model="ruleForm.name"></el-input>-->
          <!--</el-form-item>-->
          <el-form-item label="上传图片：" prop="img">
            <div class="imgBox">
              <div class="img">
                <div v-for="(item,index) in imgList" :key="index">
                  <img :src="delImg" class="delImg" @click="deleteImg(index)"/>
                  <img :src="item" @click="toLink(item)"/>
                </div>
                <div class="uploadImg">
                  <div>
                    <p >+</p>
                    <input ref="upload" type="file" name="file"   multiple="multiple" @change="upImg($event)"/>
                  </div>
                </div>
              </div>
            </div>
          </el-form-item>
          <el-form-item label="联系电话： " prop="receMob">
            <el-input v-model="ruleForm.receMob" type="tel"></el-input>
          </el-form-item>

          <el-form-item>
            <el-button type="primary" @click="submitForm('ruleForm')">确认</el-button>
          </el-form-item>
        </el-form>

      </el-scrollbar>

    </div>
  </div>
</template>

<script>
    import closeImg from '@/assets/img/close.png'
    import delImg from '@/assets/img/personal/del.png'
    export default {
        name: "role",
      data(){
          return{
            choose:'',
            closeImg,
            delImg,
            list:[],
            imgList:[],
            ruleForm:{
              img:'',
              receMob: '',
              type:'',
              applyReas:'',
              resource: '',
              desc: ''
            },
            rules: {
              receMob: [
                { required: true, message: '请输入联系方式', trigger: 'blur' },
              ],
              applyReas: [
                { required: true, message: '请填写原因', trigger: 'blur' }
              ],
              type: [
                {  required: true, message: '请选择服务类型', trigger: 'change' }
              ],
              img:[
                { required: true, message: '请上传图片', }
              ]
            }
          }
      },
      props: {
        info: {
          type: Object,
          default: null
        },
      },
     watch:{
       info(){
         if(this.info){
           this.list = this.info.itms
         }
       }
     },
      mounted(){
      },
      methods: {
          close(){
            this.$emit('close')
          },
        async upImg(e){
          if(e.target.files.length===0){
            return
          }
          let img = await this.api.uploadImg(e)
          this.imgList.push(img)
          this.ruleForm.img = this.imgList.join(',')
        },
        //删除图片
        deleteImg(index){
          this.imgList.splice(index,1)
          this.ruleForm.img = this.imgList.join(',')
        },
        toLink(url){
          window.open(url)
        },
        submitForm(formName) {
          this.$refs[formName].validate((valid) => {
            if (valid) {
              let list = []
              this.info.itms.forEach(item=>{
                list.push({
                  orderItmId:item.id,
                  rtnQty:item.qty
                })
              })
              let param = {
                orderId:this.info.id,
                type:this.ruleForm.type,
                applyReas:this.ruleForm.applyReas,
                img:this.ruleForm.img,
                receMob:this.ruleForm.receMob,
                itmVos:list
              }
              this.api.orderReturn(param).then(()=>{
                this.$message({
                  message: '退货申请成功',
                  type: 'success'
                });
                this.close()
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

<style lang="less" scoped>
  @import url("../../assets/css/init.less");
  .afterSale {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: rgba(0,0,0,.23);
    z-index: 10;
    display: flex;
    display: -webkit-flex;
  }
  .container {
    background: #fff;
    border-radius: 8px;
    margin: auto;
    max-height: 80%;
    padding-bottom: 20px;
    width: 571px;
    display: flex;
    display: -webkit-flex;
    flex-direction: column;
  }
  .header {
    height: 78px;
    font-size: 18px;
    color: #333333;
    width: 100%;
    line-height: 78px;
    text-align: center;
    border-bottom: 1px solid #DFDFDF;
    position: relative;
    img{
      position: absolute;
      width: 20px;
      right: 20px;
      top:20px;
      cursor: pointer;
    }
  }


  .main{
    flex: 1;
    overflow-y: auto;
    width: 100%;
    height: 100%;
    box-sizing: border-box;
    padding-left: 20px;
    .demo-ruleForm{
      padding-right: 40px;
      /deep/.el-button--primary{
         background: @themeColor;
         width: 291px;
        border: 0;
        border-radius: 0;
      }
      .imgBox{
        width: 59.5%;
        .img{
          //overflow: hidden;
          width: 100%;
          >div{
            width:30%;
            margin-right: 3%;
            margin-bottom: 10px;
            float: left;
            position: relative;
            height: 75px;
            img{
              width: 100%;
              height: 100%;
            }
            .delImg{
              position: absolute;
              top: -7.5px;
              right: -7.5px;
              z-index: 2;
              width: 15px;
              height: 15px;
            }
          }
          .uploadImg{
            display: flex;
            display: -webkit-flex;
            align-items: center;
            // width: 85px;
            >p{
              color: #333333;
              font-size: 15px;
              float: left;
              line-height: 85px;
              margin-left: 3%;
            }
            >div{
              display: flex;
              display: -webkit-flex;
              color: #C6C6C6;
              // width: 85px;
              width: 100%;
              // height: 85px;
              height: 100%;
              p{
                // width: 85px;
                width: 100%;
                text-align: center;
                // height: 85px;
                // line-height: 70px;
                border: 1px dashed #C6C6C6;
                font-size: 50px;
                line-height: 60px;
              }
              input{
                width: 100%;
                height: 100%;
                opacity: 0;
                position: absolute;
                left: 0;
                top: 0;
              }
            }
          }
        }
        .img::after{
          content: "";
          display: block;
          clear: both;
        }
      }
    }
    .proList{
      display: flex;
      display: -webkit-flex;
      padding-top: 30px;
      padding-right: 40px;
      >p{
        width: 100px;
        text-align: right;
        padding-right: 12px;
      }
      ul{
        flex: 1;

        li{
          display: flex;
          display: -webkit-flex;
          align-items: center;
          margin-bottom: 30px;
          color: #333333;
          img{
            width: 90px;
            padding-right: 18px;
          }
        }
      }
    }
  }
</style>
