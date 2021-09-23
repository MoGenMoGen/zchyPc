<template>
  <div class="main">
    <div style="padding:0 30px">
      <tobbar :title="title" :showAll="false"></tobbar>
    </div>
    <p style="border-bottom: 1px solid rgba(0,0,0,0.1);"></p>
    <div class="body" v-show="nextShow==false">
      <div class="head">
        <span>新增整改单</span>
        <div class="line">
        </div>
      </div>
      <div class="list">
        <div class="" style="display: flex;">
          <div class="" style="width: 48%;">
            <div class="listContent">
              <div class="listLeft">
                <span>*</span>
                整改名称:
              </div>
              <div class="listRight">
                <input type="" name="" id="" value="" placeholder="请输入整改名称" v-model="rectificationName" />
              </div>
            </div>
            <div class="listContent">
              <div class="listLeft">
                <span>*</span>
                整改单位:
              </div>
              <div class="listRight">
                <el-select v-model="rectificationCompany" clearable filterable placeholder="请选择整改单位">
                  <el-option v-for="item in options" :key="item.label" :label="item.label" :value="item.label">
                  </el-option>
                </el-select>
              </div>
            </div>
            <div class="listContent">
              <div class="listLeft">
                <span>*</span>
                检查区域:
              </div>
              <div class="listRight">
                <input type="" name="" id="" value="" placeholder="请输入检查区域" />
              </div>
            </div>
            <div class="listContent">
              <div class="listLeft">
                <span>*</span>
                责任整改人:
              </div>
              <div class="listRight">
                <el-select v-model="people" clearable filterable placeholder="请选择整改人">
                  <el-option v-for="item in optionsTwo" :key="item.label" :label="item.label" :value="item.label">
                  </el-option>
                </el-select>
              </div>
            </div>
            <div class="listContent">
              <div class="listLeft">
                <span>*</span>
                整改期限:
              </div>
              <div class="listRight">
                <el-date-picker v-model="pickTime" type="date" placeholder="选择日期">
                </el-date-picker>
              </div>
            </div>
          </div>
          <div class=""  style="width: 48%;">
            <div class="listContent" style="align-items: flex-start;">
              <div class="listLeft" style="line-height: 40px;">
                <span>*</span>
                隐患说明:
              </div>
              <div class="listRight">
                <textarea rows="" cols="" placeholder="请输入隐患说明" v-model="dangerDescription"></textarea>
              </div>
            </div>
            <div class="listContent" style="align-items: flex-start;">
              <div class="listLeft"  style="line-height: 40px;">
                <span>*</span>
                整改要求:
              </div>
              <div class="listRight">
                <textarea rows="" cols="" placeholder="请输入整改要求" v-model="requirement"></textarea>
              </div>
            </div>
          </div>
        </div>
        <div class="listContent">
          <div class="listLeft" style="margin-bottom: 73px;">
            <span>*</span>
            隐患图片:
          </div>
          <div class="listRight" style="width: 80%;">
          <!--  <el-upload action="/general/oss/upload" list-type="picture-card"
              :on-preview="handlePictureCardPreview" :on-remove="handleRemove">
              <i class="el-icon-plus"></i>
            </el-upload>
            <el-dialog :visible.sync="dialogVisible">
              <img width="100%" :src="dialogImageUrl" alt="">
            </el-dialog> -->
             <el-form :model="form">
                <el-form-item  style="margin-bottom: -20px;">
                  <el-upload
                    ref="upload"
                    action="/general/oss/upload"
                    accept="image/png,image/gif,image/jpg,image/jpeg"
                    list-type="picture-card"

                    :auto-upload="false"

                    :before-upload="handleBeforeUpload"
                    :on-preview="handlePictureCardPreview"
                    :on-remove="handleRemove"
                     :on-success="handSuccess">

                    <i class="el-icon-plus"></i>
                  </el-upload>
                  <el-dialog :visible.sync="dialogVisible" >
                    <img width="100%" :src="dialogImageUrl" alt="">
                  </el-dialog>
                </el-form-item>
                <el-form-item>
                <div class="btn"style="margin-top:30px; margin-left: 0;" >
                  <button type="button" class="next" @click="nextTo">下一步</button>
                  <button type="button" class="cancel"@click="cancel">取消</button>
                </div>
                </el-form-item>
              </el-form>
          </div>
        </div>

      </div>
    </div>
    <div class="body" v-show="nextShow==true">
      <div class="head">
        <span>安全隐患整改通知单</span>
        <div class="line">

        </div>
      </div>
      <div class="list">
         <div class="" style="display: flex;">
            <div style="width: 48%;">
                <div class="listContent">
                  <div class="listLeft">
                    整改单号：
                  </div>
                  <div class="listRight">
                      {{danhao}}
                  </div>
                </div>
                <div class="listContent">
                  <div class="listLeft">
                    整改单位：
                  </div>
                  <div class="listRight">
                      {{rectificationCompany}}
                  </div>
                </div>
                <div class="listContent">
                  <div class="listLeft">
                    整改要求：
                  </div>
                  <div class="listRight">
                      {{requirement}}
                  </div>
                </div>
                <div class="listContent">
                  <div class="listLeft">
                    整改责任人签字：
                  </div>
                  <div class="listRight">
                     <input type="" name="" id="" value="" placeholder="请签字" />
                  </div>
                </div>
                <div class="listContent">
                  <div class="listLeft">
                    复查日期：
                  </div>
                  <div class="listRight">
                      <el-date-picker v-model="reviewTime" type="date" placeholder="选择复查日期">
                      </el-date-picker>
                  </div>
                </div>
                <div class="listContent">
                  <div class="listLeft">
                    检验检测单位：
                  </div>
                  <div class="listRight">
                   <el-select v-model="mechanism" clearable filterable placeholder="请选择检验检测机构">
                     <el-option v-for="item in optionsThree" :key="item.label" :label="item.label" :value="item.label">
                     </el-option>
                   </el-select>
                  </div>
                </div>
                <div class="listContent" style="align-items: flex-start;">
                  <div class="listLeft" style="line-height: 40px;">
                    复查情况:
                  </div>
                  <div class="listRight">
                    <textarea rows="" cols="" placeholder="请输入复查情况" v-model="reviewSituation"></textarea>
                  </div>
                </div>


            </div>
            <div style="width: 48%;">
                <div class="listContent">
                  <div class="listLeft">
                    整改名称：
                  </div>
                  <div class="listRight">
                      {{rectificationName}}
                  </div>
                </div>
                <div class="listContent">
                  <div class="listLeft">
                  隐患说明：
                  </div>
                  <div class="listRight">
                      {{dangerDescription}}
                  </div>
                </div>
                <div class="listContent">
                  <div class="listLeft">
                    责任整改人：
                  </div>
                  <div class="listRight">
                      {{people}}
                  </div>
                </div>
                <div class="listContent">
                  <div class="listLeft">
                    整改日期：
                  </div>
                  <div class="listRight">
                      <el-date-picker v-model="rectifyTime" type="date" placeholder="选择复查日期">
                      </el-date-picker>
                  </div>
                </div>
                <div class="listContent">
                  <div class="listLeft">
                    复查人签字：
                  </div>


                  <div class="listRight">
                     <input type="" name="" id="" value="" placeholder="请签字" />
                  </div>
                </div>

            </div>

         </div>
          <div class="listContent" style="align-items: flex-start;width: 100%;">
            <div class="listLeft">
              隐患图片：
            </div>
            <div class="listRight" style="width: 80%;">
                <div class="img" style="display: flex;flex-wrap: wrap;" v-viewer>
                    <img :src="item" style="width:  148px;height: 148px;margin-right: 10px;margin-bottom: 10px; " v-for="(item,index) in imgList" :key="index">
                </div>
            </div>
          </div>
          <div class="btn" style="padding-bottom: 20px;">
            <button type="button" class="next" @click="submit">完成</button>
            <button type="button" class="cancel" @click="backTo">上一步</button>
          </div>
      </div>
    </div>
  </div>

