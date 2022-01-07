<template>
  <div class="certificate">
    <div class="main-title">
      <div>
        <div></div>
        <!-- <img style="cursor: pointer;" :src="left" @click="lastPageNum"> -->
        <span>船舶证书</span>
        <div></div>
        <!-- <img style="cursor: pointer;" :src="right" @click="nextPageNum"> -->
      </div>
      <img style="width: 100%;" :src="borderImg">
    </div>
    <div v-for="(item, index) in list1" :key="index">
      <div class="title" @click="toOpen(index)">
        <div>
          <span style="margin-right: 30px">{{ item.nm }}</span>
          <span>(证件有效期:{{ item.validUntil?item.validUntil.substring(0,10):'' }})</span>
        </div>
        <p>
          <img :src="arrowDown" v-if="item.show" />
          <img :src="arrowUp" v-else />
        </p>
      </div>
      <div class="rich" v-show="item.show">
        <!-- <div class="imgBox" v-viewer>
          <p v-for="j in item.imgList">
            <img :src="j" />
          </p>
        </div> -->
        <div class="doc">
          <p v-for="j in item.fileList">
            <img :src="j.img" @click="toLink(j.url)" />
            <span @click="toLink(j.url)">{{ j.fileNm }}</span>
          </p>
        </div>
      </div>
    </div>
    <div class="pagination">
      <el-pagination
        small
        @current-change="handleCurrentChange1"
        :current-page.sync="pageNum"
        :page-size="pageSize"
        layout="prev, pager, next"
        :total="total">
      </el-pagination>
    </div>

    <div class="main-title" style="margin-top: 40px;">
      <div>
        <!-- <img style="cursor: pointer;" :src="left" @click="lastPageNum2"> -->
        <div></div>
        <span>设备证书</span>
        <div class="addNew" @click="addOpen" v-if="formW=='jiance'&&shipStatus!='已交船'">
          <img :src="addLogo">新增
        </div>
        <div v-else></div>
        <!-- <img style="cursor: pointer;" :src="right" @click="nextPageNum2"> -->
      </div>
      <img style="width: 100%;" :src="borderImg">
    </div>
    <div v-for="(item, index) in list2" :key="index">
      <div class="title" @click="toOpen2(index)">
        <div>
          <span style="margin-right: 30px">{{ item.nm }}</span>
          <span>(证件有效期:{{ item.validUntil?item.validUntil.substring(0,10):'' }})</span>
        </div>
        <p>
          <span v-if="formW=='jiance'&&item.audit!=2" style="color: red;margin-right: 10px;" @click.stop="del(item)">删除</span>
          <span v-if="formW=='jiance'&&item.audit!=2" style="color: red;margin-right: 10px;" @click.stop="change(item)">修改</span>
          <span v-if="formW=='jiance'&&item.audit==2" style="color: rgb(39, 120, 190);margin-right: 10px;">审核通过</span>
          <span v-if="formW=='jiance'&&item.audit==3" style="color: red;margin-right: 10px;">审核未通过</span>
          <span v-if="formW=='jiance'&&item.audit==1" style="margin-right: 10px;">未审核</span>
          <img :src="arrowDown" v-if="item.show" />
          <img :src="arrowUp" v-else />
        </p>
      </div>
      <div class="rich" v-show="item.show">
        <!-- <div class="imgBox" v-viewer>
          <p v-for="j in item.imgList">
            <img :src="j" />
          </p>
        </div> -->
        <div class="doc">
          <p v-for="j in item.fileList">
            <img :src="j.img" @click="toLink(j.url)" />
            <span @click="toLink(j.url)">{{ j.fileNm }}</span>
          </p>
        </div>
      </div>
    </div>
    <div class="pagination">
      <el-pagination
        small
        @current-change="handleCurrentChange2"
        :current-page.sync="pageNum2"
        :page-size="pageSize"
        layout="prev, pager, next"
        :total="total2">
      </el-pagination>
    </div>

    <div class="main-title" style="margin-top: 40px;">
      <div>
        <div></div>
        <!-- <img style="cursor: pointer;" :src="left" @click="lastPageNum"> -->
        <span>产品说明书</span>
        <div></div>
        <!-- <img style="cursor: pointer;" :src="right" @click="nextPageNum"> -->
      </div>
      <img style="width: 100%;" :src="borderImg">
    </div>
    <div v-for="(item, index) in list3" :key="index">
      <div class="title" @click="toOpen3(index)">
        <div>
          <span style="margin-right: 30px">{{ item.nm }}</span>
          <span>(证件有效期:{{ item.validUntil?item.validUntil.substring(0,10):'' }})</span>
        </div>
        <p>
          <img :src="arrowDown" v-if="item.show" />
          <img :src="arrowUp" v-else />
        </p>
      </div>
      <div class="rich" v-show="item.show">
        <!-- <div class="imgBox" v-viewer>
          <p v-for="j in item.imgList">
            <img :src="j" />
          </p>
        </div> -->
        <div class="doc">
          <p v-for="j in item.fileList">
            <img :src="j.img" @click="toLink(j.url)" />
            <span @click="toLink(j.url)">{{ j.fileNm }}</span>
          </p>
        </div>
      </div>
    </div>
    <div class="pagination">
      <el-pagination
        small
        @current-change="handleCurrentChange3"
        :current-page.sync="pageNum3"
        :page-size="pageSize"
        layout="prev, pager, next"
        :total="total3">
      </el-pagination>
    </div>

    <el-dialog :title="title" :visible.sync="submitShow" @close="closePost">
      <div class="postBox">
        <div class="list">
          <div class="listTitle">报告名称：</div>
          <div class="listContent">
            <el-input placeholder="请输入报告名称" style="width: 454px" v-model="form.nm">
            </el-input>
          </div>
        </div>
        <div class="list">
          <div class="listTitle">报告出具时间：</div>
          <div class="listContent">
            <el-date-picker class="select" v-model="time" type="date" placeholder="选择日期" align="right">
            </el-date-picker>
          </div>
        </div>
        <!-- <div class="list">
          <div class="listTitle">上传图片：</div>
          <div class="listContent">
            <el-form :model="formOne">
              <el-form-item>
                <el-upload ref="upload" action="/general/oss/upload" accept="image/png,image/gif,image/jpg,image/jpeg"
                  list-type="picture-card" :auto-upload="true" :before-upload="handleBeforeUpload"
                  :on-preview="handlePictureCardPreview" :on-remove="handleRemoveOne" :on-success="imgSuccess"  :file-list="imginfo" >
                  <i class="el-icon-plus"></i>
                </el-upload>
                <el-dialog :visible.sync="dialogVisible">
                  <img width="100%" :src="dialogImageUrl" alt="" />
                </el-dialog>
              </el-form-item>
            </el-form>
          </div>
        </div> -->
        <div class="list" style="align-items: flex-start">
          <div class="listTitle" style="line-height: 50px">上传附件：</div>
          <div class="listContent" style="border-bottom: 1px dotted #cccccc">
            <el-form :model="formTwo">
              <el-form-item>
                <el-upload ref="uploadExcel" action="/general/oss/upload" :auto-upload="true" :on-change="fileChange"
                  :on-success="handleSuccess" :on-remove="handleRemoveTwo" :on-error="handleError"
                  :on-preview="HandFilePreView" :file-list="fileinfo">
                  <el-button size="small" plain style="width: 100px; height: 50px">选择文件</el-button>
                </el-upload>
              </el-form-item>
            </el-form>
          </div>
        </div>
        <div class="list" style="justify-content: center;">
          <button type="button" cover="point" @click="submit" style=" cursor: pointer;
          width: 200px;
          height: 52px;
          background: #2778be;
          font-size: 16px;
          color: #ffffff;
          border: 0;">{{title}}</button>
        </div>
      </div>
    </el-dialog>
  </div>
