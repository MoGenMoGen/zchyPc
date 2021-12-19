<template>
  <!--船舶档案-->
  <div class="main">
    <el-dialog
     title="新增"
     :visible.sync="postShow"
     width="30%"
     @close="closePost">
      <div class="postBox">
        <div class="list">
          <div class="listTitle">设计机构：</div>
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
          <div class="listTitle">设计流程：</div>
          <div class="listContent" >
            <el-select
              v-model="value2"
              clearable
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
          <div class="listTitle">权重排序：</div>
          <div class="listContent">
             <el-input-number v-model="value3" @change="handleChange" label="描述文字"></el-input-number>
          </div>
        </div>

        <div class="list">
          <div class="listTitle">完成时间：</div>
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
    <!--    查看视频预览弹出框-->
    <el-dialog
      :visible.sync="show"
      width="2%"
      :before-close="test"
      :show-close="closeShow"
    >
      <div
        style="
          position: fixed;
          top: 0;
          right: 0;
          bottom: 0;
          left: 0;
          margin: auto;
          height: 600px;
          width: 800px;
        "
      >
        <yunjianzao v-if="show" ref="videoPlayer"></yunjianzao>
      </div>
      <!--<hik v-if="show" ref="videoPlayer" :cameraIndexCode="code"></hik>-->
      <!--</div>-->
    </el-dialog>
    <div class="line0">
      <tobbar></tobbar>
    </div>

    <div class="line1">
      <div>
        <p
          v-for="(item, index) in tab1"
          :key="item.id"
          :class="{ active: tab1Id == item.id }"
          @click="chooseTab1(item, index)"
        >
          <!--控制显示 v-show="formW!='jiance'||(formW=='jiance'&&index==2)" -->
          <span>{{ item.nm }}</span>
        </p>
      </div>
    </div>
    <div class="line2" v-if="!showWeiBao && !showBian && !showYunXing">
      <div>
        <p
          v-for="(item, index) in selectList"
          :key="item.id"
          :class="{ active2: tab2Id == item.id }"
          @click="chooseTab2(item, index)"
        >
          <!--控制显示 v-show="formW!='jiance'||(formW=='jiance'&&index==3)" -->
          <span>{{ item.nm }}</span>
        </p>
        <span class="btm" :style="{ left: tabIndex * 129 + 'px' }"></span>
        <p v-if="tab2Id == 10 ||tab2Id == 11 || tab2Id == 12 || tab2Id == 13" style="position: absolute;right:30%;color: red;"@click="postShow=true">新增</p>
      </div>
    </div>
    <!--      变更记录-->
    <div class="line3" v-if="showBian">
      <bian />
    </div>
    <!--      维保记录-->
    <div class="line3" v-if="showWeiBao">
      <weibao />
    </div>
    <!--      运行记录-->
    <div class="line3" v-if="showYunXing">
      <yunxing />
    </div>
    <div class="line4">
      <!--基本信息-->
      <base-info1 v-if="tab2Id == 20" :title="designTitle"></base-info1>
      <!--船舶设备-->
      <ship-equip
        v-if="tab2Id == 21"
        type="1"
        ifPerson="true"
        :id="id"
        :width="width * 0.81 - 60"
      ></ship-equip>
      <!--船舶检验证书-->
      <certificate v-if="tab2Id == 22"></certificate>
      <!-- 设备及其他证书 -->
      <jianyanjiance v-if="tab2Id == 23"></jianyanjiance>
      <!-- 合同列表 -->
      <contract v-if="tab2Id == 24"></contract>
      <!-- 历史资料 -->
      <history-pic v-if="tab2Id == 25"></history-pic>
    </div>
    <div class="line4">
      <base-info3 v-if="tab2Id == 30" :title="designTitle"></base-info3>
      <jianzaoliucheng v-if="tab2Id == 31"></jianzaoliucheng>
      <pinzhibaozhang v-if="tab2Id == 32"></pinzhibaozhang>
      <testreport v-if="tab2Id == 33"></testreport>
      <!--<yunjianzao v-if="tab2Id==34" ref="videoPlayer" :cameraIndexCode="code"></yunjianzao>-->
      <yunjianyan v-if="tab2Id == 35"></yunjianyan>
    </div>
    <div class="line4">
      <base-info2 v-if="tab2Id == 10" :title="designTitle"></base-info2>
      <fangansheji
        v-if="tab2Id == 11 || tab2Id == 12 || tab2Id == 13"
        :title="designTitle"
        :flowCd="tab2Id"
      ></fangansheji>
    </div>
  </div>