</template>

<script>
  import tobbar from "../../components/person/tobbar";
  import {
    mapState
  } from "vuex";
  export default {
    name: "allShip",
    components: {
      tobbar,
    },
    data() {
      return {
        dialogImageUrl: '',
        dialogVisible: false,
        formLabelWidth: '80px',

        form: {},
        info:{

        },
        danhao:"ZG20210330001",
        title: '整改单',
        value: "",
        value2: "",
        pickTime: "",
        dialogImageUrl: '',
        nextShow: false,
        dialogVisible: false,
        options: [{
          value: '选项1',
          label: '黄金糕'
        }, {
          value: '选项2',
          label: '双皮奶'
        }, {
          value: '选项3',
          label: '蚵仔煎'
        }, {
          value: '选项4',
          label: '龙须面'
        }, {
          value: '选项5',
          label: '北京烤鸭'
        }],
        optionsTwo: [{
          value: '选项1',
          label: '傻杯'
        }, {
          value: '选项2',
          label: '250'
        }, {
          value: '选项3',
          label: '蚵仔煎'
        }, {
          value: '选项4',
          label: '龙须面'
        }, {
          value: '选项5',
          label: '北京烤鸭'
        }],
        optionsThree: [{
          value: '选项1',
          label: '姚峰'
        }, {
          value: '选项2',
          label: '250'
        }, {
          value: '选项3',
          label: '摩根'
        }, {
          value: '选项4',
          label: '龙须面'
        }, {
          value: '选项5',
          label: '北京烤鸭'
        }],
        reviewSituation:"",//复查情况
        mechanism:"",//检验检测机构
        reviewTime:'',//复查日期
        rectifyTime:'',//整改日期
        rectificationName:'',//整改名称
        rectificationCompany:'',//整改单位
        dangerDescription:'',//隐患说明
        requirement:'',//整改要求
        people:"",//责任整改人
        imgList:[]

      }
    },
    layout: 'person',
    async mounted() {},
    computed: {
      ...mapState([
        'currentRole'
      ])
    },
    watch: {
    },
    methods: {
         handleBeforeUpload(file){
            console.log('before')
            if(!(file.type === 'image/png' || file.type === 'image/gif' || file.type === 'image/jpg' || file.type === 'image/jpeg')) {
              this.$notify.warning({
                title: '警告',
                message: '请上传格式为image/png, image/gif, image/jpg, image/jpeg的图片'
              })
            }
            let size = file.size / 1024 / 1024 / 2
            if(size > 2) {
              this.$notify.warning({
                title: '警告',
                message: '图片大小必须小于2M'
              })
            }
          },
      handleRemove(file, fileList) {
        this.imgList=[]
        
        for(let i=0;i<fileList.length;i++){
        
          this.imgList.push(fileList[i].response.data)
        }
        console.log(file, fileList);
      },
      handlePictureCardPreview(file) {
        this.dialogImageUrl = file.url;
        this.dialogVisible = true;
      },
        // uploadFile() {

        //     this.$refs.upload.submit()
        //   },
          handSuccess(response, file, fileList){
            this.imgList=[]

            for(let i=0;i<fileList.length;i++){

              this.imgList.push(fileList[i].response.data)
            }

console.log("姚峰是猪",file,fileList,this.imgList);
          },
      nextTo() {
         this.$refs.upload.submit()

        this.nextShow = true

      },
      cancel(){
        this.until.back()
      },
      backTo(){
        this.nextShow=false
      },
      submit(){
      this.until.back()
      }
    },
  }
