<template>
<!--  建造流程-->
  <div style="margin-top: -30px;">
    <el-dialog
     title="新增"
     :visible.sync="postShow"
     width="30%"
     @close="closePost">
      <div class="postBox">
        <div class="list">
          <div class="listTitle">分类：</div>
          <div class="listContent">
            <el-select
              v-model="value1"
              clearable
              placeholder="请选择"
              class="select"
            >
              <el-option
                v-for="item in options"
                :key="item.cd"
                :label="item.nm"
                :value="item.cd"
              >
              </el-option>
            </el-select>
          </div>
        </div>
        <div class="list">
          <div class="listTitle">建造过程：</div>
          <div class="listContent" >
            <el-select
              v-model="value2"
              clearable
              multiple
              placeholder="请选择建造过程"
              class="select"
            >
              <el-option
                v-for="item in optionsTwo"
                :key="item.cd"
                :label="item.nm"
                :value="item.cd"
              >
              </el-option>
            </el-select>
          </div>
        </div>
        <div class="list">
          <div class="listTitle">标题：</div>
          <div class="listContent">
            <el-input
              placeholder="请输入标题"
              style="width: 454px"
              v-model="title"
            >
            </el-input>
          </div>
        </div>
        <div class="list">
          <div class="listTitle">建造单位：</div>
          <div class="listContent" >
            <el-select
              v-model="value3"
              clearable
              placeholder="请选择建造单位"
              class="select"
            >
              <el-option
                v-for="item in optionsThree"
                :key="item.cd"
                :label="item.nm"
                :value="item.cd"
              >
              </el-option>
            </el-select>
          </div>
        </div>
        <div class="list">
          <div class="listTitle">监理时间：</div>
          <div class="listContent">
            <el-date-picker
              class="select"
              v-model="pickDate"
              type="datetime"
              placeholder="选择日期时间"
              align="right"
            >
            </el-date-picker>
          </div>
        </div>
        <div class="list">
          <div class="listTitle">图片：</div>
          <div class="listContent">
            <el-form :model="form">
              <el-form-item>
                <el-upload
                  ref="upload"
                  action="/general/oss/upload"
                  accept="image/png,image/gif,image/jpg,image/jpeg"
                  list-type="picture-card"
                  :auto-upload="true"
                  :before-upload="handleBeforeUpload"
                  :on-preview="handlePictureCardPreview"
                  :on-remove="handleRemoveOne"
                  :on-success="imgSuccess"
                >
                  <i class="el-icon-plus"></i>
                </el-upload>
                <el-dialog :visible.sync="dialogVisible">
                  <img width="100%" :src="dialogImageUrl" alt="" />
                </el-dialog>
              </el-form-item>
            </el-form>
          </div>
        </div>
        <div class="list" style="align-items: flex-start">
          <div class="listTitle" style="line-height: 50px">上传文档：</div>
          <div class="listContent" style="border-bottom: 1px dotted #cccccc">
            <el-form :model="formTwo">
              <el-form-item>
                <el-upload
                  ref="uploadExcel"
                  action="/general/oss/upload"
                  :auto-upload="true"
                  :on-change="fileChange"
                  :on-success="handleSuccess"
                  :on-remove="handleRemoveTwo"
                  :on-error="handleError"
                  :on-preview="HandFilePreView"
                >
                  <el-button size="small" plain style="width: 100px; height: 50px"
                    >选择文件</el-button
                  >
                </el-upload>
              </el-form-item>
            </el-form>
          </div>
        </div>
        <div class="list" style="justify-content: center;">
          <button type="button" cover="point" @click="save" style=" cursor: pointer;
          width: 200px;
          height: 52px;
          background: #2778be;
          font-size: 16px;
          color: #ffffff;
          border: 0;">保存</button>
        </div>
      </div>
      </el-dialog>
    <div class="liuchengBox">
      <div class="topBox">
        <p v-for="(item,index) in options" :key="index" @click="checkIndex=index" :class="{clickP:checkIndex==index}">
          {{item.nm}}
        </p>
        <!-- <p style="position: absolute; right: 15%; color: red;" @click="postShow=true">新增</p> -->
      </div>
      <!-- 不是照片 -->
      <div class="bottomBox" v-if="checkIndex!=2">
        <div v-for="(item,index) in list" :key="index">
          <div class="title" @click="item.show=!item.show">

            <span>{{item.nm}}</span>
            <p>
              <span style="color: #2778BE;font-weight: bold">点击查看内容</span>
              <img :src="arrowDown" v-if="item.show">
              <img :src="arrowUp" v-else>
            </p>
          </div>



          <div class="rich" v-if="item.show">
            <p class="desc" v-show="item.imgList">
              {{item.nm}}报告：
            </p>
            <div class="report" v-viewer>
              <p v-for="j in item.imgList">
                <img :src="j.img">
              </p>
            </div>

            <p style="margin-top: 20px" v-show="item.fileList">附件：</p>
            <div class="doc">
              <p v-for="j in item.fileList" @click="toLink(j.url)">
                <img :src="j.img">
                <span>{{j.fileNm}}</span>
              </p>
            </div>
          </div>

        </div>
      </div>
      <!-- 照片 -->
      <div class="bottomBox" v-if="checkIndex==2">
        <div v-for="(item,index) in list" :key="index">
          <div class="title" @click="item.show=!item.show">
            <span>{{item.nm}}</span>
            <p>
              <span style="color: #2778BE;font-weight: bold">点击查看内容</span>
              <img :src="arrowDown" v-if="item.show">
              <img :src="arrowUp" v-else>
            </p>
          </div>



          <div class="rich" v-if="item.show">
            <p class="desc" v-show="item.imgList1">
              时间 : {{item.actDt}}
            </p>
            <div class="report" v-viewer>
              <p v-for="j in item.imgList1">
                <img :src="j">
              </p>
            </div>
          </div>

        </div>
      </div>
    </div>

  </div>

