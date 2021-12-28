<template>
  <!--  检测报告(编辑)-->
  <div>
    <div class="bigBox" v-if="submitShow">
      <div class="submitBox2">
        <div class="submitTitle">
          <p>修改</p>
          <img :src="close" alt="" @click="submitShow=false">
        </div>
        <div class="content">
          <div>
            <p class="left-title">报告名称：</p>
            <p class="left-msg">
              <el-input v-model="form.nm"></el-input>
            </p>
          </div>
          <div>
            <p class="left-title">报告出具时间：</p>
            <p class="left-msg">
              <el-date-picker v-model="form.validUntil" type="date" placeholder="选择日期时间">
              </el-date-picker>
            </p>
          </div>
          <div>
            <p class="left-title">上传附件：</p>
            <!-- <div class="imgBox">
              <div class="img">
                <div class="uploadImg">
                  <div>
                    <p>+</p>
                    <input ref="upload" type="file" name="file" multiple="multiple" @change="upImg($event)" />
                  </div>
                </div>
                <div v-for="(item,index) in imgList" :key="index">
                  <img :src="del" class="delImg" @click="deleteImg(index)" />
                  <img :src="item.imgUrl" v-viewer />
                </div>
              </div>
            </div> -->
            <div class="fileBox">
              <div class="file">
                <div class="uploadFile">
                  <div>
                  	<button>选择文件</button>
                    <input ref="upload" type="file" name="file"   multiple="multiple" @change="upFile($event)"/>
                  </div>
                </div>
                <div v-for="(item,index) in fileList" :key="index" class="fileP">
                  <p @click="toLink(item.url)">{{item.url}}</p>
                  <img :src="del" class="delImg" @click="deleteFile(index)"/>
                </div>
              </div>
            </div>
          </div>
          <button @click="toSubmit">保存</button>
        </div>
      </div>
    </div>
    <!--    船舶信息-->
    <div class="info">
      <!-- <img :src="info.logo"> -->
      <div>
        <p>船舶编号:<span>{{shipCd}}</span></p>
        <p>船舶档案名称:<span>{{fileNm}}</span></p>
        <p style="width: 100%;display: flex;align-items: center;justify-content: space-between;">
          <span>船舶状态:<span style="color: #2778BE">{{shipStatus}}</span></span>
          <span class="addNew" @click="addNew"><img src="../img/closexx.png">新增</span>
         </p>
      </div>
    </div>
    <div class="bigBox" v-if="submitShow2">
      <div class="submitBox2">
        <div class="submitTitle">
          <p>新增</p>
          <img :src="close" alt="" @click="submitShow2=false">
        </div>
        <div class="content">
          <div>
            <p class="left-title">报告名称：</p>
            <p class="left-msg">
              <el-input v-model="form.nm" placeholder="请输入报告名称"></el-input>
            </p>
          </div>
          <div>
            <p class="left-title">报告出具时间：</p>
            <p class="left-msg">
              <el-date-picker v-model="form.validUntil" type="date" placeholder="选择日期时间">
              </el-date-picker>
            </p>
          </div>
          <div>
            <p class="left-title">上传附件：</p>
            <!-- <div class="imgBox">
              <div class="img">
                <div class="uploadImg">
                  <div>
                    <p>+</p>
                    <input ref="upload" type="file" name="file" multiple="multiple" @change="upImg($event)" />
                  </div>
                </div>
                <div v-for="(item,index) in imgList" :key="index">
                  <img :src="del" class="delImg" @click="deleteImg(index)" />
                  <img :src="item.imgUrl" @click="toLink(item)" />
                </div>
              </div>
            </div> -->
            <div class="fileBox">
              <div class="file">
                <div class="uploadFile">
                  <div>
                  	<button>选择文件</button>
                    <input ref="upload" type="file" name="file"   multiple="multiple" @change="upFile($event)"/>
                  </div>
                </div>
                <div v-for="(item,index) in fileList" :key="index" class="fileP">
                  <p @click="toLink(item.url)">{{item.url}}</p>
                  <img :src="del" class="delImg" @click="deleteFile(index)"/>
                </div>
              </div>
            </div>
          </div>
          <button @click="toSubmit" style="cursor: pointer;">保存</button>
        </div>
      </div>
    </div>
    <div class="certificate">
      <div class="title">
        <p @click="currentType=1" :class="{active:currentType===1}">检测报告</p>
        <p>
          <span>({{list.length}})</span>
        </p>
      </div>
      <!--通过审核-->
      <ul v-show="currentType">
        <li v-for="(item,index) in list" :key="index" :style="{width:width*0.16+'px'}">

          <!-- <div class="img" :style="{height:width*0.16*size+'px'}">
            <img :src="item.imgList[0]">
            <viewer :images="item.imgList" class="viewer" :style="{height:width*0.16*size+'px',width:width*0.16+'px'}">
              <img v-for="(src,k) in item.imgList" :src="src" :key="k">
            </viewer>
          </div> -->
          <div class="doc">
            <p v-for="j in item.fileList" @click="toLink(j.url)">
              <img :src="j.img">
              <span>{{j.fileNm}}</span>
            </p>
          </div>
          <p>{{item.nm}}</p>
          <p>证书有效期：{{item.validUntil}}</p>
          <p class="set">
            <sapn style="margin-right: 30px" v-if="item.audit==2">审核已通过</sapn>
            <sapn style="margin-right: 30px" v-else-if="item.audit==1">待审核</sapn>
            <sapn style="margin-right: 30px" v-else>未通过</sapn>
            <span class="shape" @click.stop="change(index)" style="margin-right: 10px" v-if="item.audit!=2"><i class="el-icon-edit"></i>修改</span>
            <span class="shape" @click.stop="delet2(index)" v-if="item.audit!=2"><i class="el-icon-delete"></i>删除</span>
          </p>
        </li>
      </ul>
      <!--未通过审核-->
      <ul v-show="!currentType">
        <li v-for="(item,index) in listNo" :key="index" :style="{width:width*0.16+'px'}">
          <!-- <div class="img" :style="{height:width*0.16*size+'px'}">
            <viewer :images="item.imgList" class="viewer" :style="{height:width*0.16*size+'px',width:width*0.16+'px'}">
              <img v-for="(src,k) in item.imgList" :src="src" :key="k">
            </viewer>
          </div> -->
          <div class="doc">
            <p v-for="j in item.fileList" @click="toLink(j.url)">
              <img :src="j.img">
              <span>{{j.fileNm}}</span>
            </p>
          </div>
          <p>{{item.nm}}</p>
          <p>证书有效期：{{item.validUntil}}</p>
          <p v-if="item.options">审核意见：{{item.options}}</p>
          <p class="set">
            <span @click="change(index)"><i class="el-icon-edit"></i>修改</span>
            <span @click="delet(index)"><i class="el-icon-delete"></i>删除</span>
          </p>
        </li>
      </ul>
    </div>

  </div>

