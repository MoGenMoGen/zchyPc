<template>
  <!--全部船舶-->
  <div class="main">
    <ship-notice ref="notice"></ship-notice>
    <div v-if="dialogVisible" class="pop">
      <div class="pop-cont">
        <p>
          <span>提示</span>
          <img :src="deleteIc" @click="dialogVisible = false" />
        </p>

        <div class="popMain">
          <div>
            <p>联系人：</p>
            <input v-model="form.linkman" />
          </div>
          <div>
            <p>联系电话：</p>
            <input v-model="form.phone" type="tel" />
          </div>
          <div>
            <p>出售金额：</p>
            <input v-model="form.salesAmt" type="number" />
          </div>
          <div>
            <p>备注：</p>
            <textarea v-model="form.rmks"></textarea>
          </div>
        </div>
        <p>
          <span @click="dialogVisible = false">取消</span>
          <span @click="toConfirm">确认</span>
        </p>
      </div>
    </div>

    <div v-if="toastVisible" class="toast">
      <div class="toast-cont">
        <p><img :src="successIc" />提交成功</p>
      </div>
    </div>
    <div style="padding: 0 30px">
      <tobbar :title="title" :show-all="false"></tobbar>
    </div>
    <p style="border-bottom: 1px solid rgba(0, 0, 0, 0.1)"></p>
    <div class="line2">
      <div class="endTm">
        <span>完成时间 :</span>
        <el-date-picker
          v-model="finishTm"
          type="datetimerange"
          format="yyyy-MM-dd"
          range-separator="至"
          start-placeholder="开始日期"
          end-placeholder="结束日期"
        >
        </el-date-picker>
      </div>

      <div class="slt">
        <p>状态：</p>
        <el-select v-model="statusL" placeholder="请选择" clearable>
          <el-option
            v-for="item in options"
            :key="item.value"
            :label="item.label"
            :value="item.value"
          >
          </el-option>
        </el-select>
      </div>
      <div class="search">
        <p>
          <input placeholder="搜索船舶名称" v-model="search" />
        </p>

        <p @click="toSearch"><img :src="searchIc" /></p>
      </div>
    </div>
    <div class="line3">
      <p>船舶编号</p>
      <p>船舶名称</p>
      <!--<p>整船金额</p>-->
      <p>船舶状态</p>
      <p>交付时间</p>
      <p>操作</p>
    </div>
    <div class="line4">
      <div v-for="item in list" :key="item.id">
        <p>{{ item.cd }}</p>
        <p>
          {{ item.nm }}
          <img
            :src="VRImg"
            style="cursor: pointer"
            v-if="item.vrUrl"
            @click="toVR(item.id)"
          />
        </p>
        <!--<p>￥{{item.totalPrice}} /艘</p>-->
        <p>{{ item.statusNm }}</p>
        <p>{{ item.deliveryTm }}</p>
        <p>
          <span class="detail" @click="toDetail(item)">查看详情</span>
          <span
            v-if="item.statusCd == 'DOCS_STATUS.03'"
            class="sail"
            @click="showPop(item)"
            >我要出售</span
          >
          <!--船厂、设计院、监理监造、检验检测-->
          <span
            v-if="
              currentRole.identityCd == 'identity30' ||
              currentRole.identityCd == 'identity20' ||
              currentRole.identityCd == 'identity50' ||
              currentRole.identityCd == 'identity40'
            "
            class="sail"
            @click="toNotice(item)"
            >节点通知</span
          >
        </p>
      </div>
    </div>

    <div class="Footer">
      <el-pagination
        background
        @size-change="handleSizeChange"
        @current-change="handleCurrentChange"
        :current-page.sync="currentPage3"
        :page-size="pageSize"
        layout="prev, pager, next, jumper"
        :total="total"
      >
      </el-pagination>
    </div>
  </div>
</template>

