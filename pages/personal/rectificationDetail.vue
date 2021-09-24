<template>
  <!-- 整改详情 -->
  <div class="main">
    <div style="padding: 0 30px">
      <tobbar :title="title" :showAll="false"></tobbar>
    </div>
    <p style="border-bottom: 1px solid rgba(0, 0, 0, 0.1)"></p>
    <div id="Issueshow" v-if="Issueshow">
      <div class="addBox">
        <div class="head">
          <p>下发说明</p>
          <img
            src="~@/assets/img/close.png"
            @click="Issueshow = false"
            class="point"
          />
        </div>
        <div class="addBody">
          <el-input
            type="textarea"
            :rows="8"
            placeholder="请输入二次下发内容"
            v-model="info.desc"
            style="width: 386px; margin-top: 30px"
          ></el-input>
          <el-upload
            style="margin-top: 20px;"
            ref="upload"
            action="/general/oss/upload"
            accept="image/png,image/gif,image/jpg,image/jpeg"
            list-type="picture-card"
            :auto-upload="false"
            :before-upload="handleBeforeUpload"
            :on-preview="handlePictureCardPreview"
            :on-remove="handleRemove"
            :on-success="handSuccess"
          >
            <i class="el-icon-plus"></i>
          </el-upload>
          <el-dialog :visible.sync="dialogVisible" :modal-append-to-body="false">
            <img width="100%" :src="dialogImageUrl" alt="" />
          </el-dialog>
          <div class="btn">
            <div class="cancel button" @click="Issueshow=false">取消</div>
            <div class="confirm button" @click="handleconfirmIssue">确认</div>
          </div>
        </div>
      </div>
    </div>
    <div class="content">
      <div class="contitle">整改单<span class="sontitle"></span></div>
      <div class="conwrapper">
        <div style="display: flex">
          <div class="lefttextpart">
            <div class="itemstyle">整改单号：{{ info.cd }}</div>
            <div class="itemstyle">整改单位：{{ info.unit }}</div>
            <div class="itemstyle">整改日期： {{ info.zgdate }}</div>
            <div class="itemstyle">检查区域：{{ info.jcarea }}</div>
            <div class="itemstyle">整改要求：{{ info.zgrequire }}</div>
          </div>
          <div class="righttextpart">
            <div class="itemstyle">整改名称：{{ info.zgname }}</div>
            <div class="itemstyle">整改责任人：{{ info.zgperson }}</div>
            <div class="itemstyle">下发日期：{{ info.xfdate }}</div>
            <div class="itemstyle">隐患说明：{{ info.yhintro }}</div>
          </div>
        </div>
        <div class="itemstyle">隐患图片</div>
        <div class="problempiclist" v-viewer>
          <img
            :src="item"
            class="dangerpic"
            alt=""
            v-for="(item, index) in info.ygpiclist"
            :key="index"
          />
        </div>
        <div class="lefttextpart">
          <div class="itemstyle">复查人：{{ info.fcperson }}</div>
          <div class="itemstyle">检验检测单位：{{ info.jyunit }}</div>
          <div class="itemstyle">检查时间：{{ info.jcdate }}</div>
        </div>
      </div>
      <div class="contitle">整改执行情况<span class="sontitle"></span></div>
      <!-- 船厂待执行开始 -->
      <div
        class="conwrapper"
        v-if="
          currentRole &&
          currentRole.identityCd == 'identity30' &&
          info.status == 1
        "
      >
        <div class="itemstyle">整改上报</div>
        <vue-ueditor-wrap
          v-model="info.fulltext"
          style="z-index: 99; width: 100%"
          :config="info.myConfig"
        ></vue-ueditor-wrap>
        <div style="display: flex; padding: 15px 0">
          <div class="itemstyle">备注内容：</div>
          <el-input
            type="textarea"
            :rows="5"
            placeholder="请输入内容"
            v-model="info.desc"
            style="width: 277px"
          ></el-input>
        </div>
        <div class="btn">
          <div class="confirm button">确定</div>
          <div class="cancel button">取消</div>
        </div>
      </div>
      <!--  船厂待执行结束  -->
      <!-- 检验检测待执行开始 -->
      <div
        class="conwrapper"
        v-else-if="
          currentRole &&
          currentRole.identityCd == 'identity50' &&
          info.status == 1
        "
      >
        <div class="itemstyle">
          整改状态：<span style="color: #e4393c">待执行</span>
        </div>
      </div>
      <!-- 检验检测待执行结束 -->
      <!-- 检验检测、船厂其他情况开始 -->
      <div class="conwrapper" v-else>
        <div class="itemstyle">整改上报：{{ info.zgsb }}</div>
        <div class="problempiclist" v-viewer>
          <img
            :src="item"
            v-for="(item, index) in info.ygpiclist"
            :key="index"
            class="dangerpic"
            alt=""
          />
        </div>
        <div class="lefttextpart">
          <div class="itemstyle">上报日期{{ info.zgyj }}</div>
          <div class="itemstyle">确认人： {{ info.qfr }}</div>
          <div class="itemstyle">确认日期：{{ info.confirmdate1 }}</div>
        </div>
      </div>
      <!-- 检验检测、船厂其他情况结束 -->
      <!--  船厂已完成、检验检测已完成、待结案才有结案标题开始-->
      <div
        class="contitle"
        v-if="
          info.status == 3 ||
          (info.status == 2 &&
            currentRole &&
            currentRole.identityCd == 'identity50')
        "
      >
        结案<span class="sontitle"></span>
      </div>
      <!--  船厂已完成、检验检测已完成、待结案才有结案标题结束-->

      <!-- 检验检测、船厂已完成开始 -->
      <div class="conwrapper" v-if="info.status == 3">
        <div class="lefttextpart">
          <div class="itemstyle">结案意见：{{ info.zgyj }}</div>
          <div class="itemstyle">签发人： {{ info.qfr }}</div>
          <div class="itemstyle">确认日期：{{ info.confirmdate2 }}</div>
          <div class="itemstyle">检验检测单位：{{ info.jcunit }}</div>
        </div>
      </div>
      <!-- 检验检测、船厂已完成结束 -->
      <!-- 检验检测待结案开始 -->
      <div
        class="conwrapper"
        v-else-if="
          info.status == 2 &&
          currentRole &&
          currentRole.identityCd == 'identity50'
        "
      >
        <div style="display: flex; padding: 15px 0">
          <div class="itemstyle">结案内容：</div>
          <el-input
            type="textarea"
            :rows="5"
            placeholder="请输入内容"
            v-model="info.jacontent"
            style="width: 277px"
          ></el-input>
        </div>
        <div class="closeCaseBtn">
          <div class="jabtn button">确认结案</div>
          <div class="nextIssuebtn button" @click="Issueshow = true">下次下发</div>
          <div class="cancelbtn button">取消</div>
        </div>
      </div>
      <!-- 检验检测待结案结束 -->
    </div>
  </div>