</script>

<style scoped lang="less">
  .main {
    width: 100%;
    .body {
      .head {
        padding: 30px;
        display: flex;
        align-items: center;
        span {
          font-size: 16px;
          color: #333333;
          font-weight: 400;
          position: relative;
        }
        .line {
          width: 578px;
          height: 1px;
          background: #000000;
          opacity: 0.1;
          margin-left: 10px;
        }
      }
      .list {
        padding-left: 54px;
        .listContent {
          margin-top: 20px;
          display: flex;
          align-items: center;
          .listLeft {
            font-size: 14px;
            font-weight: 400;
            width: 114px;
            span {
              color: #E4393C;
            }

          }


          .listRight {
            .el-select {
              width: 277px;
              font-size: 12px;
            }
            .el-select::placeholder {
              font-size: 12px;
              font-family: Microsoft YaHei;
              font-weight: 400;
              color: #999999;
            }
            /deep/.el-input--suffix .el-input__inner{
              font-size: 12px;
              color: black;
            }
            .el-input {
              width: 277px;
              // padding: 10px;
              font-size: 12px;
            }
            /deep/.el-input--suffix .el-input__inner::placeholder {
              font-size: 12px;
              font-family: Microsoft YaHei;
              font-weight: 400;
              color: #999999;
            }
            input {
              width: 277px;
              height: 40px;
              background: #FFFFFF;
              border: 1px solid #DDDDDD;
              padding: 10px;
              font-size: 12px;
            }
            input::placeholder {
              font-size: 12px;
              font-family: Microsoft YaHei;
              font-weight: 400;
              color: #999999;
            }
            textarea {
              width: 277px;
              height: 92px;
              background: #FFFFFF;
              border: 1px solid #DDDDDD;
              padding: 10px;
              font-size: 12px;
              resize: none;
            }
            textarea::placeholder {
              font-size: 12px;
              font-family: Microsoft YaHei;
              font-weight: 400;
              color: #999999;
            }
          }
        }
        .btn {
          margin-left: 90px;
          margin-top: 40px;
          .next {
            width: 86px;
            height: 35px;
            background: #2778BE;
            border: 0;
            border-radius: 4px;
            font-size: 16px;
            color: #ffffff;
            cursor: pointer;
          }
          .cancel {
            width: 86px;
            height: 35px;
            background: #FFFFFF;
            border: 1px solid #DDDDDD;
            border-radius: 4px;
            font-size: 16px;
            color: #999999;
            margin-left: 30px;
            cursor: pointer;
          }
        }
      }
    }
  }
</style>