<script>
import tobbar from "../../components/person/tobbar";
import searchIc from "../../assets/img/personal/search.png";
import deleteIc from "../../assets/img/personal/delete.png";
import successIc from "../../assets/img/success.png";
import shipNotice from "@/components/person/shipNotice";
import VRImg from "../../assets/img/personal/VR.png";
export default {
  name: "allShip",
  components: {
    tobbar,
    shipNotice,
  },
  data() {
    return {
      VRImg,
      searchIc,
      deleteIc,
      successIc,
      finishTm: "",
      search: "",
      dialogVisible: false,
      toastVisible: false,
      title: "全部船舶",
      total: 0,
      pageSize: 10,
      currentPage3: 1,
      currentRole: "",
      userInfo: null,
      options: [],
      value: "全部",
      statusL: "",
      list: [],
      form: {
        linkman: "",
        orgEnterId: "",
        phone: "",
        docsId: "",
        salesAmt: "",
        rmks: "",
      },
    };
  },
  layout: "person",
  computed: {},
  watch: {
    statusL() {
      this.getList();
    },
    finishTm() {
      this.getList();
    },
  },
  async mounted() {
    this.userInfo = await this.api.personInfo(); //个人信息
    this.form.phone = this.userInfo.mob;
    this.form.linkman = this.userInfo.realNm;
    this.currentRole = JSON.parse(this.until.seGet("currentRole"));
    console.log(this.userInfo);
    this.cd = this.until.getQueryString("myCd");
    this.getOpList();
    this.getList();
  },
  methods: {
    //节点通知
    toNotice(item) {
      this.$refs.notice.toNotice(item);
    },

    async getOpList() {
      this.statusList = await this.api.dataDictionary("DOCS_STATUS");
      this.statusList.forEach((item) => {
        this.options.push({ value: item.cd, label: item.nm });
      });
      console.log(this.statusList);
    },
    async getList() {
      this.list = [];
      let qry = this.query.new();

      if (this.statusL) {
        this.query.toW(qry, "statusCd", this.statusL, "LK");
      }
      if (this.finishTm) {
        let timeS = this.finishTm[0];
        let timeF = this.finishTm[1];
        let timeS1 = this.until.formatDate(timeS);
        timeS =
          timeS1.year +
          "-" +
          timeS1.month +
          "-" +
          timeS1.day +
          " " +
          timeS1.hour +
          ":" +
          timeS1.minite +
          ":" +
          timeS1.second;
        let timeF1 = this.until.formatDate(timeF);
        timeF =
          timeF1.year +
          "-" +
          timeF1.month +
          "-" +
          timeF1.day +
          " " +
          timeF1.hour +
          ":" +
          timeF1.minite +
          ":" +
          timeF1.second;
        this.query.toW(qry, "deliveryTm", timeS + "," + timeF, "BT");
      }
      this.query.toP(qry, this.currentPage3, this.pageSize);
      let param = {
        keyWord: this.search,
        orgEnterId: this.currentRole.id,
      };
      this.query.toO(qry, "cd", "asc");
      let data = await this.api.allShip(this.query.toEncode(qry), param);
      this.total = data.page.total;
      data.data.list.forEach((item) => {
        item.deliveryTm = item.deliveryTm.split(" ")[0];

        this.list.push(item);
      });
    },
    toSearch() {
      this.getList();
    },
    handleSizeChange(val) {
      console.log(`每页 ${val} 条`);
    },
    handleCurrentChange(val) {
      console.log(`当前页: ${val}`);
      this.currentPage3 = val;
      this.getList();
    },
    toDetail(item) {
      this.$router.push(
        "./archives?id=" +
          item.id +
          "&shipCd=" +
          item.cd +
          "&shipStatus=" +
          item.statusNm
      );
    },
    //弹框确认
    toConfirm() {
      this.store.commit("changeLoading", true);
      this.api.toSale(this.form).then(() => {
        this.dialogVisible = false;

        this.toastVisible = true;
        let this_ = this;
        setTimeout(() => {
          this_.toastVisible = false;
        }, 2000);
      });
    },
    //显示弹框
    showPop(item) {
      this.form.orgEnterId = this.currentRole.id;
      this.form.docsId = item.id;
      this.dialogVisible = true;
    },
    toVR(id) {
      window.open("./mframe?typeCd=3&id=" + id);
    },
  },
};
</script>
<style lang="less">
.endTm {
  //时间选择
  .el-date-editor.el-input {
    border-radius: 5px;
    border: 1px solid rgba(223, 223, 223, 1);
    width: 81.9%;
    height: 100%;
    cursor: pointer;
  }
  .el-input__inner {
    height: 100%;
    cursor: pointer;
    border: 1px solid rgba(223, 223, 223, 1);
  }
  .el-date-editor .el-range-separator {
    width: auto;
  }
}
</style>