</template>

<script>
import { mapState } from "vuex";
const certificate = (resolve) =>
  require(["@/components/onlineTrading/certificate"], resolve); //检测证书
const shipEquip = (resolve) =>
  require(["@/components/onlineTrading/shipEquipTree"], resolve); //设备
import tobbar from "../../components/person/tobbar";
import bian from "../../components/person/bian";
import weibao from "../../components/person/weibao";
import yunxing from "../../components/person/yunxing";
import fangansheji from "../../components/person/fangansheji";
import baseInfo3 from "../../components/person/baseInfo3";
import baseInfo2 from "../../components/person/baseInfo2";
import baseInfo1 from "../../components/person/baseInfo1";
import yunjianzao from "../../components/person/yunjianzao";
import jianzaoliucheng from "../../components/person/jianzaoliucheng";
import pinzhibaozhang from "../../components/person/pinzhibaozhang";
import testreport from "../../components/person/testreport";
import jianyanjiance from "../../components/person/jianyanjiance";
import yunjianyan from "../../components/person/yunjianyan";
import contract from "../../components/person/contract";
import historyPic from "../../components/person/historyPic";
export default {
  name: "archives",
  layout: "person",
  components: {
    certificate,
    shipEquip,
    tobbar,
    bian,
    weibao,
    yunxing,
    fangansheji,
    baseInfo3,
    baseInfo2,
    baseInfo1,
    yunjianzao,
    jianzaoliucheng,
    pinzhibaozhang,
    jianyanjiance,
    testreport,
    contract,
    yunjianyan,
    historyPic,
  },
  computed: {
    ...mapState(["bWidth", "width"]),
  },
  data() {
    return {
      dialogImageUrl: "",
      dialogVisible: false,
      postShow:false,
      options:[{
        nm:'哈哈哈',
        cd:1
      }],
      value1:'',
      value2:'',
      optionsTwo:[{
        nm:'哈哈哈',
        cd:1
      }],
      title:'',
      value3:'',
      optionsThree:[{
        nm:'哈哈哈',
        cd:1
      }],
      pickDate:'',
      form:{},
      imgInfo:[],
      formTwo:{
        file:'',
      },
      fileInfoList:[],
      closeShow: false,
      show: false,
      code: "121b6f4e237c4a889df93a060d428fa0", //视频编号
      tab1Id: -1, //第一个标签页选中
      tab2Id: -1, //第二个标签页选中
      tabIndex: 0, //第二个标签页底部边框偏移
      selectList: [], //选中列表
      showWeiBao: false,
      showBian: false,
      showYunXing:false,
      designTitle: "方案设计", //设计标题
      id: "",
      tab1: [
        {
          id: 0,
          nm: "船舶资料",
        },
        {
          id: 1,
          nm: "设计档案",
        },
        {
          id: 2,
          nm: "建造档案",
        },
        {
          id: 3,
          nm: "变更记录",
        },
        {
          id: 4,
          nm: "维保记录",
        },
        // {
        //   id: 5,
        //   nm: "运行记录",
        // },
      ],
      tab21: [
        {
          id: 10,
          nm: "基本信息",
        },
        {
          id: 11,
          nm: "方案设计",
        },
        {
          id: 12,
          nm: "送审设计",
        },
        {
          id: 13,
          nm: "完工设计",
        },
      ],
      tab22: [
        {
          id: 20,
          nm: "基本信息",
        },
        {
          id: 21,
          nm: "船舶设备",
        },
        {
          id: 22,
          nm: "船舶检验证书",
        },
        {
          id: 23,
          nm: "设备及其他证书",
        },
        {
          id: 24,
          nm: "合同列表",
        },
        {
          id: 25,
          nm: "历史资料",
        },
      ],
      tab23: [
        {
          id: 30,
          nm: "基本信息",
        },
        {
          id: 31,
          nm: "建造过程",
        },
        {
          id: 32,
          nm: "建造监理",
        },
        {
          id: 33,
          nm: "检测报告",
        },
        {
          id: 34,
          nm: "云监造",
        },
        {
          id: 35,
          nm: "云检验",
        },
      ],
      formW: "",
    };
  },

  mounted() {
    this.formW = this.until.getQueryString("formW");
    this.id = this.until.getQueryString("id");
    if(this.until.getQueryString('toYunCheck')) {
      this.tab1Id=2;
      this.tab2Id=35;
      this.selectList = this.tab23
      this.tabIndex=5;
    } else {
      this.chooseTab1(this.tab1[0], 0);
    }
    console.log(this.id);
    //控制显示
    // if(this.formW=='jiance'){
    //   this.chooseTab1(this.tab1[2],2)
    // }else{
    //   this.chooseTab1(this.tab1[0],0)
    // }
    // this.chooseTab1(this.tab1[0], 0);
  },

  methods: {
    closePost(){
      
    },
    handleChange(val){
      this.value3=val
      console.log(this.value3);
    },
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
    test() {
      console.log("关闭了");
      this.$refs.videoPlayer.off();
      this.show = false;
    },
    chooseTab1(item, index) {
      if (item.id != this.tab1Id) {
        this.tab1Id = item.id;
        if (index == 0) {
          this.selectList = this.tab22;
          this.showBian = false;
          this.showWeiBao = false;
          this.showYunXing = false;
          this.chooseTab2(this.selectList[0], 0);
        } else if (index == 1) {
          this.selectList = this.tab21;
          this.chooseTab2(this.selectList[0], 0);
          this.showBian = false;
          this.showWeiBao = false;
          this.showYunXing = false;
        } else if (index == 2) {
          this.selectList = this.tab23;
          //控制显示
          // if(this.formW=='jiance'){
          //   this.chooseTab2(this.selectList[3],3)
          // }else{
          //   this.chooseTab2(this.selectList[0],0)
          // }
          this.chooseTab2(this.selectList[0], 0);

          this.showBian = false;
          this.showWeiBao = false;
          this.showYunXing = false;
        } else if (index == 3) {
          this.tab2Id = 0;
          this.showWeiBao = false;
          this.showBian = true;
          this.showYunXing = false;
        } else if (index == 4) {
          this.tab2Id = 0;
          this.showBian = false;
          this.showWeiBao = true;
          this.showYunXing = false;
        }else if (index == 5) {
          this.tab2Id = 0;
          this.showBian = false;
          this.showWeiBao = false;
          this.showYunXing = true;
        }
      }
    },
    chooseTab2(item, index) {
      if (item.id == 34) {
        // this.tab2Id=item.id
        // this.show = true
        // console.log(this.$refs)
        // this.$refs.videoPlayer.getData()
        window.open(
          "http://visual.ship88.cn/bigScreen/index3.html?id=" + this.id
        );
      } else if (item.id != this.tab2Id) {
        this.tab2Id = item.id;
        // this.$refs.videoPlayer.off();

        //控制显示
        // this.tabIndex=this.formW=='jiance'?0:index
        this.tabIndex = index;

        this.designTitle = item.nm;
      }
    },
  },
};
</script>

