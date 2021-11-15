<template>
  <!--投标保证金弹窗-->
  <div id="offer">
    <div class="body">
      <div class="title">
        <p>保证金</p>
        <img @click="close" src="@/assets/img/close.png" />
      </div>
      <div class="foot">
        <div class="form">
          <el-form :label-position="labelPosition" label-width="120px" :model="info">
            <el-form-item label="保证金金额(元)：" style="margin-top: 10px;">
              <el-input type="text" v-model="info.depositAmt" id="long" placeholder="请填写投标保证金金额"></el-input>
            </el-form-item>
            <el-form-item label="保证金凭证：">
              <div class="imgBox">
                <div class="img" v-if="(applyInfo.despoit&&applyInfo.despoit.shipBidDepositVo.audit!=2)||!applyInfo.despoit">
                  <div class="uploadImg">
                    <div>
                      <p>+</p>
                      <input ref="upload" type="file" name="file" multiple="multiple" @change="upFile($event)" />
                    </div>
                  </div>
                </div>
                <div v-for="(item,index) in fileList" :key="index" class="fileList">
                  <div class="fileList-item" @click="toLink(item.url)"><img
                      :src="item.imgUrl" /><span>{{index+1}}.{{item.nm}}</span></div>
                  <div class="cancel"><img :src="del" class="delImg" @click="deleteFile(index)" /></div>
                </div>
              </div>
            </el-form-item>
          </el-form>
        </div>
        <div class="button" v-if="(applyInfo.despoit&&applyInfo.despoit.shipBidDepositVo.audit!=2)||!applyInfo.despoit">
          <button @click="close">取消</button>
          <button @click="submit">确认</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
  import {
    mapState
  } from "vuex";
  import delImg from '@/assets/img/personal/del.png'
  import excel from '@/assets/img/personal/excel.png'
  import ppt from '@/assets/img/personal/ppt.png'
  import word from '@/assets/img/personal/word.png'
  import pdf from '@/assets/img/personal/pdf.jpg'
  import del from '@/assets/img/personal/del.png'
  export default {
    name: "offer",
    data() {
      return {
        labelPosition: 'left',
        imageUrl: '',
        delImg,
        excel,
        ppt,
        word,
        pdf,
        del,
        list1: [],
        fileList: [],
        info: {
          orgId: "",
          orgNm: "",
          bidId: "",
          bidNm: "",
          depositAmt: "",
          depositImgUrl: '',
        },
      }
    },
    props: {
      bail: {
        type: Boolean,
        default: false
      },
      applyInfo: {
        type: Object,

      }
    },
    computed: {
      ...mapState([
        'bWidth',
        'width',
        "currentRole",
      ])
    },
    mounted() {
      console.log(this.applyInfo)
      if (this.applyInfo.despoit) {
        this.info.depositAmt = this.applyInfo.depositMoney;
        let attachments
        if (this.applyInfo.despoit.shipBidDepositVo.depositImgUrl) {
          attachments = this.applyInfo.despoit.shipBidDepositVo.depositImgUrl.split(",");
          this.info.depositImgUrl = this.applyInfo.despoit.shipBidDepositVo.depositImgUrl+','
        } else {
          attachments = []
        }
        this.fileList = this.getInfo(attachments);
      }
    },
    methods: {
      close(data) {
        if (!this.applyInfo.despoit) {
          this.$refs.upload.value = ''
          this.fileList = []
        }
        this.$emit('close', data)
      },
      submit() {
        if (!this.info.depositAmt) {
          this.$message({
            message: '请填写保证金金额',
            type: 'warning',
            duration: '1500',
            offset: '50'
          });
          return
        }
        if(!this.info.depositImgUrl) {
          this.$message({
            message: '请上传保证金凭证',
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
        this.info.depositImgUrl = this.info.depositImgUrl.substring(0, this.info.depositImgUrl.length - 1)
        // console.log('提交了')
        // return
        if(this.applyInfo.despoit) {
          this.info.id = this.applyInfo.despoit.shipBidDepositVo.id
          this.api.bidBailUpd(this.info).then(() => {
            this.$message({
              message: '修改成功',
              type: 'success',
              duration: '1500',
            });
            this.fileList = []
            this.$refs.upload.value = ''
            this.close('submit')
          })
        } else {
          this.api.bidBail(this.info).then(() => {
            this.$message({
              message: '上传成功',
              type: 'success',
              duration: '1500',
            });
            this.fileList = []
            this.$refs.upload.value = ''
            this.close('submit')
          })
        }
      },
      //删除文件
      deleteFile(index) {
        this.fileList.splice(index, 1)
      },
      //上传文件
      async upFile(e) {
        console.log(e)
        if (e.target.files.length === 0) {
          return
        }
        let img = await this.api.uploadImg(e)
        let type = img.split('.')[img.split('.').length - 1]
        let nm = e.target.files[0].name
        this.info.depositImgUrl += img + ','
        if (type == 'pdf') {
          this.fileList.push({
            url: img,
            imgUrl: this.pdf,
            type: '1',
            nm: nm
          })
        } else if (type == 'doc' || type == 'docx') {
          this.fileList.push({
            url: img,
            imgUrl: this.word,
            type: '1',
            nm: nm
          })
        } else if (type == 'ppt' || type == 'pptx') {
          this.fileList.push({
            url: img,
            imgUrl: this.ppt,
            type: '1',
            nm: nm
          })
        } else if (type == 'xls' || type == 'xlsx') {
          this.fileList.push({
            url: img,
            imgUrl: this.excel,
            type: '1',
            nm: nm
          })
        } else {
          this.fileList.push({
            url: img,
            imgUrl: img,
            type: '2',
            nm: nm
          })
        }
        console.log('文件列表')
        console.log(this.fileList)
      },
      toLink(url) {
        console.log(url)
        window.open(url)
      },
      getInfo(info) {
        let data = info;
        let data1 = [];
        let fileList2 = [];
        if (data.length > 0) {
          data.forEach((v) => {
            let type = v.split(".")[v.split(".").length - 1];
            let nmList = v.split(".com/"); //分割出url后的内容
            let nm = "";
            nmList.forEach((j, z) => {
              //防止文件名中有 .com/ 所以循环加入
              if (z != 0) {
                nm += j;
              }
            });
            nmList = nm.split("_"); //分割随机字符后的内容
            nm = "";
            nmList.forEach((j, z) => {
              //防止文件名中有 _ 所以循环
              if (z != 0) {
                nm += j;
              }
            });
            nm = nm.split("." + type)[0];
            if (type == "pdf") {
              fileList2.push({
                url: v,
                imgUrl: this.pdf,
                nm: nm,
              });
            } else if (type == "doc" || type == "docx") {
              fileList2.push({
                url: v,
                imgUrl: this.word,
                nm: nm,
              });
            } else if (type == "ppt" || type == "pptx") {
              fileList2.push({
                url: v,
                imgUrl: this.ppt,
                nm: nm,
              });
            } else if (type == "xls" || type == "xlsx") {
              fileList2.push({
                url: v,
                imgUrl: this.excel,
                fileNm: nm,
              });
            } else {
              fileList2.push({
                url: v,
                imgUrl: v,
                nm: nm,
              });
            }
          });
        }
        return fileList2;
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

  #offer {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: rgba(0, 0, 0, .5);
    z-index: 10;

    .body {
      background: #fff;
      border-radius: 8px;
      /*flex: 1;*/
      /*overflow-y: scroll;*/
      position: fixed;
      top: 50%;
      left: 50%;
      transform: translate(-50%, -50%);
      max-height: 597px;
      padding-bottom: 20px;
      width: 440px;

      .title {
        height: 77px;
        display: flex;
        align-items: center;
        justify-content: center;
        position: relative;
        border-bottom: 1px solid #DFDFDF;

        >p {
          color: #333333;
          font-size: 18px;
        }

        >img {
          width: 20px;
          position: absolute;
          top: 19px;
          right: 21px;
          cursor: pointer;
        }
      }
    }

    .foot {
      .form {
        max-height: 440px;
        margin-bottom: 8px;
        padding: 0 38px;
        overflow: auto;
      }

      .button {
        display: flex;
        padding: 0 27px;
        justify-content: space-between;

        button:nth-child(1) {
          width: 138px;
          height: 52px;
          background-color: #FFFFFF;
          border: 1px solid #DDDDDD;
          font-size: 16px;
          color: #666;
          cursor: pointer;
        }

        button:nth-child(2) {
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

  .imgBox {
    width: 100%;

    .img {
      //overflow: hidden;
      width: 100%;

      >div {
        width: 30%;
        margin-right: 3%;
        margin-bottom: 10px;
        float: left;
        position: relative;
        height: 75px;

        img {
          width: 100%;
          height: 100%;
        }

        .delImg {
          position: absolute;
          top: -7.5px;
          right: -7.5px;
          z-index: 2;
          width: 15px;
          height: 15px;
        }
      }

      .uploadImg {
        display: flex;
        display: -webkit-flex;
        align-items: center;

        /*width: 100%;*/
        /*width: 85px;*/
        /*height: 85px;*/
        >p {
          color: #333333;
          font-size: 15px;
          float: left;
          line-height: 85px;
          margin-left: 3%;
        }

        >div {
          display: flex;
          display: -webkit-flex;
          color: #C6C6C6;
          // width: 85px;
          width: 100%;
          // height: 85px;
          height: 100%;

          p {
            // width: 85px;
            width: 100%;
            text-align: center;
            // height: 85px;
            // line-height: 70px;
            border: 1px dashed #C6C6C6;
            font-size: 50px;
            line-height: 60px;
          }

          input {
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

    .img::after {
      content: "";
      display: block;
      clear: both;
    }
  }

  .fileList {
    width: 100%;
    display: flex;
    align-items: center;
    display: -webkit-flex;

    .fileList-item {
      width: 80%;
      display: flex;
      flex-direction: column;
      cursor: pointer;

      img {
        width: 32px;
        height: 36px;
      }

      span {
        display: block;
        overflow-wrap: break-word;
        line-height: normal;
      }
    }

    .cancel {
      img {
        width: 20px;
        cursor: pointer;
        margin-left: 10px;
      }
    }
  }
</style>
