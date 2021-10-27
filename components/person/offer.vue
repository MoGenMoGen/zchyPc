<template>
<!--投标报价弹窗-->
<div id="offer" v-show="offer">
  <div class="body">
    <div class="title">
      <p>报价</p>
      <img @click="close" src="@/assets/img/close.png"/>
    </div>
    <div class="foot">
      <div class="form">
        <el-form :label-position="labelPosition" label-width="100px" :model="info">
          <el-form-item label="项目名称：">
            <p>{{applyInfo.nm}}</p>
          </el-form-item>
          <el-form-item label="投标金额：">
            <el-input type="text" v-model="info.offerAmt" id="long" placeholder="请填写投标报价金额"></el-input>
          </el-form-item>
          <el-form-item label="附件上传：">
              <div class="imgBox">
                <div class="img">
                  <div class="uploadImg">
                    <div>
                      <p >+</p>
                      <input ref="upload" type="file" name="file"   multiple="multiple" @change="upFile($event)"/>
                    </div>
                  </div>
                </div>
                <div v-for="(item,index) in fileList" :key="index" class="fileList">

<!--                  <img :src="item.imgUrl" @click="toLink(item)"/>-->
                  <p @click="toLink(item.url)"><img :src="item.imgUrl"/>{{index+1}}.{{item.nm}}</p><span><img :src="del" class="delImg" @click="deleteFile(index)"/></span>
                </div>
              </div>
          </el-form-item>
        </el-form>
      </div>
      <div class="button">
        <button @click="close">取消</button>
        <button @click="submit">保存</button>
      </div>
    </div>
  </div>
</div>
</template>

<script>
  import {mapState} from "vuex";
  import delImg from '@/assets/img/personal/del.png'
  import excel from '@/assets/img/personal/excel.png'
  import ppt from '@/assets/img/personal/ppt.png'
  import word from '@/assets/img/personal/word.png'
  import pdf from '@/assets/img/personal/pdf.jpg'
  import del from '@/assets/img/personal/del.png'

  export default {
        name: "offer",
      data(){
          return{
            labelPosition: 'left',
            imageUrl: '',
            delImg,
            excel,
            ppt,
            word,
            pdf,
            del,
            fileList:[],
            info:{
              orgId: "5024282848367616",
              orgNm: "中创海洋科技股份有限公司",
              bidId: "5031217130673152",
              bidNm: "",
              offerAmt: "",
              imgUrl: "",
              attachment: '',
              rmks: "",
              completeTm:'',
            },
          }
      },
    props: {
      offer: {
        type: Boolean,
        default: false
      },
      applyInfo:{
        type:Object,

      }
    },
      computed:{
        ...mapState([
          'bWidth',
          'width',
          "currentRole",
        ])
      },
      mounted() {
          this.info.completeTm = this.applyInfo.completeTm
      },
    methods:{
        close(data) {
          this.$refs.upload.value = ''
          this.fileList=[]
          this.$emit('close',data)
        },
      submit(){
        if(this.until.TimeStep(this.info.completeTm) >= 0){
          this.$message({
            message: '已经过了截止时间',
            type: 'warning',
            duration: '1500',
            offset: '50'
          });
          return
        }
        if(!this.info.offerAmt){
          this.$message({
            message: '请填写投标金额',
            type: 'warning',
            duration: '1500',
            offset: '50'
          });
          return
        }
          this.info.orgId = this.applyInfo.orgId
          this.info.orgNm = this.applyInfo.orgNm
          this.info.bidId = this.applyInfo.bidId
          this.info.bidNm = this.applyInfo.nm
        // console.log('提交了')
        // return
          this.api.bidOffer(this.info).then(()=>{
            this.$message({
              message: '报价成功',
              type: 'success',
              duration: '1500',
            });
            this.fileList=[]
            this.$refs.upload.value = ''
            this.close('submit')
          })

      },
      //删除文件
      deleteFile(index){
        this.fileList.splice(index,1)
      },
      //上传文件
      async upFile(e){
        console.log(e)
        if(e.target.files.length===0){
          return
        }
        let img = await this.api.uploadImgEnc(e)
        let type=img.split('.')[img.split('.').length - 1]
        let nm = e.target.files[0].name
        this.info.attachment = img
        if(type=='pdf'){
          this.fileList.push({url:img,imgUrl:this.pdf,type:'1',nm:nm})
        }else if(type=='doc'||type=='docx'){
          this.fileList.push({url:img,imgUrl:this.word,type:'1',nm:nm})
        }else if(type=='ppt'||type=='pptx'){
          this.fileList.push({url:img,imgUrl:this.ppt,type:'1',nm:nm})
        }else if(type=='xls'||type=='xlsx'){
          this.fileList.push({url:img,imgUrl:this.excel,type:'1',nm:nm})
        }else{
          this.fileList.push({url:img,imgUrl:img,type:'2',nm:nm})
        }
        console.log('文件列表')
        console.log(this.fileList)
      },
      toLink(url){
        console.log(url)
        window.open(url)
      },

      },
    }
</script>

<style lang="less">
  .avatar-uploader .el-upload {
    border: 1px dashed #d9d9d9;
    border-radius: 6px;
    cursor: pointer;
    position: relative;
    overflow: hidden;
  }
  .avatar-uploader .el-upload:hover {
    border-color: #409EFF;
  }
  .avatar-uploader-icon {
    font-size: 28px;
    color: #8c939d;
    width: 85px;
    height: 85px;
    line-height: 85px;
    text-align: center;
  }
  .avatar {
    width: 85px;
    height: 85px;
    display: block;
  }
</style>
<style lang="less" scoped>
  @import url("../../assets/css/init.less");
#offer{
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0,0,0,.5);
  z-index: 10;
  .body{
    background: #fff;
    border-radius: 8px;
    /*flex: 1;*/
    /*overflow-y: scroll;*/
    position: fixed;
    top: 50%;
    left: 50%;
    transform: translate(-50%,-50%);
    max-height: 597px;
    padding-bottom: 20px;
    width: 440px;
    .title{
      height: 77px;
      display: flex;
      align-items: center;
      justify-content: center;
      position: relative;
      border-bottom: 1px solid #DFDFDF;
      >p{
        color: #333333;
        font-size: 18px;
      }
      >img{
        width: 20px;
        position: absolute;
        top: 19px;
        right: 21px;
        cursor: pointer;
      }
    }
  }
  .foot{
    .form{
      padding: 0 38px;
    }
    .button{
      display: flex;
      padding: 0 27px;
      justify-content: space-between;
      button:nth-child(1){
        width: 138px;
        height: 52px;
        background-color: #FFFFFF;
        border: 1px solid #DDDDDD;
        font-size: 16px;
        color: #666;
        cursor: pointer;
      }
      button:nth-child(2){
        width: 238px;
        height: 52px;
        background-color: #2778BE;
        font-size: 16px;
        border: 0;
        color: #FFF;
        cursor: pointer;
      }
    }
  }
}
  .imgBox{
    width: 100%;
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
        /*width: 100%;*/
        /*width: 85px;*/
        /*height: 85px;*/
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
  .fileList{
    width: 100%;
    display: flex;
    display: -webkit-flex;
    /*flex-direction: column;*/
    >span>img{
      width: 10px;
      cursor: pointer;
    }
    >p{
      color: #333333;
      font-size: 13px;
      flex: 1;
      cursor: pointer;
      >img{
        width: 16px;
        height: 18px;
        vertical-align: sub;
        margin-right: 10px;
      }
    }
  }
</style>