<style scoped lang="less">

.main {

  width: 100%;
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
  /deep/.el-dialog {
    box-shadow: none;
  }
  /deep/.el-dialog,
  .el-pager li {
    background: none;
  }
  .line0 {
    padding: 0 30px;
  }
  .line1 {
    width: 100%;
    padding: 0 30px;
    div {
      display: flex;
      width: 100%;
      background: #f7f7f7;
      height: 40px;
      align-items: center;
      p {
        width: 129px;
        display: flex;
        align-items: center;
        justify-content: center;
        height: 100%;
        /*对所有属性过渡0.2s*/
        transition: all 0.2s;
        &:hover {
          cursor: pointer;
        }
      }
    }
  }
  .line2 {
    padding: 0 30px;
    margin-top: 15px;
    width: 100%;

    div {
      border-bottom: 1px solid rgba(0, 0, 0, 0.1);
      display: flex;
      height: 42px;
      align-items: center;
      position: relative;
      p {
        width: 129px;
        display: flex;
        align-items: center;
        justify-content: center;
        height: 100%;
        /*对所有属性过渡0.2s*/
        &:hover {
          cursor: pointer;
        }
      }
      .btm {
        position: absolute;
        width: 129px;
        height: 100%;
        border-bottom: 2px solid #2778be;
        /*对left属性过渡5秒*/
        transition: left 0.5s;
      }
    }
  }
  .line3 {
    margin-top: 30px;
  }
  .line4 {
    margin-top: 30px;
    padding: 0 30px;
  }
}

.active {
  background: #2778be;
  color: white;
}
.active2 {
  color: #2572b5;
  /*border-bottom: 2px solid #2572B5;*/
}
</style>