</template>

<script>
import tobbar from "../../components/person/tobbar";
import VueUeditorWrap from "vue-ueditor-wrap";
import { mapState } from "vuex";

export default {
  name: "allShip",
  components: {
    tobbar,
    VueUeditorWrap,
  },
  data() {
    return {
      id: 1,
      title: "整改单",
      Issueshow: false,
      // 控制图片预览
      dialogVisible: false,
      // 预览路径
      dialogImageUrl: "",

      info: {
        cd: "ZG20210330001",
        unit: "澳新船厂有限公司",
        zgdate: "2021年3月25日",
        jcarea: "消防通道",
        zgrequire: "请按照要求进行整改",
        zgname: "消防通道畅通整改",
        zgperson: "张三",
        xfdate: "2021年3月25日",
        yhintro: "消防通道堵塞",
        ygpiclist: [
          "https://img0.baidu.com/it/u=1493112636,2334046030&fm=26&fmt=auto",
          "https://img0.baidu.com/it/u=1493112636,2334046030&fm=26&fmt=auto",
          "https://img0.baidu.com/it/u=1493112636,2334046030&fm=26&fmt=auto",
          "https://img0.baidu.com/it/u=1493112636,2334046030&fm=26&fmt=auto",
          "https://img0.baidu.com/it/u=1493112636,2334046030&fm=26&fmt=auto",
        ],
        fcperson: "章老师",
        jyunit: "船级社检测机构",
        jcdate: "2021年03月25日",
        zgsb: "已按整改要求完成整改。",
        sbdate: "2021年03月25日",
        confirmer: "张三",
        confirmdate1: "2021年03月25日",
        zgyj: "整改有效，允许结案。",
        qfr: "章老师",
        confirmdate2: "2021年03月25日",
        jcunit: "船级社检测机构",
        // 完成情况，1:待执行，2：待结案，3：已完成
        status: 2,
        desc: "",
        jacontent: "",
        fulltext: "",
        myConfig: {
          // 编辑器不自动被内容撑高
          autoHeightEnabled: false,
          // 初始容器高度
          initialFrameHeight: 440,
          // 初始容器宽度
          initialFrameWidth: "100%",
          // 上传文件接口（这个地址是我为了方便各位体验文件上传功能搭建的临时接口，请勿在生产环境使用！！！）
          serverUrl: "https://www.ship88.cn/general/ueditor/exec",
          UEDITOR_HOME_URL: "/sinovat/UEditor/",
          // UEditor 资源文件的存放路径，如果你使用的是 vue-cli 生成的项目，通常不需要设置该选项，vue-ueditor-wrap 会自动处理常见的情况，如果需要特殊配置，参考下方的常见问题2
          // UEDITOR_HOME_URL:
          //   process.env.NODE_ENV === "production"
          //     ? "/static/ueditor/"
          //     : "/static/ueditor/"
        },
        // 下发图片列表
        Issuepiclist: [],
      },
    };
  },
  layout: "person",
  async mounted() {
    this.id = this.until.getQueryString("id");
  },
  computed: {
    ...mapState(["currentRole"]),
  },
  watch: {},
  methods: {
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
          message:
            "请上传格式为image/png, image/gif, image/jpg, image/jpeg的图片",
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
    handleRemove(file, fileList) {
      console.log(2222,fileList);
      this.info.Issuepiclist = [];

      for (let i = 0; i < fileList.length; i++) {
        this.info.Issuepiclist.push(fileList[i].response.data);
      }
      console.log(file, fileList);
    },
    handlePictureCardPreview(file) {
      this.dialogImageUrl = file.url;
      this.dialogVisible = true;
    },
    handSuccess(response, file, fileList) {
      console.log(1324,fileList);
      this.info.Issuepiclist = [];

      for (let i = 0; i < fileList.length; i++) {
        this.info.Issuepiclist.push(fileList[i].response.data);
      }
    },
    handleconfirmIssue(){
      this.Issueshow=false;
    }
  },
};
</script>
<style lang="less" scoped>