</template>

<script>
  import {
    mapState
  } from "vuex";
  import arrowUp from "../../components/img/arrowUp.png";
  import arrowDown from "../../components/img/arrowDown.png";
  import excel from "@/assets/img/personal/excel.png";
  import ppt from "@/assets/img/personal/ppt.png";
  import word from "@/assets/img/personal/word.png";
  import pdf from "@/assets/img/personal/pdf.jpg";
  import left from "@/assets/img/onlineTrade/left.png";
  import right from "@/assets/img/onlineTrade/right.png";
  import borderImg from "@/assets/img/onlineTrade/分割线.png";
  import addLogo from "@/assets/img/onlineTrade/新增.png";
  import close from "@/assets/img/close.png"
  export default {
    props: ['formW'],
    name: "productList",
    data() {
      return {
        excel,
        ppt,
        word,
        pdf,
        arrowDown,
        arrowUp,
        left,
        right,
        borderImg,
        addLogo,
        close,
        title: '',
        shipStatus: '',
        list1: [],
        list2: [],
        list3: [],
        certificateList: [],
        certificateList2: [],
        certificateList3: [],
        imgList: '', //用于上传时的图片字符串
        fileList: '', //用于上传时的文件字符串
        submitShow: false,
        time: '',
        form: {
          nm: "",
          docsId: "",
          validUntil: "",
          imgUrl: "",
          attachment: '',
          issuerId: "",
          issuerNm: "",
          types: "1", //0 船舶检验证书（后台上传） 1设备及其他证书 2检测报告
        },
        currentRole: {},
        pageNum: 1,
        pageNum2: 1,
        pageNum3: 1,
        pageSize: 10,
        total: 0,
        total2: 0,
        total3: 0,
        dialogVisible: false,
        dialogImageUrl: '',
        formOne: {},
        formTwo: {
          file: "",
        },
        imginfo: [], //用于关闭弹窗清空选择图片
        fileinfo: [], //用于关闭弹窗清空选择文件
        listFile: [], //用于回显零时数组
      };
    },
    computed: {
      ...mapState(["bWidth", "width"]),
    },
    watch: {},
    mounted() {
      this.id = this.until.getQueryString("id");
      this.currentRole = JSON.parse(this.until.seGet('currentRole'))
      this.shipStatus = this.until.getQueryString('shipStatus')
      this.getInfo()
      if (this.formW == 'jiance') {
        this.getInfo2()
      } else {
        this.getInfo3()
      }
      this.getInfo4()
    },
    methods: {
      toLink(url) {
        window.open(url);
      },
      toOpen(index) {
        this.list1[index].show = !this.list1[index].show;
        this.$set(this.list1, index, this.list1[index]);
      },
      toOpen2(index) {
        this.list2[index].show = !this.list2[index].show;
        this.$set(this.list2, index, this.list2[index]);
      },
      toOpen3(index) {
        this.list3[index].show = !this.list3[index].show;
        this.$set(this.list3, index, this.list3[index]);
      },
      async getInfo() {
        this.certificateList = [];
        this.list1 = []
        let qry = this.query.new();
        this.query.toW(qry, "docsId", this.id, "EQ");
        this.query.toW(qry, "types", "0", "EQ");
        this.query.toP(qry, this.pageNum, this.pageSize);
        let data = await this.api.jianyanList2(
          this.query.toEncode(qry)
        );
        this.certificateList = data.data.list
        this.total = data.page.total
        this.certificateList.forEach((item,index) => {
          item.imgList = item.imgUrl ? item.imgUrl.split(",") : [];
          let fileList1 = item.attachment ? item.attachment.split(",") : [];
          let fileList2 = [];
          fileList1.forEach((v) => {
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
                img: this.pdf,
                fileNm: nm
              });
            } else if (type == "doc" || type == "docx") {
              fileList2.push({
                url: v,
                img: this.word,
                fileNm: nm
              });
            } else if (type == "ppt" || type == "pptx") {
              fileList2.push({
                url: v,
                img: this.ppt,
                fileNm: nm
              });
            } else if (type == "xls" || type == "xlsx") {
              fileList2.push({
                url: v,
                img: this.excel,
                fileNm: nm
              });
            } else {
              fileList2.push({
                url: v,
                img: v,
                fileNm: nm
              });
            }
            item.fileList = fileList2;
          });
          if(index==0) {
            item.show = true
          } else {
            item.show = false
          }
          this.list1.push(item);
        });
      },
      async getInfo2() {
        this.certificateList2 = [];
        this.list2 = []
        let qry = this.query.new();
        this.query.toW(qry, "docsId", this.id, "EQ");
        this.query.toW(qry, "types", "1", "EQ");
        this.query.toP(qry, this.pageNum2, this.pageSize);
        let data = await this.api.jianyanListAll2(
          this.query.toEncode(qry)
        );
        this.certificateList2 = data.data.list
        this.total2 = data.page.total
        this.certificateList2.forEach((item,index) => {
          item.imgList = item.imgUrl ? item.imgUrl.split(",") : [];
          let fileList1 = item.attachment ? item.attachment.split(",") : [];
          let fileList2 = [];
          fileList1.forEach((v) => {
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
                img: this.pdf,
                fileNm: nm
              });
            } else if (type == "doc" || type == "docx") {
              fileList2.push({
                url: v,
                img: this.word,
                fileNm: nm
              });
            } else if (type == "ppt" || type == "pptx") {
              fileList2.push({
                url: v,
                img: this.ppt,
                fileNm: nm
              });
            } else if (type == "xls" || type == "xlsx") {
              fileList2.push({
                url: v,
                img: this.excel,
                fileNm: nm
              });
            } else {
              fileList2.push({
                url: v,
                img: v,
                fileNm: nm
              });
            }
            item.fileList = fileList2;
          });
          if(index==0) {
            item.show = true
          } else {
            item.show = false
          }
          this.list2.push(item);
        });
      },
      async getInfo3() {
        this.certificateList2 = [];
        this.list2 = []
        let qry = this.query.new();
        this.query.toW(qry, "docsId", this.id, "EQ");
        this.query.toW(qry, "types", "1", "EQ");
        this.query.toP(qry, this.pageNum2, this.pageSize);
        let data = await this.api.jianyanList2(
          this.query.toEncode(qry)
        );
        this.certificateList2 = data.data.list
        this.total2 = data.page.total
        this.certificateList2.forEach((item,index) => {
          item.imgList = item.imgUrl ? item.imgUrl.split(",") : [];
          let fileList1 = item.attachment ? item.attachment.split(",") : [];
          let fileList2 = [];
          fileList1.forEach((v) => {
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
                img: this.pdf,
                fileNm: nm
              });
            } else if (type == "doc" || type == "docx") {
              fileList2.push({
                url: v,
                img: this.word,
                fileNm: nm
              });
            } else if (type == "ppt" || type == "pptx") {
              fileList2.push({
                url: v,
                img: this.ppt,
                fileNm: nm
              });
            } else if (type == "xls" || type == "xlsx") {
              fileList2.push({
                url: v,
                img: this.excel,
                fileNm: nm
              });
            } else {
              fileList2.push({
                url: v,
                img: v,
                fileNm: nm
              });
            }
            item.fileList = fileList2;
          });
          if(index==0) {
            item.show = true
          } else {
            item.show = false
          }
          this.list2.push(item);
        });
      },
      async getInfo4() {
        this.certificateList3 = [];
        this.list3 = []
        let qry = this.query.new();
        this.query.toW(qry, "docsId", this.id, "EQ");
        this.query.toW(qry, "types", "3", "EQ");
        this.query.toP(qry, this.pageNum, this.pageSize);
        let data = await this.api.jianyanList2(
          this.query.toEncode(qry)
        );
        this.certificateList3 = data.data.list
        this.total3 = data.page.total
        this.certificateList3.forEach((item,index) => {
          item.imgList = item.imgUrl ? item.imgUrl.split(",") : [];
          let fileList1 = item.attachment ? item.attachment.split(",") : [];
          let fileList2 = [];
          fileList1.forEach((v) => {
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
                img: this.pdf,
                fileNm: nm
              });
            } else if (type == "doc" || type == "docx") {
              fileList2.push({
                url: v,
                img: this.word,
                fileNm: nm
              });
            } else if (type == "ppt" || type == "pptx") {
              fileList2.push({
                url: v,
                img: this.ppt,
                fileNm: nm
              });
            } else if (type == "xls" || type == "xlsx") {
              fileList2.push({
                url: v,
                img: this.excel,
                fileNm: nm
              });
            } else {
              fileList2.push({
                url: v,
                img: v,
                fileNm: nm
              });
            }
            item.fileList = fileList2;
          });
          if(index==0) {
            item.show = true
          } else {
            item.show = false
          }
          this.list3.push(item);
        });
      },
      addOpen() {
        this.submitShow = true
        this.title = '新增'
      },
      closePost() {
        this.imgList = ''
        this.fileList = ''
        this.imginfo = []
        this.fileinfo = []
        this.time = ''
        this.form = {
          id: "",
          nm: "",
          docsId: "",
          validUntil: "",
          imgUrl: "",
          attachment: '',
          issuerId: "",
          issuerNm: "",
          types: "1", //0 船舶检验证书（后台上传） 1设备及其他证书 2检测报告
        }

      },
      handleBeforeUpload(file) {
        console.log("before");
        if (
          !(
            file.type === "image/png" ||
            file.type === "image/gif" ||
            file.type === "image/jpg" ||
            file.type === "image/jpeg"
          )
        ) {
          this.$notify.warning({
            title: "警告",
            message: "请上传格式为image/png, image/gif, image/jpg, image/jpeg的图片",
          });
        }
        let size = file.size / 1024 / 1024 / 2;
        if (size > 2) {
          this.$notify.warning({
            title: "警告",
            message: "图片大小必须小于2M",
          });
        }
      },
      handlePictureCardPreview(file) {
        this.dialogImageUrl = file.url;
        this.dialogVisible = true;
      },
      handleRemoveOne(file, fileList) {
        this.imgList = fileList
        this.imgList.forEach(item => {
          if (item.response) {
            item.newImg = item.response.data
          } else {
            item.newImg = item.url
          }
        })
        this.imgList = this.imgList.map((item) => item.newImg).join(",");
      },
      imgSuccess(res, file, fileList) {
        this.imgList=fileList
        this.imgList.forEach(item=>{
          if(item.response){
            item.newImg=item.response.data
          }
         else{
           item.newImg=item.url
         }
        })
        this.imgList =this.imgList.map((item) => item.newImg).join(",");
        console.log(123, this.imgList);
      },
      fileChange(file, fileList) {
        this.formTwo.file = file.raw;
      },
      handleSuccess(res, file, fileList) {
        console.log(111,fileList);
        // this.fileInfoList = fileList.map((item) => item.response.data).join(",");
        this.fileList=fileList
        this.fileList.forEach(item=>{
          if(item.response){
            item.newFile=item.response.data
          }
         else{
           item.newFile=item.url
         }
        })
        this.fileList =this.fileList.map((item) => item.newFile).join(",");
      },
      handleRemoveTwo(file, fileList) {
        this.fileList=fileList
        this.fileList.forEach(item=>{
          if(item.response){
            item.newFile=item.response.data
          }
         else{
           item.newFile=item.url
         }
        })
        this.fileList =this.fileList.map((item) => item.newFile).join(",");
      },
      handleError(err, file, fileList) {
        this.$notify.error({
          title: "错误",
          message: `文件上传失败`,
        });
      },
      HandFilePreView(file) {},
      lastPageNum() {
        if(this.pageNum==1) {
          return
        } else {
          this.pageNum--
          this.getInfo()
        }
      },
      nextPageNum() {
        if(this.pageNum*this.pageSize>=this.total) {
          return
        } else {
          this.pageNum++
          this.getInfo()
        }
      },
      lastPageNum2() {
        if(this.pageNum2==1) {
          return
        } else {
          this.pageNum2--
          if (this.formW == 'jiance') {
            this.getInfo2()
          } else {
            this.getInfo3()
          }
        }
      },
      nextPageNum2() {
        if(this.pageNum2*this.pageSize>=this.total2) {
          return
        } else {
          this.pageNum2++
          if (this.formW == 'jiance') {
            this.getInfo2()
          } else {
            this.getInfo3()
          }
        }
      },
      submit() {
        this.form.docsId=this.id
        this.form.issuerId=this.currentRole.id
        this.form.issuerNm=this.currentRole.company
        if(!this.form.nm){
          this.$message({
            message: '请输入名称',
            type: 'warning',
            duration:'1500'
          });
          return
        }
        if(!this.time){
          this.$message({
            message: '请选择有效日期',
            type: 'warning',
            duration:'1500'
          });
          return
        }
        this.form.validUntil = this.until.formatDate2(new Date(this.time).getTime())
        this.form.imgUrl = this.imgList
        this.form.attachment = this.fileList
        if(this.form.id) {
          this.api.inspectUpd(this.form).then((res)=>{
            if(res.code==0){
              this.$message({
                message: '修改成功',
                type: 'success',
                duration:'1000'
              });
              setTimeout(()=>{
                this.closePost()
                this.submitShow = false
                this.getInfo2()
              },1000)
            }else{
              this.$message({
                message: res.msg,
                type: 'warning',
                duration:'1000'
              });
            }
          })
        } else {
          this.api.inspectAdd(this.form).then((res)=>{
            if(res.code==0){
              this.$message({
                message: '新增成功',
                type: 'success',
                duration:'1000'
              });
              setTimeout(()=>{
                this.closePost()
                this.submitShow = false
                this.getInfo2()
              },1000)
            }else{
              this.$message({
                message: res.msg,
                type: 'warning',
                duration:'1000'
              });
            }
          })
        }
      },
      change(item) {
        console.log(item)
        this.submitShow = true
        this.title = '修改'
        this.form.id = item.id
        this.time = item.validUntil?item.validUntil.substring(0,10):''
        this.form.nm = item.nm
        this.imgList = item.imgUrl
        this.fileList = item.attachment
        if(item.imgUrl) {
          let urlStr=item.imgUrl.split(",")
          urlStr.forEach(item1=> {
            let obj = new Object();
            obj.url = item1;
            this.imginfo.push(obj)
          });
        }
        if(item.attachment) {
          let modelList = item.attachment.split(',')
          this.getFile(modelList)
          if (this.listFile.length>0) {
            for (let i = 0; i < this.listFile.length; i++) {
              this.fileinfo.push({
                name: this.listFile[i].fileNm,
                url: this.listFile[i].url
              })
            }
          }
        }
      },
      async getFile(info) {
      	this.listFile = []
      	let data = info
      	let data1 = []
      	let fileList2 = []
      	if (data.length > 0) {
      		data.forEach(v => {
      			let type = v.split('.')[v.split('.').length - 1]
      			let nmList = v.split('.com/') //分割出url后的内容
      			let nm = ""
      			nmList.forEach((j, z) => { //防止文件名中有 .com/ 所以循环加入
      				if (z != 0) {
      					nm += j
      				}
      			})
      			nmList = nm.split('_') //分割随机字符后的内容
      			nm = ""
      			nmList.forEach((j, z) => { //防止文件名中有 _ 所以循环
      				if (z != 0) {
      					nm += j
      				}
      			})
      			// nm = nm.split('.' + type)[0]
      			if (type == 'pdf') {
      				fileList2.push({
      					url: v,
      					'fileNm': nm
      				})
      			} else if (type == 'doc' || type == 'docx') {
      				fileList2.push({
      					url: v,
      					'fileNm': nm
      				})
      			} else if (type == 'ppt' || type == 'pptx') {
      				fileList2.push({
      					url: v,
      					'fileNm': nm
      				})
      			} else if (type == 'xls' || type == 'xlsx') {
      				fileList2.push({
      					url: v,
      					'fileNm': nm
      				})
      			} else {
      				fileList2.push({
      					url: v,
      					img: v,
      					'fileNm': nm
      				})
      			}

      		})
      	}
      	this.listFile = fileList2
      	console.log('151', this.listFile)
      },
      del(item) {
        this.$confirm('确定删除?', '提示', {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning'
        }).then(() => {
          this.api.inspectDel(item.id).then(res=>{
            this.$message({
              type: 'success',
              message: '删除成功!'
            });
            this.getInfo2()
          })
        }).catch(() => {

        });
      },
      handleCurrentChange1(val) {
        this.pageNum = val
        this.getInfo()
      },
      handleCurrentChange2(val) {
        this.pageNum2 = val
        if (this.formW == 'jiance') {
          this.getInfo2()
        } else {
          this.getInfo3()
        }
      },
      handleCurrentChange3(val) {
        this.pageNum3 = val
        this.getInfo4()
      }
    },
  };