</template>

<script>
    import test1 from '../../assets/img/personal/test1.png'
    import arrowUp from '../../components/img/arrowUp.png'
    import arrowDown from '../../components/img/arrowDown.png'
    import excel from '@/assets/img/personal/excel.png'
    import ppt from '@/assets/img/personal/ppt.png'
    import word from '@/assets/img/personal/word.png'
    import pdf from '@/assets/img/personal/pdf.jpg'
    export default {
        name: "fangansheji",
        data(){
            return{
                test1,
                excel,
                ppt,
                word,
                pdf,
                arrowDown,
                arrowUp,
                currentRole:{},
                dialogImageUrl: "",
                dialogVisible: false,
                list:[],
                checkIndex:0,
                options:[],
                postShow:false,
                value1:'',
                value2:'',
                optionsTwo:[],
                title:'',
                value3:'',
                optionsThree:[],
                pickDate:'',
                form:{},
                imgInfo:[],
                formTwo:{
                  file:'',
                },
                fileInfoList:[],

            }
        },
        watch:{
          checkIndex(){
            this.getInfo()
          }
        },
        mounted() {
          this.id= this.until.getQueryString('id')
          this.getDic()

        },
        methods:{
          save(){

          },
          HandFilePreView(file) {},
          handleError(err, file, fileList) {
            this.$notify.error({
              title: "错误",
              message: `文件上传失败`,
            });
          },
          handleRemoveTwo(file, fileList) {
            this.fileInfoList=fileList
            this.fileInfoList.forEach(item=>{
              if(item.response){
                item.newFile=item.response.data
              }
             else{
               item.newFile=item.url
             }
            })
            this.fileInfoList =this.fileInfoList.map((item) => item.newFile).join(",");
          },
          fileChange(file, fileList) {
            this.formTwo.file = file.raw;
          },
          handleSuccess(res, file, fileList) {
            console.log(111,fileList);
            // this.fileInfoList = fileList.map((item) => item.response.data).join(",");
            this.fileInfoList=fileList
            this.fileInfoList.forEach(item=>{
              if(item.response){
                item.newFile=item.response.data
              }
             else{
               item.newFile=item.url
             }
            })
            this.fileInfoList =this.fileInfoList.map((item) => item.newFile).join(",");
          },
          imgSuccess(res, file, fileList) {
            // this.imgInfo = fileList.map((item) => item.response.data).join(",");
            this.imgInfo=fileList
            this.imgInfo.forEach(item=>{
              if(item.response){
                item.newImg=item.response.data
              }
             else{
               item.newImg=item.url
             }
            })
            this.imgInfo =this.imgInfo.map((item) => item.newImg).join(",");
            console.log(123, this.imgInfo);
          },
         handleRemoveOne(file, fileList) {
          this.imgInfo=fileList
          this.imgInfo.forEach(item=>{
            if(item.response){
              item.newImg=item.response.data
            }
           else{
             item.newImg=item.url
           }
          })
          this.imgInfo =this.imgInfo.map((item) => item.newImg).join(",");
         },
         handlePictureCardPreview(file) {
           this.dialogImageUrl = file.url;
           this.dialogVisible = true;
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
          closePost(){

          },
          async getDic(){
            this.options=await this.api.dataDictionary('DOCS_BUILD_SHIP_CAT')
            this.optionsTwo=this.options
            this.optionsThree=this.options
            // console.log('获取数据字典')
            // console.log(this.options)
            this.getInfo()
          },
          async getInfo(){
            this.list=[]
            this.currentRole=JSON.parse(this.until.seGet('currentRole'))
            let qry = this.query.new()
            this.query.toW(qry,'docsId',this.id,'EQ')
            this.query.toW(qry,'catCd',this.options[this.checkIndex].cd,'EQ')
            let data=await this.api.buildStep(this.query.toEncode(qry))
            let data1=[]
            if(data.length>0){
              data.forEach((item,index)=>{
                if(index==0){
                  item.show=true
                }else{
                  item.show=false
                }
                if(this.options[this.checkIndex].cd!='DOCS_BUILD_SHIP_CAT.30'){
                  let imgList1=item.imgUrl ? item.imgUrl.split(',') : []
                  let imgList2=[]
                  imgList1.forEach(v=>{
                    imgList2.push({url:v,img:v})
                  })
                  item.imgList=imgList2

                  let fileList1=item.fileUrl?item.fileUrl.split(','):[]
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
                }else{
                  item.imgList1=item.imgUrl ? item.imgUrl.split(',') : []
                  if(data1.length==0){
                    console.log(item)
                    data1[0]=item
                  }else{
                    if(item.actDt==data1[data1.length-1].actDt){
                      data1[data1.length-1].imgList1.push(...item.imgList1)
                    }else{
                      data1.push(item)
                    }
                  }
                }
              })
            }
            if(this.options[this.checkIndex].cd!='DOCS_BUILD_SHIP_CAT.30'){
              this.list=data
            }else{
              this.list=data1
            }

            console.log(this.list)
          },
          toLink(url){
            console.log(url)
            window.open(url)
          },
        }


    }
</script>

<style scoped lang="less">
  /deep/ .el-dialog{
        background-color: #FFFFFF !important;
        width:50% !important;
        .list {
          display: flex;
          align-items: center;
          margin-bottom: 30px;
          margin-left: 3%;
          .listTitle {
            width: 160px;
            font-size: 15px;
            color: #666666;

          }

          .listContent {
            margin-left: 20px;
            position: relative;
            width: 500px !important;
            .el-input{
              width: 100% !important;
            }
            .select {
              width: 100% !important;
            }

            textarea {
              width:  100% !important;
              height: 152px;
              border: 1px solid #dddddd;
              padding: 20px;
              resize: none;
            }

            textarea::placeholder {
              font-size: 14px;
              color: rgb(192, 196, 204);
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
 .postBox {
    padding: 30px 0;

    .list {
      display: flex;
      align-items: center;
      margin-bottom: 30px;

      .listTitle {
        width: 100px;
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

        textarea {
          width: 454px;
          height: 152px;
          border: 1px solid #dddddd;
          padding: 20px;
          resize: none;
        }

        textarea::placeholder {
          font-size: 14px;
          color: rgb(192, 196, 204);
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
  .title{
    margin-bottom: 10px;
    width: 100%;
    height: 40px;
    background: #EEEEEE;
    color: #666666;
    padding:0 28px;
    display: flex;
    align-items: center;
    justify-content: space-between;

    &:hover{
      cursor: pointer;
    }
    p:nth-of-type(1){
      display: flex;
      align-items: center;
      img{
        width: 12px;
        margin-left: 25px;
      }
    }
  }

  .liuchengBox{
    display: flex;
    flex-direction: column;
    .topBox{
      width: 100%;
      margin-bottom: 30px;
      // background-color: #f3f3f3;
      display: flex;
      align-items: center;
      border-bottom: 1px solid rgba(0,0,0,0.1);
      overflow-x: auto;
      >p{
        font-size: 14px;
        color: #666666;
        padding: 14px 20px;
        cursor: pointer;
        white-space: nowrap;
      }
      .clickP{
        font-size: 14px;
        color: #303030;
        font-weight: 600;
        border-bottom: 2px solid #2778BE;

      }
    }
    .bottomBox{
      // flex: 4;
    }
  }
  .rich{

      padding: 24px 28px;
      .desc{
        color: #666666;

      }
      .report{
        display: flex;
        flex-wrap: wrap;
        width: 100%;
        p{
          width: 180px;
          height: 120px;
          margin: 10px 10px 0 0;
          img{
            display: block;
            width: 180px;
            height: 120px;
            object-fit: cover;
            margin: 0 auto;
          }
        }
      }
      .doc{
        display: flex;
        // flex-direction: column;
        flex-wrap: wrap;
        width: 100%;
        p{
          margin-top: 10px;
          width: 16.6%;
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
</style>