</style>
<style scoped lang="less">
.main {
  width: 100%;
  .button:hover{
        cursor: pointer;
    
  }
  #Issueshow {
    position: fixed;
    top: 0;
    left: 0;
    width: 100vw;
    height: 100vh;
    background: rgba(0, 0, 0, 0.5);
    z-index: 1;
    display: flex;
    justify-content: center;
    align-items: center;
    .addBox {
      width: 501px;
      height: 578px;
      background-color: #ffffff;
      display: flex;
      flex-direction: column;
      border-radius: 8px;
      .head {
        display: flex;
        justify-content: space-between;
        align-items: center;
        height: 50px;
        width: 100%;
        padding: 0 30px;
        border-bottom: 1px solid rgba(0, 0, 0, 0.3);
        > p {
          margin: 0 auto;
          font-size: 24px;
          padding-left: 16px;
          // width: 37.4%;
        }
        > img {
          width: 16px;
          height: 16px;
          cursor: pointer;
        }
      }
      .addBody {
        width: 100%;
        flex: 1;
        overflow: auto;
        display: flex;
        flex-direction: column;
        align-items: center;
        .btn {
          margin-top: 40px;
          margin-bottom: 40px;
          display: flex;
          .cancel {
            width: 139px;
            height: 52px;
            background: rgba(228, 57, 60, 0);
            border: 1px solid #dddddd;
            border-radius: 5px;
            font-size: 16px;
            font-weight: 400;
            color: #666666;
            line-height: 52px;
            text-align: center;
            margin-right: 20px;
          }
          .confirm {
            width: 226px;
            height: 52px;
            background: #2778be;
            border-radius: 5px;
            font-size: 16px;
            font-weight: 400;
            color: #ffffff;
            line-height: 52px;
            text-align: center;
          }
        }
      }
    }
  }

  .content {
    background: #fff;
    padding: 30px;
    padding-bottom: 325px;
    box-sizing: border-box;
    .contitle {
      width: 100%;
      display: flex;
      font-size: 16px;
      font-weight: 400;
      color: #333333;
      align-items: center;
    }
    .sontitle {
      margin-left: 5px;
      flex: 1;
      height: 1px;
      background: rgba(0, 0, 0, 0.1);
    }
    .conwrapper {
      width: 100%;
      box-sizing: border-box;
      padding: 22px 0 22px 34px;
      .lefttextpart {
        width: 250px;
        margin-right: 278px;
      }
      .righttextpart {
      }
      .itemstyle {
        font-size: 14px;
        font-weight: 400;
        color: #606060;
        padding: 13px 0;
      }
      .problempiclist {
        width: 100%;
        display: flex;
        flex-wrap: wrap;
        .dangerpic {
          width: 248px;
          height: 177px;
          margin: 10px 0;
          margin-right: 20px;
        }
      }
      .btn {
        display: flex;
        padding-top: 20px;
        margin-left: 70px;
        .confirm {
          width: 86px;
          height: 35px;
          font-size: 16px;
          font-weight: 400;
          color: #ffffff;
          line-height: 35px;
          background: #2778be;
          border-radius: 4px;
          margin-right: 30px;
          text-align: center;
        }
        .cancel {
          text-align: center;
          width: 86px;
          height: 35px;
          background: #ffffff;
          border: 1px solid #dddddd;
          border-radius: 4px;
          font-size: 16px;
          font-weight: 400;
          color: #999999;
          line-height: 35px;
        }
      }
      .closeCaseBtn {
        display: flex;
        padding: 35px 67px;
        .jabtn {
          width: 86px;
          height: 35px;
          background: #e4393c;
          border-radius: 4px;
          font-size: 16px;
          font-weight: 400;
          color: #ffffff;
          line-height: 35px;
          margin-right: 30px;
          text-align: center;
        }
        .nextIssuebtn {
          width: 86px;
          height: 35px;
          background: #2778be;
          border-radius: 4px;
          font-size: 16px;
          font-family: Microsoft YaHei;
          font-weight: 400;
          color: #ffffff;
          line-height: 35px;
          margin-right: 30px;
          text-align: center;
        }
        .cancelbtn {
          text-align: center;
          width: 86px;
          height: 35px;
          background: #ffffff;
          border: 1px solid #dddddd;
          border-radius: 4px;
          font-size: 16px;
          font-weight: 400;
          color: #999999;
          line-height: 35px;
        }
      }
    }
  }
}
</style>