<style scoped lang="less">
.main {
  width: 100%;
  .line2 {
    width: 100%;
    display: flex;
    align-items: center;
    height: 78px;
    padding-left: 29px;
    padding-right: 16px;
    div {
      flex: 1;
      display: flex;
      align-items: center;
    }
    .endTm {
      display: flex;
      align-items: center;
      height: 38px;
      box-sizing: content-box;
      margin-right: 10px;
      width: 32.5%;
      > span {
        white-space: nowrap;
        display: inline-block;
        height: 12px;
        font-size: 14px;
        margin-right: 20px;
        font-family: Microsoft YaHei;
        font-weight: 400;
        line-height: 12px;
        color: #333333;
      }
    }
    .slt {
      margin-right: 30px;
      p {
        margin-right: 10px;
      }
    }
    .search {
      display: flex;
      align-items: center;
      p:nth-of-type(1) {
        border-top: 1px solid #dfdfdf;
        border-bottom: 1px solid #dfdfdf;
        border-left: 1px solid #dfdfdf;
        border-top-left-radius: 5px;
        border-bottom-left-radius: 5px;
        height: 38px;
        padding: 0 12px;
        align-items: center;
        display: flex;
        input {
          border: none;
          outline: none;
          color: #666666;
        }
        ::-webkit-input-placeholder {
          /* Chrome/Opera/Safari */
          color: #999999;
        }
        ::-moz-placeholder {
          /* Firefox 19+ */
          color: #999999;
        }
        :-ms-input-placeholder {
          /* IE 10+ */
          color: #999999;
        }
        :-moz-placeholder {
          /* Firefox 18- */
          color: #999999;
        }
      }
      p:nth-of-type(2) {
        background: #2778be;
        height: 38px;
        display: flex;
        align-items: center;
        justify-content: center;
        width: 55px;
        border-top-right-radius: 5px;
        border-bottom-right-radius: 5px;
        &:hover {
          cursor: pointer;
        }
        img {
          width: 18px;
        }
      }
    }
  }
  .line3 {
    display: flex;
    align-items: center;
    height: 60px;
    padding: 0 31px;
    background: #f7f7f7;
    p {
      text-align: center;
    }
    p:nth-of-type(1) {
      flex: 1.2;
    }
    p:nth-of-type(2) {
      flex: 1.5;
      padding-right: 10px;
    }
    /*p:nth-of-type(3){*/
    /*flex:1;*/
    /*}*/
    p:nth-of-type(3) {
      flex: 0.8;
    }
    p:nth-of-type(4) {
      flex: 1;
    }
    p:nth-of-type(5) {
      flex: 1;
      text-align: center;
    }
  }
  .line4 {
    div {
      display: flex;
      align-items: center;
      border-bottom: 1px solid #dfdfdf;
      padding: 15px 31px;
      color: #333333;
      font-size: 15px;
      p {
        text-align: center;
      }
      p:nth-of-type(1) {
        // flex: 0.9;
        flex: 1.2;
      }
      p:nth-of-type(2) {
        // flex: 2;
        flex: 1.5;
        display: flex;
        justify-content: center;
        padding-right: 10px;
        display: flex;
        align-items: center;
        img {
          width: 89px;
          height: 28px;
          margin-left: 9px;
        }
      }
      /*p:nth-of-type(3){*/
      /*flex:1;*/
      /*color: #E4393C;*/
      /*}*/
      p:nth-of-type(3) {
        flex: 0.8;
      }
      p:nth-of-type(4) {
        flex: 1;
      }
      p:last-of-type {
        flex: 1;
        display: flex;
        justify-content: center;
        flex-direction: column;
        span:hover {
          cursor: pointer;
        }
        .detail {
          margin: 0 auto;
          display: block;
          color: #2778be;
        }
        .sail {
          margin: 5px auto;
          display: flex;
          color: #ffffff;
          background: #fa5050;
          width: 83px;
          height: 28px;
          align-items: center;
          justify-content: center;
          border-radius: 3px;
        }
      }
    }
  }
  .Footer {
    margin-top: 40px;
    width: 100%;
    display: inline-flex;
    justify-content: center;
  }
}

.pop {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: rgba(0, 0, 0, 0.1);
  z-index: 99;
  .pop-cont {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    width: 500px;
    height: 500px;
    background: rgba(255, 255, 255, 1);
    border-radius: 8px;
    display: flex;
    display: -webkit-flex;
    flex-direction: column;
    padding-bottom: 20px;
    .popMain {
      flex: 1;
      overflow-y: scroll;
      height: 100%;
      padding: 10px 0;
      > div {
        display: flex;
        display: -webkit-flex;
        padding: 10px 20px;
        line-height: 40px;
        align-items: center;
        p {
          width: 100px;
          text-align: right;
        }

        input {
          height: 40px;
          text-indent: 10px;
        }
        input,
        textarea {
          flex: 1;
          border: 1px solid #e0e0e0;
          border-radius: 3px;
        }
        textarea {
          line-height: 30px;
          padding: 10px;
        }
      }
    }
    > p:nth-of-type(1) {
      width: 100%;
      height: 78px;
      display: flex;
      align-items: center;
      justify-content: center;
      position: relative;
      font-size: 18px;
      font-family: Microsoft YaHei;
      font-weight: 400;
      color: rgba(51, 51, 51, 1);
      border-bottom: 1px solid #dfdfdf;
      img {
        position: absolute;
        width: 20px;
        height: 20px;
        top: 20px;
        right: 20px;
        &:hover {
          cursor: pointer;
        }
      }
    }

    > p:nth-of-type(2) {
      display: flex;
      align-items: center;
      justify-content: center;
      span {
        width: 128px;
        height: 42px;
        border: 1px solid #cecece;
        border-radius: 5px;
        display: flex;
        align-items: center;
        justify-content: center;
        font-size: 18px;
      }
      span:hover {
        cursor: pointer;
      }
      span:nth-of-type(1) {
        margin-right: 22px;
      }
      span:nth-of-type(2) {
        margin-left: 22px;
        color: white;
        background: #e4393c;
      }
    }
  }
}

.toast {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: rgba(0, 0, 0, 0.1);
  .toast-cont {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    width: 271px;
    height: 139px;
    background: rgba(255, 255, 255, 1);
    border-radius: 8px;
    display: flex;
    align-items: center;
    justify-content: center;
    p {
      font-size: 20px;
      font-family: Microsoft YaHei;
      font-weight: 400;
      display: flex;
      align-items: center;

      img {
        width: 32px;
        height: 34px;
        margin-right: 11px;
      }
    }
  }
}
</style>