</template>

<script>
  import {
    mapState
  } from "vuex";
  import test1 from '../../assets/img/personal/test1.png'
  import arrowUp from '../../components/img/arrowUp.png'
  import arrowDown from '../../components/img/arrowDown.png'
  import excel from '@/assets/img/personal/excel.png'
  import ppt from '@/assets/img/personal/ppt.png'
  import word from '@/assets/img/personal/word.png'
  import pdf from '@/assets/img/personal/pdf.jpg'
  import del from '@/assets/img/personal/del.png'

  import close from "@/assets/img/close.png"
  export default {
    name: "fangansheji",
    computed: {
      ...mapState([
        'bWidth',
        'width'
      ])
    },
    data() {
      return {
        submitShow: false,
        submitShow2: false,
        currentType: 1, //1通过审核的，0未通过审核的
        close,
        test1,
        excel,
        ppt,
        word,
        pdf,
        del,
        arrowDown,
        arrowUp,
        size: 1, //证书图片的宽高比
        fileNm: "",
        id: "",
        shipCd: "",
        shipStatus: "",
        list: [],
        listNo: [],
        info: {},
        imgList: [],
        fileList: [],
        currentRole: {},
        form: {
          nm: "",
          docsId: "",
          validUntil: "",
          imgUrl: "",
          attachment: '',
          issuerId: "",
          issuerNm: "",
          types: "2", //0 船舶检验证书（后台上传） 1设备及其他证书 2检测报告
        },
      }
    },
    async mounted() {

      this.fileNm = this.until.getQueryString('fileNm')
      this.id = this.until.getQueryString('id')
      this.shipCd = this.until.getQueryString('shipCd')
      this.shipStatus = this.until.getQueryString('shipStatus')
      this.currentRole = JSON.parse(this.until.seGet('currentRole'))
      let param = {
        docsId: this.id,
        identityCd: 'identity50'
      }
      this.info = await this.api.orgInfoBasic(param)
      this.getInfo()
      this.getInfoNo()
    },
    methods: {
      addNew() {
        this.submitShow2 = true
      },
      //修改
      change(index) {
        this.form = JSON.parse(JSON.stringify(this.list[index]))
        this.imgList = []
        if (this.form.imgList) {
          this.form.imgList.forEach(item => {
            this.imgList.push({
              url: item,
              imgUrl: item,
              type: '2'
            })
          })
        }
        this.fileList = []
        if (this.form.fileList) {
          this.form.fileList.forEach(item => {
            this.fileList.push({
              url: item.url,
              imgUrl: item.img
            })
          })
        }
        console.log(this.fileList)
        this.submitShow = true
      },

      //删除
      delet2(index) {
        this.$confirm('确定删除?', '提示', {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning'
        }).then(() => {
          this.api.inspectDel(this.list[index].id).then(res => {
            this.listNo.splice(index, 1)
            this.$message({
              type: 'success',
              message: '删除成功!'

            });
            this.getInfo()
          })

        }).catch(() => {

        });
      },
      //删除
      delet(index) {
        this.$confirm('确定删除?', '提示', {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning'
        }).then(() => {
          this.api.inspectDel(this.listNo[index].id).then(res => {
            this.listNo.splice(index, 1)
            this.$message({
              type: 'success',
              message: '删除成功!'
            });
          })

        }).catch(() => {

        });
      },
      toSubmit() {
        this.form.docsId = this.id
        this.form.issuerId = this.currentRole.id
        this.form.issuerNm = this.currentRole.company
        let imgList = []
        this.imgList.forEach(item => {
          imgList.push(item.url)
        })
        let fileList = []
        this.fileList.forEach(item => {
          fileList.push(item.url)
        })
        this.form.imgUrl = ''
        this.form.attachment = fileList.join(",")
        if (!this.form.validUntil) {
          this.$message({
            message: '请选择有效日期',
            type: 'warning',
            duration: '1500'
          });
          return
        }
        let timeC = this.until.formatDate(this.form.validUntil)
        this.form.validUntil = timeC.year + '-' + timeC.month + '-' + timeC.day
        console.log(this.form)
        if (!this.form.nm) {
          this.$message({
            message: '请输入名称',
            type: 'warning',
            duration: '1500'
          });
          return
        }
        if (this.form.id) {
          this.api.inspectUpd(this.form).then((res) => {
            if (res.code == 0) {
              this.$message({
                message: '修改成功',
                type: 'success',
                duration: '1000'
              });
              setTimeout(() => {
                this.submitShow = false
                this.form = {
                  nm: "",
                  docsId: "",
                  validUntil: "",
                  imgUrl: "",
                  issuerId: "",
                  issuerNm: "",
                  types: "2", //0 船舶检验证书（后台上传） 1设备及其他证书 2检测报告
                }
                this.imgList = []
                this.listNo = []
                this.getInfo()
              }, 1000)
            } else {
              this.$message({
                message: res.msg,
                type: 'warning',
                duration: '1000'
              });
            }

          })
        } else {
          this.api.inspectAdd(this.form).then((res) => {
            if (res.code == 0) {
              this.$message({
                message: '新增成功',
                type: 'success',
                duration: '1000'
              });
              setTimeout(() => {
                this.form = {
                  nm: "",
                  docsId: "",
                  validUntil: "",
                  imgUrl: "",
                  issuerId: "",
                  issuerNm: "",
                  types: "2", //0 船舶检验证书（后台上传） 1设备及其他证书 2检测报告
                }
                this.imgList = []
                this.list = []
                this.submitShow2 = false
                this.getInfo()
              }, 1000)
            } else {
              this.$message({
                message: res.msg,
                type: 'warning',
                duration: '1000'
              });
            }

          })
        }

      },

      async getInfo() {
        this.list = []
        // this.currentRole=JSON.parse(this.until.seGet('currentRole'))
        let qry = this.query.new()
        this.query.toW(qry, 'docsId', this.id, 'EQ')
        this.query.toW(qry, 'types', '2', 'EQ')
        this.query.toP(qry, '1', '999')
        let data = await this.api.jianyanListAll(this.query.toEncode(qry))
        if (data.length > 0) {
          data.forEach((item, index) => {
            item.validUntil = item.validUntil ? item.validUntil.split(' ')[0] : ''
            item.imgList = item.imgUrl ? item.imgUrl.split(',') : []
            let fileList1=item.attachment?item.attachment.split(','):[]
            let fileList2=[]
            fileList1.forEach(v=>{
              let type=v.split('.')[v.split('.').length - 1]
              let nmList=v.split('.com/')  //分割出url后的内容
              let nm=""
              nmList.forEach((j,z)=>{       //防止文件名中有 .com/ 所以循环加入
                if(z!=0){
                  nm+=j
                }
              })
              nmList=nm.split('_')        //分割随机字符后的内容
              nm=""
              nmList.forEach((j,z)=>{   //防止文件名中有 _ 所以循环
                if(z!=0){
                  nm+=j
                }
              })
              nm=nm.split('.'+type)[0]
              if(type=='pdf'){
              	fileList2.push({url:v,img:this.pdf,'fileNm':nm})
              }else if(type=='doc'||type=='docx'){
              	fileList2.push({url:v,img:this.word,'fileNm':nm})
              }else if(type=='ppt'||type=='pptx'){
              	fileList2.push({url:v,img:this.ppt,'fileNm':nm})
              }else if(type=='xls'||type=='xlsx'){
              	fileList2.push({url:v,img:this.excel,'fileNm':nm})
              }else{
              	fileList2.push({url:v,img:v,'fileNm':nm})
              }
              item.fileList=fileList2
            })
            this.list.push(item)
          })
        }
        // console.log('列表')
        // console.log(this.list)
      },
      async getInfoNo() {
        this.listNo = []
        // this.currentRole=JSON.parse(this.until.seGet('currentRole'))
        let qry = this.query.new()
        this.query.toW(qry, 'docsId', this.id, 'EQ')
        this.query.toP(qry, '1', '999')
        let data = await this.api.jianyanListNo2(this.query.toEncode(qry))
        if (data.length > 0) {
          data.forEach((item, index) => {
            item.validUntil = item.validUntil ? item.validUntil.split(' ')[0] : ''
            item.imgList = item.imgUrl ? item.imgUrl.split(',') : [],
            this.listNo.push(item)
          })
        }
        // console.log('列表')
        // console.log(this.list)
      },
      //删除图片
      deleteImg(index) {
        this.imgList.splice(index, 1)
      },
      //上传图片
      async upImg(e) {
        console.log(e)
        if (e.target.files.length === 0) {
          return
        }
        let img = await this.api.uploadImg(e)
        this.imgList.push({
          url: img,
          imgUrl: img,
          type: '2'
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
        let img = await this.api.uploadImg(e)
        let type=img.split('.')[img.split('.').length - 1]
        console.log(type)
        if(type=='pdf'){
        	this.fileList.push({url:img,imgUrl:this.pdf,type:'1'})
        }else if(type=='doc'||type=='docx'){
        	this.fileList.push({url:img,imgUrl:this.word,type:'1'})
        }else if(type=='ppt'||type=='pptx'){
        	this.fileList.push({url:img,imgUrl:this.ppt,type:'1'})
        }else if(type=='xls'||type=='xlsx'){
        	this.fileList.push({url:img,imgUrl:this.excel,type:'1'})
        }else{
        	this.fileList.push({url:img,imgUrl:img,type:'2'})
        }
        console.log('文件列表')
        console.log(this.fileList)
      },
      toLink(url) {
        console.log(url)
        window.open(url)
      },
      // show(index){
      //   let vier="viewer"+index
      //   console.log(vier)
      //   console.log(this.$refs[vier])
      //   // this.$refs[vier].show()
      // }
    }


  }
</script>

<style scoped lang="less">
  @import url("../../assets/css/init.less");

  .bigBox {
    width: 100%;
    height: 100%;
    background-color: rgba(0, 0, 0, 0.5);
    position: fixed;
    top: 0;
    left: 0;
    display: flex;
    align-items: center;
    justify-content: center;
    z-index: 99;

    .submitBox2 {
      width: 600px;
      max-height: 80%;
      background-color: #ffffff;
      border-radius: 8px;
      display: flex;
      flex-direction: column;

      .submitTitle {
        position: relative;

        >p {
          padding: 30px 0;
          border-bottom: 1px solid #dfdfdf;
          text-align: center;
          font-size: 18px;
          color: #333333;

        }

        >img {
          position: absolute;
          width: 20px;
          height: 20px;
          top: 19px;
          right: 21px;
          cursor: pointer;
        }
      }

      .content {
        display: flex;
        flex-direction: column;
        width: 100%;
        padding: 30px 30px;
        overflow-y: auto;

        >div {
          display: flex;
          align-items: center;
          margin-bottom: 30px;

          >p {
            margin-right: 28px;
          }

          .left-title {
            width: 115px;
            margin-right: 0;
            // padding-top: 19px;
            font-size: 15px;
            color: rgba(102, 102, 102, 1);
          }

          .left-msg {
            flex: 1;

            .el-select {
              width: 100%;
            }

            >input {
              width: 100%;
              height: 52px;
              border: 1px solid rgba(221, 221, 221, 1);
              padding-left: 13px;

            }

            >textarea {
              width: 100%;
              height: 165px;
              padding: 20px 13px;
              background: rgba(255, 255, 255, 1);
              border: 1px solid rgba(221, 221, 221, 1);
              resize: none;
            }
          }

          .imgBox {
            width: 59.5%;

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

                // width: 85px;
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
          .fileBox{
            flex: 1;
            .file{
              .uploadFile{
                position: relative;
                >div{
                  display: flex;
                  display: -webkit-flex;
                  color: #C6C6C6;
                  width: 100%;
                  height: 100%;
                  input{
                    // width: 100%;
                    // height: 100%;
                    opacity: 0;
                    position: absolute;
                    left: 0;
                    top: 0;
                  }
                }
              }
              .fileP{
                position: relative;
                >p{
                  cursor: pointer;
                  width: 100%;
                  max-width: 400px;
                  margin: 10px 0;
                  overflow: hidden;
                  white-space: nowrap;
                  text-overflow: ellipsis;
                }
                .delImg{
                  cursor: pointer;
                  position: absolute;
                  top: -7.5px;
                  right: -7.5px;
                  z-index: 2;
                  width: 15px;
                  height: 15px;
                }
              }
            }
          }
        }

        >button {
          width: 100%;
          height: 52px;
          background: rgba(39, 120, 190, 1);
          border: none;
          margin-top: 30px;
          font-size: 16px;
          color: rgba(255, 255, 255, 1);
          line-height: 52px;
        }
      }
    }
  }

  .info {
    display: flex;
    height: 150px;
    width: 100%;
    margin-bottom: 30px;

    img {
      width: 150px;
      height: 150px;
      margin-right: 19px;
    }

    div {
      width: 100%;
      padding-bottom: 5px;
      display: flex;
      flex-direction: column;
      justify-content: space-between;
    }
    .addNew {
      width: 90px;
      height: 30px;
      display: flex;
      align-items: center;
      justify-content: center;
      font-size: 16px;
      color: #fff;
      background-color: #2778BE;
      cursor: pointer;
      img {
        width: 15px;
        height: 15px;
        margin-right: 10px;
      }
    }
  }

  .submitBox {
    width: 100%;
    padding-top: 30px;
    // border-top: 1px solid rgba(0, 0, 0, 0.1);

    >p {
      // margin: 30px 30px 0;
      height: 40px;
      background: rgba(247, 247, 247, 1);
      border: 1px solid rgba(238, 238, 238, 1);
      // padding:14px 0 14px 28px;
      padding-left: 28px;
      font-size: 14px;
      font-weight: bold;
      color: #666666;
      line-height: 40px;
    }

    .content {
      display: flex;
      padding-bottom: 30px;
      width: 100%;

      >div {
        width: 50%;
        // padding-top: 30px;
      }

      .left {
        display: flex;
        flex-direction: column;

        >div {
          display: flex;
          // justify-content: center;
          align-items: center;
          margin-bottom: 28px;

          .left-title {
            // padding-top: 19px;
            font-size: 15px;
            color: rgba(102, 102, 102, 1);
          }

          .left-msg {
            width: 75%;

            >input {
              width: 100%;
              height: 52px;
              padding: 0 21px;
              background: rgba(255, 255, 255, 1);
              border: 1px solid rgba(221, 221, 221, 1);
              resize: none;
            }

            >button {
              width: 100%;
              height: 52px;
              background: rgba(39, 120, 190, 1);
              border: none;
              margin-top: 59px;
              font-size: 16px;
              color: rgba(255, 255, 255, 1);
              line-height: 52px;
            }
          }
        }
      }

      .right {
        display: flex;
        flex-direction: column;

        >div {
          display: flex;
          justify-content: center;
          // align-items: center;
          margin-bottom: 28px;

          >p {
            margin-top: 20px;
            margin-right: 28px;
          }

          .imgBox {
            width: 59.5%;

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

                // width: 85px;
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
        }
      }
    }
  }

  .certificate {
    .title {
      width: 100%;
      background: #F7F7F7;
      border: 1px solid #eeeeee;
      height: 40px;
      line-height: 40px;
      text-indent: 28px;
      color: #666666;
      margin: 30px 0;
      display: flex;
      align-items: center;
      justify-content: space-between;

      p {
        cursor: pointer;
        padding-right: 40px;

        span {
          color: @redColor;
        }
      }

      p.active {
        color: @themeColor;
        text-decoration: underline;
      }
    }

    ul {
      width: 100%;
      overflow: hidden;

      li {
        width: 23%;
        margin-right: 2%;
        // height: 150px;
        display: inline-block;
        margin-bottom: 20px;
        padding-bottom: 30px;

        &:nth-of-type(4n) {
          margin-right: 0;
        }

        .img {
          width: 100%;
          height: 70%;
          display: flex;
          display: -webkit-flex;
          position: relative;

          img {
            width: 100%;
            height: 100%;
            max-width: 100%;
            max-height: 100%;
            margin: auto;
          }

          .viewer {
            position: absolute;
            top: 0;
            left: 0;
            opacity: 0;
          }
        }

        p {
          line-height: 24px;
        }

        p:first-of-type {
          color: #666666;
          font-size: 14px;
        }

        p:nth-of-type(2) {
          font-size: 12px;
          color: #999999;
        }

        p.set {
          display: flex;
          justify-content: space-between;
          position: absolute;

          span {
            cursor: pointer;

            i {
              margin-right: 5px;
            }
          }

        }
        .doc{
          display: flex;
          flex-direction: column;
          width: 100%;
          p{
            margin-top: 10px;
            display: flex;
            flex-direction: column;
            align-items: center;
            img{
              width: 62px;
              height: 69px;
              margin-right: 3px;
              display: block;
              cursor: pointer;
              &:hover{
              }
            }
            span{
              // display: block;
              // width: 100%;
              // text-align: center;
              color: #0066cc;
              font-size: 13px;
              cursor: pointer;
              // margin-top: 5px;
            }
          }

        }
      }
    }
  }


  .shape {
    &:hover {
      cursor: pointer;
    }
  }
</style>
