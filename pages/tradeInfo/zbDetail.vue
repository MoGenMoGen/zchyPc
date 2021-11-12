<template>
  <div id="home" :style="{ width: bWidth + 'px' }">
    <div class="header">
      <el-carousel :height="bannerHeight + 'px'" class="adert">
        <el-carousel-item v-for="item in adertList" :key="item.aiid">
          <div
            class="bannerImg"
            :style="{ backgroundImage: 'url(' + item.imgUrl + ')' }"
            @click="toUrl(item.locUrl)"
          />
        </el-carousel-item>
      </el-carousel>
    </div>
    <div class="content main" :style="{ width: width + 'px' }">
      <div class="partA">
        <info-menu></info-menu>
      </div>
      <div class="partB">
        <div class="head">
          <p>
            <span @click="back"><i class="el-icon-arrow-left"></i>返回</span>
          </p>
        </div>
        <div class="top-menu">
          <div
            v-for="(item, index) in info"
            :key="index"
            class="top-menu-item"
            @click="selectThis(index)"
          >
            <img :src="item.selectImgUrl" v-if="selectIndex >= index" />
            <img :src="item.imgUrl" v-else />
            <div>
              <span :style="{ color: selectIndex >= index ? '#0064B2' : '' }">{{
                item.afficheTypeNm
              }}</span>
              <span>{{ item.releTm }}</span>
            </div>
            <img
              :src="jt1"
              v-if="selectIndex >= index && info.length - 1 > index"
            />
            <img
              :src="jt2"
              v-if="selectIndex < index && info.length - 1 > index"
            />
          </div>
        </div>
        <div class="body" v-html="cont"></div>
        <div class="bottom-btn" v-if="signFlag && !IsSignUp" @click="sign">
          报名
        </div>
        <div class="complated-btn" v-else-if="IsSignUp">已报名</div>
      </div>
    </div>
    <div class="footerImg" :style="{ width: bWidth + 'px' }">
      <img src="../../assets/img/footer.png" alt="" />
    </div>
    <el-dialog title="提示" :visible.sync="dialogVisible" width="30%">
      <span
        >当前帐号：{{ currentRole.company }}[{{ currentRole.identityNm }}]</span
      >
      <p style="color: #e4393c">如需切换，请去个人中心切换</p>
      <span slot="footer" class="dialog-footer">
        <el-button @click="dialogVisible = false">取 消</el-button>
        <el-button type="primary" @click="confirm">确 定</el-button>
      </span>
    </el-dialog>
  </div>
</template>

<script>
import { MessageBox } from "element-ui";
import InfoMenu from "./components/infoMenu";
import share from "@/components/share";
import { mapState } from "vuex";
import cg1 from "@/assets/img/bidAffiche/采购公告.png";
import gz1 from "@/assets/img/bidAffiche/更正公告.png";
import gz2 from "@/assets/img/bidAffiche/更正公告-灰.png";
import jg1 from "@/assets/img/bidAffiche/结果公告.png";
import jg2 from "@/assets/img/bidAffiche/结果公告-灰.png";
import jt1 from "@/assets/img/bidAffiche/箭头.png";
import jt2 from "@/assets/img/bidAffiche/箭头-灰.png";
export default {
  name: "infoDetail",
  components: { InfoMenu, share },
  data() {
    return {
      jt1,
      jt2,
      bannerHeight: 346,
      infoId: "",
      choose: "",
      adertList: [],
      info: [],
      selectIndex: 0,
      cont: "",
      signFlag: false, //是否不能报名
      dialogVisible: false,
      IsSignUp: false, //是否已报名
      currentRole: {},
      bidInfo: {},
    };
  },
  computed: {
    ...mapState(["bWidth", "width"]),
  },
  watch: {
    $route() {
      this.infoId = this.$route.query.id;
      this.choose = this.$route.query.cid;
    },
  },
  async asyncData(context) {
    if (process.client) {
      return;
    }
    let id = context.query.id;
    let data = await context.app.api.detailInfo(id);
    return {
      data: data,
    };
  },
  mounted() {
    this.infoId = this.$route.query.id;
    this.choose = this.$route.query.cid;
    this.getAdert();
    this.getData();
    this.canSign();
  },
  methods: {
    toUrl(url) {
      if (url) {
        window.location.href = url;
      }
    },
    async getAdert() {
      this.adertList = await this.api.getAdert("PChyxx");
      if (this.adertList.length > 0) {
        let myImg = new Image();
        myImg.src = this.adertList[0].imgUrl;
        myImg.onload = () => {
          // this.bannerHeight = myImg.height/myImg.width*this.bWidth
          this.bannerHeight = myImg.height;
        };
      }
    },
    toPage(url) {
      this.$router.push(url);
    },
    back() {
      this.$router.go(-1);
    },
    getData() {
      let qry = this.query.new();
      this.query.toW(qry, "bidId", this.infoId, "EQ");
      this.query.toO(qry, "afficheTypeCd", "esc");
      this.api.getBidAfficheList(this.query.toEncode(qry)).then((res) => {
        res.data.list.forEach((item) => {
          item.releTm = item.releTm.substring(0, 10);
          if (item.afficheTypeCd == "5635882628584448") {
            item.selectImgUrl = cg1;
            item.imgUrl = "";
          } else if (item.afficheTypeCd == "5635883070706688") {
            item.selectImgUrl = gz1;
            item.imgUrl = gz2;
          } else if (item.afficheTypeCd == "5635883361522688") {
            item.selectImgUrl = jg1;
            item.imgUrl = jg2;
          }
          item.cont = this.until.imgStyle(item.cont);
        });
        this.info = res.data.list;
        this.cont = this.info[this.selectIndex].cont;
      });
    },
    selectThis(index) {
      this.selectIndex = index;
      this.cont = this.info[this.selectIndex].cont;
    },
    canSign() {
      let role = JSON.parse(this.until.seGet("currentRole"));
      console.log(role)
      let id = role.id;
      this.api.getBidInfo(this.infoId, id).then((res) => {
        this.bidInfo = res.data;
        console.log(this.bidInfo)
        if (this.bidInfo.applyNum > 0) this.IsSignUp = true;
        let nowDate = new Date().getTime();
        if (new Date(res.data.completeTm + " 23:59:59").getTime() > nowDate&&!this.until.seGet("currentRole")) {
          if(role.identityCd==this.bidInfo.viewRangeCd||this.bidInfo.orgEnterIds.indexOf(role.id)>-1){
            this.signFlag = true;
          } else {
            this.signFlag = false
          }
        } else {
          this.signFlag = false;
        }
      });
    },
    sign() {
      if (this.until.seGet("currentRole")) {
        this.dialogVisible = true;
        this.currentRole = JSON.parse(this.until.seGet("currentRole"));
      } else {
        MessageBox.confirm("您未登录，立即登录?", "提示", {
          confirmButtonText: "确定",
          cancelButtonText: "取消",
          type: "warning",
        }).then(() => {
          window.location.href = "./login/login";
        });
      }
    },
    confirm() {
      let data = {
        attachment: "",
        bidId: this.bidInfo.id,
        bidNm: this.bidInfo.nm,
        orgId: this.currentRole.id,
        orgNm: this.currentRole.company,
        rmks: "",
      };
      this.api.bidApply(data).then((res) => {
        this.$alert(`${this.bidInfo.nm}项目已报名成功，请进入“个人中心-我的投标”查看`, "提示", {
          confirmButtonText: "确定",
        })
        this.dialogVisible = false;
        this.canSign();
      });
    },
  },
};
</script>

