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
          <p>再次下发说明</p>
          <img
            src="~@/assets/img/close.png"
            @click="Issueshow = false"
            class="point"
          />
        </div>
        <div class="addBody">
          <div style="display: flex; margin: 30px 0">
            <p style="margin-right: 20px">
              <span style="color: red">*</span>下发说明:
            </p>
            <el-input
              type="textarea"
              :rows="8"
              placeholder="请输入二次下发内容"
              v-model="info.reissueReport"
              style="width: 327px"
            ></el-input>
          </div>

          <div style="display: flex">
            <p style="margin-right: 20px">
              <span style="color: red">*</span>下发图片:
            </p>
            <el-upload
              style="max-width: 327px"
              multiple
              ref="upload"
              action="/general/oss/upload"
              accept="image/png,image/gif,image/jpg,image/jpeg"
              list-type="picture-card"
              :auto-upload="true"
              :before-upload="handleBeforeUpload"
              :on-preview="handlePictureCardPreview"
              :on-remove="handleRemove"
              :on-success="handSuccess"
            >
              <i class="el-icon-plus"></i>
            </el-upload>
            <!-- <el-dialog
              :visible.sync="dialogVisible"
              :modal-append-to-body="false"
            >
              <img width="100%" :src="dialogImageUrl" alt="" />
            </el-dialog> -->
          </div>

          <div class="btn">
            <div class="cancel button" @click="Issueshow = false">取消</div>
            <div class="confirm button" @click="handleconfirmIssue">确认</div>
          </div>
        </div>
      </div>
    </div>
    <div class="content">
      <div class="contitle">
        <span>最新整改情况</span><span class="sontitle"></span>
      </div>
      <h3 style="padding: 16px 0px 0px 10px">下发内容</h3>

      <div class="conwrapper">
        <div style="display: flex">
          <div class="lefttextpart">
            <div class="itemstyle">整改单号：{{ info.cd }}</div>
            <div class="itemstyle">整改单位：{{ info.orgEnterNm }}</div>
            <!-- <div class="itemstyle">整改日期： {{ info.rectifyTm }}</div> -->
            <div class="itemstyle">检查区域：{{ info.inspArea }}</div>
            <div class="itemstyle">
              <div style="width: 70px">整改要求：</div>
              <span style="white-space: pre-line; flex: 1">{{
                info.rectifyDemand
              }}</span>
            </div>
          </div>
          <div class="righttextpart">
            <div class="itemstyle">整改名称：{{ info.nm }}</div>
            <div
              class="itemstyle"
              v-if="info.rectifyerSign && info.rectifyerSign.includes('http')"
            >
              整改责任人：
              <img
                :src="info.rectifyerSign"
                alt=""
                style="object-fit: fill; width: 60px; height: 40px"
                v-viewer
              />
            </div>
            <div class="itemstyle" v-else>
              整改责任人：
              {{ info.rectifyerSign }}
            </div>
            <div class="itemstyle">
              下发日期：<span v-if="info.issueTm">{{
                info.issueTm.slice(0, 10)
              }}</span>
            </div>
            <!-- <div class="itemstyle">
              隐患说明：<span style="white-space: pre-line">{{
                info.explains
              }}</span>
            </div> -->
          </div>
        </div>
        <div class="itemstyle">隐患图片</div>
        <div class="problempiclist" v-viewer>
          <img
            :src="item"
            class="dangerpic"
            alt=""
            v-for="(item, index) in hiddenPics"
            :key="index"
          />
        </div>
        <div class="lefttextpart">
          <div
            class="itemstyle"
            v-if="info.reviewerSign && info.reviewerSign.includes('http')"
          >
            复查人：
            <img
              :src="info.reviewerSign"
              alt=""
              style="object-fit: fill; width: 60px; height: 40px"
              v-viewer
            />
          </div>
          <div class="itemstyle" v-else>复查人：{{ info.reviewerSign }}</div>
          <div class="itemstyle">检验检测单位：{{ info.orgTestEnterNm }}</div>
          <div class="itemstyle">
            检查时间：<span v-if="info.reviewerTm">{{
              info.reviewerTm.slice(0, 10)
            }}</span>
          </div>
        </div>
      </div>
      <div v-if="info.rectifyReport" style="padding-bottom: 10px">
        <h3 style="padding: 0px 0px 20px 10px">整改上报内容</h3>
        <div
          class="itemstyle"
          style="
            padding-bottom: 2px;
            font-size: 15px;
            margin-bottom: 5px;
            padding-left: 30px;
          "
        >
          整改上报：
          <span
            style="
              margin-left: 5px;
              font-size: 14px;
              font-weight: 400;
              white-space: pre-line;
            "
            >{{ info.rectifyReport }}</span
          >
        </div>
        <div
          style="
            padding-bottom: 2px;
            font-size: 15px;
            margin-bottom: 5px;
            padding-left: 30px;
          "
        >
          执行图片：
        </div>
        <div v-viewer class="problempiclist" style="padding-left: 30px">
          <img
            class="dangerpic"
            :src="item3"
            v-for="(item3, index3) in (info.rectifyImg ? info.rectifyImg : '')
              .split(',')
              .filter((item4) => item4 != '')"
            :key="index3"
          />
        </div>
        <div
          style="
            padding-bottom: 2px;
            font-size: 15px;
            margin-bottom: 5px;
            padding-left: 30px;
          "
        >
          整改日期:
          <span
            v-if="info.rectifyTm"
            style="margin-left: 5px; font-size: 14px; font-weight: 400"
            >{{ info.rectifyTm.slice(0, 10) }}</span
          >
        </div>
      </div>

      <div class="contitle">整改执行情况<span class="sontitle"></span></div>
      <!-- 船厂待执行开始 -->
      <div
        class="conwrapper"
        v-if="
          currentRole &&
          currentRole.identityCd == 'identity30' &&
          info.state == 2
        "
      >
        <div style="display: flex; padding: 15px 0">
          <div class="itemstyle" style="width: 104px">
            <span style="color: red">*</span>整改上报：
          </div>
          <el-input
            type="textarea"
            :rows="5"
            placeholder="请输入内容"
            v-model="info.rectifyReport"
            style="width: 277px"
          ></el-input>
        </div>
        <div style="display: flex; padding: 15px 0">
          <div class="itemstyle" style="width: 104px">
            <span style="color: red">*</span>执行图片：
          </div>
          <el-upload
            style="margin-top: 20px"
            multiple="true"
            ref="upload"
            action="/general/oss/upload"
            accept="image/png,image/gif,image/jpg,image/jpeg"
            list-type="picture-card"
            :auto-upload="true"
            :before-upload="handleBeforeUpload"
            :on-preview="handlePictureCardPreview"
            :on-remove="handleRemove"
            :on-success="handSuccess"
          >
            <i class="el-icon-plus"></i>
          </el-upload>
        </div>
        <el-dialog :visible.sync="dialogVisible" :modal-append-to-body="false">
          <img width="100%" :src="dialogImageUrl" alt="" />
        </el-dialog>
        <!--
          富文本
           <vue-ueditor-wrap
          v-model="info.rectifyReport"
          style="z-index: 99; width: 100%"
          :config="myConfig"
        ></vue-ueditor-wrap> -->
        <div style="display: flex; padding: 15px 0">
          <div class="itemstyle" style="width: 104px">备注内容：</div>
          <el-input
            type="textarea"
            :rows="5"
            placeholder="请输入内容"
            v-model="info.rmks"
            style="width: 277px"
          ></el-input>
        </div>
        <div class="btn">
          <!-- <div class="cancel button">取消</div> -->
          <div class="confirm button" @click="handleRectifyReport">
            确定上报
          </div>
        </div>
      </div>
      <!--  船厂待执行结束  -->
      <!-- 检验检测待执行开始 -->
      <div
        class="conwrapper"
        v-else-if="
          currentRole &&
          currentRole.identityCd == 'identity50' &&
          info.state == 2
        "
      >
        <div class="itemstyle">
          整改状态：<span style="color: #e4393c">待执行</span>
        </div>
      </div>
      <!-- 检验检测待执行结束 -->
      <!-- 除检验检测待执行开始 -->
      <div
        class="conwrapper"
        v-if="
          !(
            currentRole &&
            currentRole.identityCd == 'identity50' &&
            info.state == 2
          )
        "
      >
        <div
          class="item_border"
          v-for="(item, index) in reissueList"
          :key="index"
        >
          <img
            style="
              position: absolute;
              top: 16px;
              right: 44px;
              width: 12px;
              height: 7px;
            "
            :class="{
              arrowTransform: !item.isshow,
              arrowTransformReturn: item.isshow,
            }"
            src="~@/assets/img/personal/下拉.png"
            alt=""
            @click="item.isshow = !item.isshow"
          />
          <!-- <div v-if="reissueList.length > 1 && item.reissueReport"> -->
          <!-- 第一次下发 -->
          <div v-show="item.isshow && index == 0">
            <h3 style="padding: 20px 0px; margin-left: -16px">下发内容</h3>
            <div
              style="
                padding-bottom: 2px;
                font-size: 15px;
                margin-bottom: 5px;
                display: flex;
              "
            >
              整改要求:
              <span
                style="
                  margin-left: 5px;
                  font-size: 14px;
                  font-weight: 400;
                  white-space: pre-line;
                "
                >{{ item.rectifyDemand }}</span
              >
            </div>
            <!-- <div
              style="padding-bottom: 2px; font-size: 15px; margin-bottom: 5px;display:flex;"
            >
              隐患说明:
              <span
                style="margin-left: 5px; font-size: 14px; font-weight: 400;white-space: pre-line;"
                >{{ item.explains }}</span
              >
            </div> -->
            <div v-viewer class="problempiclist">
              <img
                :src="item1"
                class="dangerpic"
                alt=""
                v-for="(item1, index1) in (item.troubleImg
                  ? item.troubleImg
                  : ''
                )
                  .split(',')
                  .filter((item2) => item2 != '')"
                :key="index1"
              />
            </div>
            <!-- <div
              style="padding-bottom: 2px; font-size: 15px; margin-bottom: 5px"
            >
              下发日期:
              <span
                style="margin-left: 5px; font-size: 14px; font-weight: 400"
                v-if="item.issueTm"
                >{{ item.issueTm.slice(0, 10) }}</span
              >
            </div> -->
            <!-- <div
              style="
                padding-bottom: 2px;
                font-size: 15px;
                font-weight: bold;
                margin-bottom: 5px;
              "
            >
              复查日期:<span
                style="margin-left: 5px; font-size: 14px; font-weight: 400"
                >{{ item.reviewerTm }}</span
              >
            </div>
            <div
              style="
                padding-bottom: 2px;
                font-size: 15px;
                font-weight: bold;
                margin-bottom: 5px;
              "
            >
              下发日期:
              <span
                style="margin-left: 5px; font-size: 14px; font-weight: 400"
                >{{ item.issueTm }}</span
              >
            </div> -->
          </div>
          <!-- 二次下发 -->
          <div v-show="item.isshow && index > 0">
            <h3 style="padding: 20px 0px; margin-left: -16px">再次下发内容</h3>
            <div
              style="
                padding-bottom: 2px;
                font-size: 15px;
                margin-bottom: 5px;
                display: flex;
              "
            >
              下发说明:
              <span
                style="
                  margin-left: 5px;
                  font-size: 14px;
                  font-weight: 400;
                  white-space: pre-line;
                "
                >{{ item.rectifyDemand }}</span
              >
            </div>
            <div v-viewer class="problempiclist">
              <img
                :src="item1"
                class="dangerpic"
                alt=""
                v-for="(item1, index1) in (item.reissueImg
                  ? item.reissueImg
                  : ''
                )
                  .split(',')
                  .filter((item2) => item2 != '')"
                :key="index1"
              />
            </div>

            <!-- <div
              style="
                padding-bottom: 2px;
                font-size: 15px;
                font-weight: bold;
                margin-bottom: 5px;
              "
            >
              复查日期:<span
                style="margin-left: 5px; font-size: 14px; font-weight: 400"
                >{{ item.reviewerTm }}</span
              >
            </div>
            <div
              style="
                padding-bottom: 2px;
                font-size: 15px;
                font-weight: bold;
                margin-bottom: 5px;
              "
            >
              下发日期:
              <span
                style="margin-left: 5px; font-size: 14px; font-weight: 400"
                >{{ item.issueTm }}</span
              >
            </div> -->
          </div>

          <!-- <div class="conwrapper" v-else>
            <div
              class="item_border"
              v-for="(item, index) in reissueList"
              :key="index"
            >
              <div v-if="reissueList.length > 1 && item.reissueReport">
                下发说明:{{ item.reissueReport }}
              </div> -->

          <div v-show="item.isshow && item.rectifyReport">
            <h3 style="padding: 20px 0px; margin-left: -16px">整改上报内容</h3>
            <div
              class="itemstyle"
              style="padding-bottom: 2px; font-size: 15px; margin-bottom: 5px"
            >
              整改上报：
              <span
                style="
                  margin-left: 5px;
                  font-size: 14px;
                  font-weight: 400;
                  white-space: pre-line;
                "
                >{{ item.rectifyReport }}</span
              >
            </div>
            <div
              style="padding-bottom: 2px; font-size: 15px; margin-bottom: 5px"
            >
              执行图片：
            </div>
            <div v-viewer class="problempiclist">
              <img
                class="dangerpic"
                :src="item3"
                v-for="(item3, index3) in (item.rectifyImg
                  ? item.rectifyImg
                  : ''
                )
                  .split(',')
                  .filter((item4) => item4 != '')"
                :key="index3"
              />
            </div>
            <div
              style="padding-bottom: 2px; font-size: 15px; margin-bottom: 5px"
            >
              整改日期:
              <span
                v-if="item.rectifyTm"
                style="margin-left: 5px; font-size: 14px; font-weight: 400"
                >{{ item.rectifyTm.slice(0, 10) }}</span
              >
            </div>
          </div>
        </div>
        <!-- <div class="itemstyle">整改上报：{{ info.rectifyReport }}</div>
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
        </div> -->
      </div>
      <!-- 除检验检测待执行结束 -->
      <!--  船厂已完成、检验检测已完成、待结案才有结案标题开始-->
      <div
        class="contitle"
        v-if="
          info.state == 4 ||
          (info.state == 3 &&
            currentRole &&
            currentRole.identityCd == 'identity50')
        "
      >
        结案<span class="sontitle"></span>
      </div>
      <!--  船厂已完成、检验检测已完成、待结案才有结案标题结束-->

      <!-- 检验检测、船厂已完成开始 -->
      <div class="conwrapper" v-if="info.state == 4">
        <div class="lefttextpart">
          <div class="itemstyle">
            <div style="width: 70px">结案意见：</div>
            <span style="white-space: pre-line; flex: 1">{{
              info.closeReport
            }}</span>
          </div>
          <div
            class="itemstyle"
            v-if="info.reviewerSign && info.reviewerSign.includes('http')"
          >
            签发人：
            <img
              :src="info.reviewerSign"
              alt=""
              style="object-fit: fill; width: 60px; height: 40px"
              v-viewer
            />
          </div>
          <div class="itemstyle" v-else>签发人： {{ info.closeUserNm }}</div>
          <div class="itemstyle">
            确认日期：{{ info.closeDate.slice(0, 10) }}
          </div>
          <div class="itemstyle">检验检测单位：{{ info.orgTestEnterNm }}</div>
        </div>
      </div>
      <!-- 检验检测、船厂已完成结束 -->
      <!-- 检验检测待结案开始 -->
      <div
        class="conwrapper"
        v-else-if="
          info.state == 3 &&
          currentRole &&
          currentRole.identityCd == 'identity50'
        "
      >
        <div style="display: flex; padding: 15px 0">
          <div class="itemstyle">
            <span style="color: red">*</span>结案内容：
          </div>
          <el-input
            type="textarea"
            :rows="5"
            placeholder="请输入内容"
            v-model="info.closeReport"
            style="width: 277px"
          ></el-input>
        </div>
        <div class="closeCaseBtn">
          <div class="jabtn button" @click="handleRectifyclose">确认结案</div>
          <div class="nextIssuebtn button" @click="Issueshow = true">
            再次下发
          </div>
          <!-- <div class="cancelbtn button">取消</div> -->
        </div>
      </div>
      <!-- 检验检测待结案结束 -->
      <!-- </div>
      </div> -->
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
      // isshow: true,
      id: 1,
      title: "整改单",
      Issueshow: false,
      // 控制图片预览
      dialogVisible: false,
      // 预览路径
      dialogImageUrl: "",
      myConfig: {
        // 编辑器不自动被内容撑高
        autoHeightEnabled: false,
        // 初始容器高度
        initialFrameHeight: 440,
        // 初始容器宽度
        initialFrameWidth: "100%",
        // 上传文件接口（这个地址是我为了方便各位体验文件上传功能搭建的临时接口，请勿在生产环境使用！！！）
        serverUrl: "https://www.ship88.cn/general/ueditor/exec",
        // serverUrl: "https://www.ship88.cn/general/oss/upload",

        UEDITOR_HOME_URL: "/sinovat2/UEditor/",
        // UEditor 资源文件的存放路径，如果你使用的是 vue-cli 生成的项目，通常不需要设置该选项，vue-ueditor-wrap 会自动处理常见的情况，如果需要特殊配置，参考下方的常见问题2
        // UEDITOR_HOME_URL:
        //   process.env.NODE_ENV === "production"
        //     ? "/static/ueditor/"
        //     : "/static/ueditor/"
      },
      // 再次下发列表
      reissueList: [],
      info: {},
    };
  },
  layout: "person",
  async mounted() {
    this.id = this.until.getQueryString("id");
    let data = await this.api.getRectifyDetail(this.id);
    this.info = data.shipDocsRectifyVo;
    this.reissueList = data.reissueList;
    this.reissueList.pop();
    this.reissueList.forEach((item) => {
      this.$set(item, "isshow", true);
    });
    setTimeout(() => {}, 2000);
  },
  computed: {
    ...mapState(["currentRole"]),
    hiddenPics() {
      if (this.info.troubleImg) return this.info.troubleImg.split(",");
      return [];
    },
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
      if (this.currentRole && this.currentRole.identityCd == "identity30") {
        this.info.rectifyImg = fileList
          .map((item) => item.response.data)
          .join(",");
      } else if (
        this.currentRole &&
        this.currentRole.identityCd == "identity50"
      ) {
        this.info.reissueImg = fileList
          .map((item) => item.response.data)
          .join(",");
      }
    },
    handlePictureCardPreview(file) {
      this.dialogImageUrl = file.url;
      this.dialogVisible = true;
    },
    handSuccess(response, file, fileList) {
      console.log("图片上传", fileList);
      if (this.currentRole && this.currentRole.identityCd == "identity30") {
        this.info.rectifyImg = fileList
          .map((item) => item.response.data)
          .join(",");
        console.log("整改上报", this.info.rectifyImg, fileList);
      } else if (
        this.currentRole &&
        this.currentRole.identityCd == "identity50"
      ) {
        this.info.reissueImg = fileList
          .map((item) => item.response.data)
          .join(",");
      }
    },
    // 再次下发
    async handleconfirmIssue() {
      if (!this.info.reissueReport || !this.info.reissueImg) {
        this.$message.error("请将信息填写完整");
      } else {
        let data = await this.api.handleRectifyReturn({
          inspId: this.reissueList[this.reissueList.length - 1].id,
          id: this.id,
          cd: this.info.cd,
          reissueImg: this.info.reissueImg,
          reissueReport: this.info.reissueReport,
        });
        if (data.code == 0) {
          this.$message({
            message: "再次下发成功",
            type: "success",
          });
          let data1 = await this.api.getRectifyDetail(this.id);
          this.info = data1.shipDocsRectifyVo;
          this.reissueList = data1.reissueList;
          this.reissueList.forEach((item) => {
            this.$set(item, "isshow", true);
          });
          this.Issueshow = false;
        } else {
          this.$message.error("再次下发失败");
        }
      }
    },
    // 整改上报
    async handleRectifyReport() {
      if (!this.info.rectifyReport) {
        this.$message.error("整改内容不能为空");
      } else if (!this.info.rectifyImg) {
        this.$message.error("执行图片不能为空");
      } else {
        let obj = {};
        //再下发说明列表
        if (this.reissueList.length > 1) {
          obj = {
            inspId: this.reissueList[this.reissueList.length - 1].id,
          };
        }
        let res = await this.api.handleRectifyReport({
          ...{
            id: this.id,
            cd: this.info.cd,
            rectifyReport: this.info.rectifyReport,
            rmks: this.info.rmks,
            rectifyImg: this.info.rectifyImg,
          },
          ...obj,
        });
        if (res.code == 0) {
          this.$message({
            message: "上报成功",
            type: "success",
          });
          let data = await this.api.getRectifyDetail(this.id);
          this.info = data.shipDocsRectifyVo;
          this.reissueList = data.reissueList;
          this.reissueList.pop();
          this.reissueList.forEach((item) => {
            this.$set(item, "isshow", true);
          });
        } else {
          this.$message.error("上报失败");
        }
      }
    },
    // 确认结案
    async handleRectifyclose() {
      if (!this.info.closeReport) {
        this.$message.error("结案内容不能为空");
      } else {
        let res = await this.api.handlerectifyClose({
          id: this.id,
          closeReport: this.info.closeReport,
        });

        if (res.code == 0) {
          this.$message({
            message: "结案成功",
            type: "success",
          });
          let data = await this.api.getRectifyDetail(this.id);
          this.info = data.shipDocsRectifyVo;
          this.reissueList = data.reissueList;
          this.reissueList.pop();
          this.reissueList.forEach((item) => {
            this.$set(item, "isshow", true);
          });
        } else {
          this.$message.error("结案失败");
        }
      }
    },
  },
};
</script>
<style lang="less" scoped>
</style>
<style scoped lang="less">
.main {
  width: 100%;
  .button:hover {
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
        padding: 0 30px;
        box-sizing: border-box;
        // align-items: center;
        .btn {
          margin: 40px auto;
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
      padding: 8px 0 7px 34px;
      .item_border {
        position: relative;
        min-height: 35px;
        border-bottom: 1px solid rgba(0, 0, 0, 0.1);
      }
      .item_border:last-child {
        border: none;
      }
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
        display: flex;
        // align-items: center;
      }
      .problempiclist {
        width: 100%;
        display: flex;
        flex-wrap: wrap;
        .dangerpic {
          object-fit: cover;
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
          margin-left: 30px;
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
      .arrowTransform {
        transition: 0.2s;
        transform-origin: center;
        transform: rotateZ(180deg);
      }
      .arrowTransformReturn {
        transition: 0.2s;
        transform-origin: center;
        transform: rotateZ(0deg);
      }
    }
  }
}
</style>