</script>

<style lang="less" scoped>
  @import url("../../assets/css/init.less");

  /deep/ .el-dialog {
    background-color: #FFFFFF !important;
    width: 50% !important;

    .list {
      display: flex;
      align-items: center;
      margin-bottom: 30px;
      margin-left: 3%;

      .listTitle {
        width: 200px;
        font-size: 15px;
        color: #666666;

      }

      .listContent {
        margin-left: 20px;
        position: relative;
        width: 500px !important;

        .el-input {
          width: 100% !important;
        }

        .select {
          width: 100% !important;
        }

        button {
          cursor: pointer;
          width: 291px;
          height: 52px;
          background: #2778be;
          font-size: 16px;
          color: #ffffff;
          border: 0;
        }
      }
    }
  }

  .certificate {
    .main-title {
      div {
        display: flex;
        align-items: center;
        justify-content: space-between;
        font-size: 16px;
        color: #303030;
        line-height: 24px;
        font-weight: bold;
        padding: 0 15px;
        box-sizing: border-box;
        .addNew {
          width: 90px;
          height: 30px;
          background-color: #2778BE;
          display: flex;
          align-items: center;
          justify-content: center;
          font-size: 16px;
          color: #fff;
          // margin: 20px auto;
          cursor: pointer;

          img {
            width: 15px;
            height: 15px;
            margin-right: 10px;
          }
        }
        span {
          margin-left: 10px;
        }
      }

    }

    .title {
      margin-bottom: 10px;
      width: 100%;
      height: 40px;
      background: #eeeeee;
      color: #666666;
      padding: 0 28px;
      display: flex;
      align-items: center;
      justify-content: space-between;

      &:hover {
        cursor: pointer;
      }

      p:nth-of-type(1) {
        display: flex;
        align-items: center;

        img {
          width: 12px;
          margin-left: 25px;
        }
      }
    }

    .rich {
      .imgBox {
        display: flex;
        flex-wrap: wrap;

        >p {
          width: calc((100% - 50px)/6);
          margin-right: 10px;
          &:nth-of-type(6n) {
            margin-right: 0;
          }
        }
      }

      .doc {
        display: flex;
        flex-wrap: wrap;
        width: 100%;

        p {
          margin-top: 10px;
          width: calc((100% - 50px)/6);
          margin-right: 10px;
          display: flex;
          flex-direction: column;
          align-items: center;
          &:nth-of-type(6n) {
            margin-right: 0;
          }
          img {
            width: 62px;
            height: 69px;
            margin-right: 3px;
            display: block;
            cursor: pointer;

            &:hover {}
          }

          span {
            color: #0066cc;
            font-size: 13px;
            cursor: pointer;
            word-break: break-all;
          }
        }
      }
    }

    .postBox {
      padding: 30px 0;

      .list {
        display: flex;
        align-items: center;
        margin-bottom: 30px;

        .listTitle {
          width: 160px;
          font-size: 15px;
          color: #666666;
        }

        .listContent {
          margin-left: 20px;
          position: relative;
          width: 900px;

          .select {
            width: 454px;
          }

          button {
            cursor: pointer;
            width: 291px;
            height: 52px;
            background: #2778be;
            font-size: 16px;
            color: #ffffff;
            border: 0;
          }
        }
      }
    }

    .pagination {
      display: flex;
      justify-content: flex-end;
    }
  }
</style>