<style lang="less">
#home {
  /*卡片图片*/
  .el-carousel__item:nth-child(2n) {
    /*background-color: #99a9bf;*/
  }
  .el-carousel__item:nth-child(2n + 1) {
    /*background-color: #d3dce6;*/
  }
}
</style>
<style lang="less" scoped>
@import url("../../assets/css/init.less");
#home {
  background-color: #ffffff;
  display: flex;
  flex-wrap: wrap;
}
.header {
  width: 100%;
  .adert {
    width: 100%;
    /*max-height: 350px;*/
    .bannerImg {
      height: 100%;
      background-position: center;
    }
  }
}
.content {
  padding-top: 50px;
  display: inline-flex;
  justify-content: space-between;
  .partA {
  }
  .partB {
    padding: 0 40px;
    width: 78.3%;
    display: flex;
    flex-direction: column;
    .head {
      p:nth-child(1) {
        text-align: center;
        width: 100%;
        color: #333333;
        font-size: 20px;
        line-height: 30px;
        > span {
          color: @themeColor;
          float: left;
          font-size: 14px;
          cursor: pointer;
        }
      }
      p:nth-child(2) {
        color: #999999;
        text-align: center;
        line-height: 50px;
        margin-bottom: 11px;
        border-bottom: 1px solid #e8e8e8;
        > i {
          color: @themeColor;
        }
        > span {
          margin-right: 34px;
        }
        .share {
          cursor: pointer;
        }
      }
    }
    .top-menu {
      width: 80%;
      margin: 20px auto;
      display: flex;
      align-items: center;
      .top-menu-item {
        display: flex;
        align-items: center;
        margin-right: 40px;
        cursor: pointer;
        img:first-child {
          width: 40px;
          height: 40px;
          margin-right: 10px;
        }
        div {
          display: flex;
          flex-direction: column;
          font-size: 12px;
          color: #909090;
          span:first-child {
            font-size: 16px;
          }
        }
        img:last-child {
          width: 7px;
          height: 13px;
          margin-left: 40px;
        }
      }
    }
    .body {
      line-height: 26px;
      color: #303030;
      text-align: justify;
      /*text-align-last:justify;*/
      -moz-text-align-last: justify;
      /*-webkit-text-align-last:justify;*/
      text-justify: inter-ideograph; /*ie中必须有这个*/
      /*text-align-last:justify;*/

      p {
        text-indent: 18px;
      }
    }
    .bottom-btn {
      width: 100px;
      height: 40px;
      background-color: #2778be;
      line-height: 40px;
      text-align: center;
      font-size: 18px;
      color: #fff;
      margin: 40px auto;
      cursor: pointer;
    }
    .complated-btn {
      width: 100px;
      height: 40px;
      background-color: #ddd;
      line-height: 40px;
      text-align: center;
      font-size: 18px;
      color: #fff;
      margin: 40px auto;
    }
  }
}
.footerImg {
  width: 100%;
  margin-top: 53px;
}
</style>
