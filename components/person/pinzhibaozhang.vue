<template>
  <!--  品质保障-->

  <div>
    <!--    船舶信息-->
    <el-dialog
      title="信息修改"
      :visible.sync="maskShow"
      width="30%"
      @close="closeMask">
      <div class="postBox">
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
          <div class="listTitle">类别：</div>
          <div class="listContent">
            <el-select
              v-model="value1"
              clearable
              placeholder="请选择"
              class="select"
            >
              <el-option
                v-for="item in options"
                :key="item.nm"
                :label="item.nm"
                :value="item.cd"
              >
              </el-option>
            </el-select>
          </div>
        </div>
        <div class="list">
          <div class="listTitle">检验状态：</div>
          <div class="listContent">
            <el-select
              v-model="value2"
              clearable
              placeholder="请选择"
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
          <div class="listTitle">过程说明：</div>
          <div class="listContent">
            <textarea
              rows=""
              cols=""
              placeholder="多行输入"
              v-model="describe"
            ></textarea>
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
              :picker-options="pickerOptions"
            >
            </el-date-picker>
          </div>
        </div>
        <div class="list">
          <div class="listTitle">上传图片：</div>
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
                  :file-list="picList"
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
                  :file-list="fileInfo"
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
        <div class="list" style="align-items: flex-start">
          <div class="listTitle" style="line-height: 50px">上传附件：</div>
          <div class="listContent" style="border-bottom: 1px dotted #cccccc">
            <el-form :model="formThree">
              <el-form-item>
                <el-upload
                  ref="uploadExcelTwo"
                  action="/general/oss/upload"
                  :auto-upload="true"
                  :on-change="fileChangeTwo"
                  :on-success="handleSuccessTwo"
                  :on-remove="handleRemoveThree"
                  :on-error="handleErrorTwo"
                  :file-list="fileInfoTwo"
                  :on-preview="HandFilePreViewTwo"
                >
                  <el-button size="small" plain style="width: 100px; height: 50px"
                    >选择附件</el-button
                  >
                </el-upload>
              </el-form-item>
            </el-form>
          </div>
        </div>

        <div class="list">
          <div class="listTitle">备注：</div>
          <div class="listContent">
            <textarea
              rows=""
              cols=""
              placeholder="多行输入"
              v-model="rmks"
              style="height: 100px"
            ></textarea>
          </div>
        </div>

      </div>
      <span slot="footer" class="dialog-footer">
        <el-button @click="cancel">取 消</el-button>
        <el-button type="primary" @click="modify">确 定</el-button>
      </span>
    </el-dialog>
    <div class="info" v-if="!ifProDetail">
      <img :src="info.logo" />
      <div>
        <p>
          船舶编号:<span>{{ shipCd }}</span>
        </p>
        <p>
          监理方:<span>{{ info.company }}</span>
        </p>
        <p>
          船舶状态:<span style="color: #2778be">{{ shipStatus }}</span>
        </p>
      </div>
    </div>
    <div class="postBox">
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
        <div class="listTitle">类别：</div>
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
        <div class="listTitle">检验状态：</div>
        <div class="listContent">
          <el-select
            v-model="value2"
            clearable
            placeholder="请选择"
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
        <div class="listTitle">过程说明：</div>
        <div class="listContent">
          <textarea
            rows=""
            cols=""
            placeholder="多行输入"
            v-model="describe"
          ></textarea>
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
            :picker-options="pickerOptions"
          >
          </el-date-picker>
        </div>
      </div>
      <div class="list">
        <div class="listTitle">上传图片：</div>
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
                :file-list="picList"
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
                :file-list="fileInfo"
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
      <div class="list" style="align-items: flex-start">
        <div class="listTitle" style="line-height: 50px">上传附件：</div>
        <div class="listContent" style="border-bottom: 1px dotted #cccccc">
          <el-form :model="formThree">
            <el-form-item>
              <el-upload
                ref="uploadExcelTwo"
                action="/general/oss/upload"
                :auto-upload="true"
                :on-change="fileChangeTwo"
                :on-success="handleSuccessTwo"
                :on-remove="handleRemoveThree"
                :on-error="handleErrorTwo"
                :on-preview="HandFilePreViewTwo"
                :file-list="fileInfoTwo"
              >
                <el-button size="small" plain style="width: 100px; height: 50px"
                  >选择附件</el-button
                >
              </el-upload>
            </el-form-item>
          </el-form>
        </div>
      </div>

      <div class="list">
        <div class="listTitle">备注：</div>
        <div class="listContent">
          <textarea
            rows=""
            cols=""
            placeholder="多行输入"
            v-model="rmks"
            style="height: 100px"
          ></textarea>
        </div>
      </div>
      <div class="list">
        <div class="listTitle"></div>
        <div class="listContent">
          <button type="button" cover="point" @click="save">保存</button>
        </div>
      </div>
    </div>
 <!--  <div class="content" v-for="(item, index) in contentList" :key="index">
      <div class="contentHead">
        <img :src="arrowDown" class="down" :class="item.flag == true ? 'rotate' : ''" />
        <div class="left">
          第{{ total - index }}期(检验时间：{{ item.actDt }})
        </div>
        <div class="right" v-if="item.audit == '2'" style="color: #2778be" @click="showDetail(index)">
          检验完成
        </div>
        <div class="right" v-if="item.audit == '1'" style="color: red" @click="showDetail(index)">
          审核中
        </div>
        <div class="right" v-if="item.audit == '3'" style="color: red" @click="showDetail(index)">
          审核不通过
        </div>
      </div>
      <div class="contentBody" :class="item.flag == true ? 'active' : ''">
        <div class="report" v-viewer>
          <img :src="item1" v-for="(item1, index1) in item.imgUrl" :key="index1" v-if="item1 != []" />
        </div>
        <div class="file" v-if="list.length > 0">
          <span>文件：</span>
          <p v-for="(item1, index1) in list[index].fileList" :key="index1" v-if="item1 != []"
            @click="toLink(item1.url)">
            <img :src="item1.img" />
            <span style="width: 100px">{{ item1.fileNm }}</span>
          </p>
        </div>
        <div class="file" v-if="list.length > 0">
          <span>附件：</span>
          <p v-for="(item1, index1) in list[index].attachmentList" :key="index1" v-if="item1 != []"
            @click="toLink(item1.url)">
            <img :src="item1.img" />
            <span style="width: 100px">{{ item1.fileNm }}</span>
          </p>
        </div>
      </div>
    </div> -->
    <div class="jianzaoBox">
      <div class="topBox">
        <p v-for="(item,index) in options" :key="index" @click="checkIndex=index" :class="{clickP:checkIndex==index}">
          {{item.nm}}

        </p>
      </div>
      <!-- 不是照片 -->
      <div class="bottomBox"
        v-if="this.options[this.checkIndex] && this.options[this.checkIndex].cd!='DOCS_SURVEY_CYCLE.70'">
        <div v-for="(item,index) in list" :key="index">
          <div class="title" @click="item.show=!item.show">

            <span>{{item.title}}（ 时间 : {{item.actDt}} ）</span>
            <p>
              <span class="right" v-if="item.audit != '2'" style="color: red;margin-right: 10px;" @click.stop="toDelete(item)">
                删除
              </span>
              <span class="right" v-if="item.audit == '2'" style="color: #2778be;margin-right: 10px;">
                检验完成
              </span>
              <span class="right" v-if="item.audit == '1'" style="color: red;margin-right: 10px;" >
                审核中
              </span>
              <span class="right" v-if="item.audit == '3'" style="color: red;margin-right: 10px;" >
                审核不通过
              </span>
              <span class="right" v-if="item.audit != '2'" style="color: red;margin-right: 10px;" @click.stop="toModify(item)">
                修改
              </span>
              <span style="color: #2778BE;font-weight: bold">点击查看内容</span>
              <img :src="arrowDown" v-if="item.show">
              <img :src="arrowUp" v-else>
            </p>
          </div>



          <div class="rich" v-if="item.show">
            <p style="margin-bottom: 30px;" v-show="item.fileList">期报：</p>
            <div class="doc">
              <p v-for="j in item.fileList" @click="toLink(j.url)">
                <img :src="j.img">
                <span>{{j.fileNm}}</span>
              </p>
            </div>
            <p class="desc" v-show="item.imgList">
              {{item.nm}}报告：
            </p>
            <div class="report" v-viewer>
              <p v-for="j in item.imgList">
                <img :src="j.img">
              </p>
            </div>

            <p style="margin-top: 20px" v-show="item.attachmentList">附件：</p>
            <div class="doc">
              <p v-for="j in item.attachmentList" @click="toLink(j.url)">
                <img :src="j.img">
                <span>{{j.fileNm}}</span>

              </p>
            </div>
          </div>

        </div>
      </div>
      <!-- 照片 -->
      <div class="bottomBox"
        v-if="this.options[this.checkIndex] && this.options[this.checkIndex].cd=='DOCS_SURVEY_CYCLE.70'">
        <div v-for="(item,index) in list" :key="index">
          <div class="title" @click="item.show=!item.show">

            <span>{{item.title}}</span>
            <p>
              <span style="color: #2778BE;font-weight: bold">点击查看内容</span>
              <img :src="arrowDown" v-if="item.show">
              <img :src="arrowUp" v-else>
            </p>
          </div>

          <div class="rich" v-if="item.show">
            <p v-if="item.description" style="margin-bottom: 30px;">详细描述:{{item.description}}</p>
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
  import test1 from "../../assets/img/personal/test1.png";
  import arrowUp from "../../components/img/arrowUp.png";
  import arrowDown from "../../components/img/arrowDown.png";
  import excel from "@/assets/img/personal/excel.png";
  import ppt from "@/assets/img/personal/ppt.png";
  import word from "@/assets/img/personal/word.png";
  import pdf from "@/assets/img/personal/pdf.jpg";
  import date from "../../components/img/date.png";

  export default {
    name: "fangansheji",
    data() {
      return {
        title: "",
        rmks: "",
        fileInfo: [],
        fileInfoTwo: [],
        picList:[],
        dialogImageUrl: "",
        dialogVisible: false,
        formLabelWidth: "80px",
        limitNum: 3,
        form: {},
        formTwo: {
          file: "",
        },
        formThree: {
          file: "",
        },
        imgInfo: [],
        imgInfo2: [],
        currentIndex: -1,
        disabled: false,
        fileInfoList: [],
        fileInfoListTwo: [],
        contentList: [],
        pickerOptions: {
          shortcuts: [{
              text: "今天",
              onClick(picker) {
                picker.$emit("pick", new Date());
              },
            },
            {
              text: "昨天",
              onClick(picker) {
                const date = new Date();
                date.setTime(date.getTime() - 3600 * 1000 * 24);
                picker.$emit("pick", date);
              },
            },
            {
              text: "一周前",
              onClick(picker) {
                const date = new Date();
                date.setTime(date.getTime() - 3600 * 1000 * 24 * 7);
                picker.$emit("pick", date);
              },
            },
          ],
        },
        itemId:"",
        value1: "",
        value2:'',
        describe: "",
        pickDate: "",
        total: "",
        date,
        left,
        test1,
        excel,
        ppt,
        word,
        pdf,
        arrowDown,
        arrowUp,
        shipCd: "",
        shipStatus: "",
        list: [],
        listFile:[],
        maskShow:false,
        temporaryimg: [],
        info: {},
        currentRole: {},
        options: [], //分类
        optionsTwo:[],
        optionsOne: [{
            value: "选项1",
            label: "黄金糕",
          },
          {
            value: "选项2",
            label: "双皮奶",
          },
          {
            value: "选项3",
            label: "蚵仔煎",
          },
          {
            value: "选项4",
            label: "龙须面",
          },
          {
            value: "选项5",
            label: "北京烤鸭",
          },
        ],
        checkIndex: 0,
        ifProDetail: false, //是否商品详情
        currentRole: "",
      };
    },
    watch: {
      checkIndex() {
        this.getInfo();
      },
    },
    async mounted() {
      this.currentRole = JSON.parse(this.until.seGet("currentRole"));
      this.ifProDetail =
        window.location.href.indexOf("/Trade/productDetail") === -1 ?
        false :
        true;
      this.getDic();
      console.log(this.ifProDetail);
      if (!this.ifProDetail) {
        this.id = this.until.getQueryString("id");
        this.shipCd = this.until.getQueryString("shipCd");
        this.shipStatus = this.until.getQueryString("shipStatus");
        let param = {
          docsId: this.id,
          identityCd: "identity40",
        };
        this.info = await this.api.orgInfoBasic(param);
      }
      let qry = this.query.new();
      this.query.toW(qry, "docsId", this.id, "EQ");
      this.api.qualityListAll(this.query.toEncode(qry)).then((res) => {
        this.contentList = res;
        this.total = this.contentList.length;
        this.contentList.forEach((item) => {
          this.$set(item, "flag", false);
          item.imgUrl = item.imgUrl.split(",");
          item.fileUrl = item.fileUrl.split(",");
        });
      });
      this.api.dataDictionary('DOCS_SURVEY_STATUS').then((res)=>{
        this.optionsTwo=res
      })

    },
    methods: {
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
      			nm = nm.split('.' + type)[0]
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
      	console.log(fileList2)
      	this.listFile = fileList2
      	console.log('151', this.list)
      },
      closeMask(){
       this.title=""
       this.value1=""
       this.value2=""
       this.describe=""
       this.pickDate=""
       this.rmks=""
       this.imgInfo=[]
       this.fileInfoList=[]
       this.fileInfoListTwo=[]
       this.picList=[]
       this.fileInfo=[]
       this.fileInfoTwo=[]
      },
      toDelete(item){
        this.$confirm("确认删除?", "提示", {
        		confirmButtonText: "确定",
        		cancelButtonText: "取消",
        		type: "warning",
        	})
        	.then(() => {
        		return  this.api.quaGuarDel(item.id).then(res=>{
              this.getInfo()
        })

        	})
        	.catch(() => {
        		this.$message({
        			type: "info",
        			message: "已取消删除",
        		});
        	})
        	.then((res) => {
        		console.log('11', res);
        		if (res.code == 0) {
        			this.$message({
        				type: "success",
        				message: "删除成功!",
        			});
        		} else {
        			this.$message({
        				type: "error",
        				message: "删除失败!",
        			});
        		}
        	});


      },
      toModify(item){
        this.itemId=item.id
         this.maskShow=true
         this.title=item.title
         this.value1=item.cd
         this.value2=item.statusCd
         this.describe=item.description
         this.pickDate=item.actDt
         this.rmks=item.rmks
         this.imgInfo=item.imgUrl
         this.fileInfoList=item.fileUrl
         this.fileInfoListTwo=item.attachment
         if(item.imgUrl){
           let urlStr=item.imgUrl.split(",")
             urlStr.forEach(item1=> {
                        let obj = new Object();
                        obj.url = item1;
                        this.picList.push(obj)
                      });
               console.log(11);
         }
         if(item.fileUrl){
           let modelList = item.fileUrl.split(',')
            this.getFile(modelList)
           					if (this.listFile.length>0) {
           						for (let i = 0; i < this.listFile.length; i++) {
           							this.fileInfo.push({
           								name: this.listFile[i].fileNm,
           								url: this.listFile[i].url
           							})
           						}

           					}
         }
         if(item.attachment){
           let modelListTwo = item.attachment.split(',')
            this.getFile(modelListTwo)
           					if (this.listFile.length>0) {
           						for (let i = 0; i < this.listFile.length; i++) {
           							this.fileInfoTwo.push({
           								name: this.listFile[i].fileNm,
           								url: this.listFile[i].url
           							})
           						}

           					}
         }

        console.log(123,this.fileInfoTwo);

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

      fileChange(file, fileList) {
        this.formTwo.file = file.raw;
      },
      fileChangeTwo(file, fileList) {
        this.formThree.file = file.raw;
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
      handleRemoveThree(file, fileList) {
       this.fileInfoListTwo=fileList
       this.fileInfoListTwo.forEach(item=>{
         if(item.response){
           item.newFile=item.response.data
         }
        else{
          item.newFile=item.url
        }
       })
       this.fileInfoListTwo =this.fileInfoListTwo.map((item) => item.newFile).join(",");
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
      handleSuccessTwo(res, file, fileList) {
        // this.fileInfoListTwo = fileList
        //   .map((item) => item.response.data)
        //   .join(",");
        this.fileInfoListTwo=fileList
        this.fileInfoListTwo.forEach(item=>{
          if(item.response){
            item.newFile=item.response.data
          }
         else{
           item.newFile=item.url
         }
        })
        this.fileInfoListTwo =this.fileInfoListTwo.map((item) => item.newFile).join(",");
      },
      handleError(err, file, fileList) {
        this.$notify.error({
          title: "错误",
          message: `文件上传失败`,
        });
      },
      handleErrorTwo(err, file, fileList) {
        this.$notify.error({
          title: "错误",
          message: `文件上传失败`,
        });
      },
      showDetail(index) {
        this.contentList[index].flag = !this.contentList[index].flag;
        console.log(this.contentList[index].flag);
      },
      HandFilePreView(file) {},
      HandFilePreViewTwo(file) {},
      handleDownload(file) {
        console.log(file);
      },
      selectone(cd) {
        this.statusCd = cd;
        console.log("kan", cd);
      },
      async save() {
        console.log(this.currentRole);
        let obj = {
          docsId: this.id,
          title: this.title,
          cd: this.value1,
          statusCd:this.value2,
          description: this.describe,
          actDt: this.pickDate,
          imgUrl: this.imgInfo,
          fileUrl: this.fileInfoList,
          attachment: this.fileInfoListTwo,
          surveyId: this.currentRole.id,
          surveyNm: this.currentRole.company,
          rmks: this.rmks,
        };
        this.api.quaGuarAdd(obj).then((res) => {
          console.log(res);
          this.getInfo()
          this.title=""
          this.value1=""
          this.value2=""
          this.describe=""
          this.pickDate=""
          this.rmks=""
          this.imgInfo=[]
          this.fileInfoList=[]
          this.fileInfoListTwo=[]
          this.picList=[]
          this.fileInfo=[]
          this.fileInfoTwo=[]
        });
      },
      cancel(){
        this.maskShow=false
        this.title=""
        this.value1=""
        this.value2=""
        this.describe=""
        this.pickDate=""
        this.rmks=""
        this.imgInfo=[]
        this.fileInfoList=[]
        this.fileInfoListTwo=[]
        this.picList=[]
        this.fileInfo=[]
        this.fileInfoTwo=[]
      },
      modify(){
       let obj = {
         id:this.itemId,
         docsId: this.id,
         title: this.title,
         cd: this.value1,
         description: this.describe,
         actDt: this.pickDate,
         imgUrl: this.imgInfo,
         fileUrl: this.fileInfoList,
         attachment: this.fileInfoListTwo,
         surveyId: this.currentRole.id,
         surveyNm: this.currentRole.company,
         rmks: this.rmks,
       };
       this.api.quaGuarUpd(obj).then((res) => {
         console.log(res);
         this.getInfo()
         this.maskShow=false
         this.title=""
         this.value1=""
         this.value2=""
         this.describe=""
         this.pickDate=""
         this.rmks=""
         this.imgInfo=[]
         this.fileInfoList=[]
         this.fileInfoListTwo=[]
         this.picList=[]
         this.fileInfo=[]
         this.fileInfoTwo=[]
       });
      },
      async getDic() {
        this.options = await this.api.dataDictionary("DOCS_SURVEY_CYCLE");
        // console.log('获取数据字典')
        // console.log(this.options)
        // data.forEach(item=>{
        //   this.options.push({label:item.nm,value:item.cd})
        // })
        console.log("7878", this.options);
        this.getInfo();
      },
      async getInfo(id) {
        // console.log('品质保障')
        // this.currentRole=JSON.parse(this.until.seGet('currentRole'))
        this.list = [];
        let qry = this.query.new();
        if (id) {
          this.id = id;
          this.query.toW(qry, "docsId", id, "EQ");
        } else {
          this.query.toW(qry, "docsId", this.id, "EQ");
        }

        this.query.toW(qry, 'cd', this.options[this.checkIndex].cd, 'EQ')
        let data = await this.api.qualityListAll(this.query.toEncode(qry));
        console.log(787897, data);
        let data1 = [];
        if (data.length > 0) {
          data.forEach((item, index) => {
            if (index == 0) {
              item.show = true;
            } else {
              item.show = false;
            }

            if (this.options[this.checkIndex].cd != 'DOCS_SURVEY_CYCLE.70') {
            let imgList1 = item.imgUrl ? item.imgUrl.split(",") : [];
            let imgList2 = [];
            imgList1.forEach((v) => {
              imgList2.push({
                url: v,
                img: v,
              });
            });
            item.imgList = imgList2;

            let fileList1 = item.fileUrl ? item.fileUrl.split(",") : [];
            let fileList2 = [];
            let fileList11 = item.attachment ? item.attachment.split(",") : [];
            let fileList22 = [];
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
                  fileNm: nm,
                });
              } else if (type == "doc" || type == "docx") {
                fileList2.push({
                  url: v,
                  img: this.word,
                  fileNm: nm,
                });
              } else if (type == "ppt" || type == "pptx") {
                fileList2.push({
                  url: v,
                  img: this.ppt,
                  fileNm: nm,
                });
              } else if (type == "xls" || type == "xlsx") {
                fileList2.push({
                  url: v,
                  img: this.excel,
                  fileNm: nm,
                });
              } else {
                fileList2.push({
                  url: v,
                  img: v,
                  fileNm: nm,
                });
              }
              item.fileList = fileList2;
            });
            fileList11.forEach((v) => {
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
                fileList22.push({
                  url: v,
                  img: this.pdf,
                  fileNm: nm,
                });
              } else if (type == "doc" || type == "docx") {
                fileList22.push({
                  url: v,
                  img: this.word,
                  fileNm: nm,
                });
              } else if (type == "ppt" || type == "pptx") {
                fileList22.push({
                  url: v,
                  img: this.ppt,
                  fileNm: nm,
                });
              } else if (type == "xls" || type == "xlsx") {
                fileList22.push({
                  url: v,
                  img: this.excel,
                  fileNm: nm,
                });
              } else {
                fileList22.push({
                  url: v,
                  img: v,
                  fileNm: nm,
                });
              }
              item.attachmentList = fileList22;
            });
            }
            else {
              item.imgList1 = item.imgUrl ? item.imgUrl.split(',') : []
              if (data1.length == 0) {
                console.log(item)
                data1[0] = item
              } else {
                if (item.actDt == data1[data1.length - 1].actDt) {
                  data1[data1.length - 1].imgList1.push(...item.imgList1)
                } else {
                  data1.push(item)
                }
              }
            }
          });
        }
        if (this.options[this.checkIndex].cd != "DOCS_SURVEY_CYCLE.70") {
          this.list = data;
        } else {
          this.list = data1;
        }
        console.log("151", this.list);
      },
      toLink(url) {
        console.log(url);
        window.open(url);
      },
    },
  };
</script>
<!-- <style  lang="less">

</style> -->
<style scoped lang="less">
/deep/ .el-dialog{
      background-color: #FFFFFF !important;
      width:70% !important;
      .list {
        display: flex;
        align-items: center;
        margin-bottom: 30px;
        margin-left: 10%;
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
  .info {
    display: flex;
    // height: 150px;
    width: 100%;
    padding-bottom: 30px;
    // margin-bottom: 30px;
    border-bottom: 1px solid rgba(0, 0, 0, 0.1);

    img {
      width: 150px;
      height: 150px;
      margin-right: 19px;
    }

    div {
      padding-bottom: 5px;
      display: flex;
      flex-direction: column;
      justify-content: space-between;
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

  .content {
    margin-bottom: 20px;

    .contentHead {
      width: 100%;
      height: 40px;
      background: #f7f7f7;
      border: 1px solid #eeeeee;
      display: flex;
      align-items: center;
      justify-content: space-between;
      box-sizing: border-box;
      padding: 0 28px;
      position: relative;

      .down {
        width: 13px;
        height: 7px;
        position: absolute;
        right: 10px;
        top: 40%;
        transform: translateY(-90%);
        transition: 0.5s;
        transform: rotate(0deg);
      }

      .rotate {
        transform: rotate(180deg);
        transition: 0.5s;
        // transform-origin: top center;
      }

      .left {
        font-size: 14px;
        font-weight: bold;
        color: #666666;
      }

      .right {
        cursor: pointer;
      }
    }

    .contentBody {
      padding: 29px 19px;
      display: none;

      .titleone {
        font-size: 14px;
        color: #666666;
      }

      .report {
        display: flex;
        max-width: 900px;

        img {
          height: 70px;
          width: 70px;
          margin-left: 10px;
        }
      }

      .file {
        p {
          cursor: pointer;

          img {
            margin-left: 10px;
            margin-top: 10px;
            height: 70px;
            width: 70px;
          }

          span {
            margin-top: 0;
          }
        }

        span {
          display: block;
          margin-left: 10px;
          margin-top: 20px;
        }
      }
    }

    .contentBodyTwo {
      padding: 29px 19px;
      min-height: 220px;
      display: none;
      border: 1px solid #eeeeee;
    }

    .active {
      display: block;
    }
  }

  .jianzaoBox {
    display: flex;
    flex-direction: column;

    .topBox {
      width: 100%;
      margin-bottom: 30px;
      // background-color: #f3f3f3;
      display: flex;
      align-items: center;
      border-bottom: 1px solid rgba(0, 0, 0, 0.1);
      overflow-x: auto;

      >p {
        font-size: 14px;
        color: #666666;
        padding: 14px 20px;
        cursor: pointer;
        white-space: nowrap;
      }

      .clickP {
        font-size: 16px;
        color: #303030;
        font-weight: 600;
        border-bottom: 2px solid #2778be;
      }
    }

    .bottomBox {
      // flex: 4;
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
    padding: 24px 28px;

    .desc {
      color: #666666;
    }

    .report {
      display: flex;
      flex-wrap: wrap;
      width: 100%;

      p {
        width: 180px;
        height: 120px;
        margin: 10px 10px 0 0;

        img {
          display: block;
          width: 180px;
          height: 120px;
          object-fit: cover;
          margin: 0 auto;
        }
      }
    }

    .doc {
      display: flex;
      // flex-direction: column;
      flex-wrap: wrap;
      width: 100%;

      p {
        margin-top: 10px;
        width: 16.6%;
        display: flex;
        flex-direction: column;
        align-items: center;

        img {
          width: 62px;
          height: 69px;
          margin-right: 3px;
          display: block;
          cursor: pointer;

          &:hover {}
        }

        span {
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
