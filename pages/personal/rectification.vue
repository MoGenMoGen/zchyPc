<template>
  <!-- 整改单列表页 -->
  <div class="main">
    <div style="padding: 0 30px">
      <tobbar :title="title" :showAll="false"></tobbar>
    </div>
    <p style="border-bottom: 1px solid rgba(0, 0, 0, 0.1)"></p>
    <div class="line1">
      <div class="endTm">
        <span>时间 :</span>
        <el-date-picker
          v-model="time"
          type="date"
          placeholder="选择日期"
          @change="handleTime"
        ></el-date-picker>
      </div>
      <div class="slt">
        <el-input
          placeholder="整改单号、整改内容"
          v-model="searchText"
          clearable
        ></el-input>
      </div>
      <div class="search">
        <p @click="Search">查询</p>
        <!-- 检验检测机构 -->
        <p
          @click="toAdd"
          v-if="currentRole && currentRole.identityCd == 'identity50'"
        >
          新增整改单
        </p>
      </div>
    </div>
    <div class="line2">
      <div>
        <p
          v-for="(item, index) in tabList"
          :key="item.id"
          :class="{ active2: tabId == item.id }"
          @click="chooseTab(item, index)"
        >
          <span>{{ item.nm }}</span>
        </p>
        <span class="btm" :style="{ left: (tabId - 1) * 129 + 'px' }"></span>
      </div>
    </div>
    <!-- 列表组件 -->
    <div class="container">
      <!-- 表头
      <div class="th">
        <div>整改单号</div>
        <div>整改内容</div>
        <div>整改单位</div>
        <div>检验检测单位</div>
        <div>下发日期</div>
        <div>当前状态</div>
      </div>
      <!-- 表格 -->
      <!-- <div class="td">
        <div
          class="th border"
          style="height: 77px; background: #fff"
          v-for="(item, index) in List"
          :key="index"
          @click="toDetail(item.id)"
        >
          <div>{{ item.cd }}</div>
          <div>{{ item.rectifyDemand }}</div>
          <div>{{ item.orgEnterNm }}</div>
          <div>{{ item.orgTestEnterNm }}</div>
          <div>{{ item.issueTm }}</div>
          <!-- 待执行 -->
      <!-- <div
            v-if="item.state == 2"
            style="color: #ff3c00; border: 1px solid #ff3c00"
          >
            待执行
          </div>
          <!-- 待结案 -->
      <!-- <div
            v-else-if="item.state == 3"
            style="color: #2778be; border: 1px solid #2778be"
          >
            待结案
          </div>
          <!-- 已完成 -->
      <!-- <div v-else-if="item.state == 4">已完成</div>
        </div>
      </div>  -->

      <el-table
        :data="List"
        :cell-style="{
          'text-align': 'center',
          color: '#333',
          'font-weight': '500',
        }"
        :header-cell-style="{
          color: '#606060',
          background: '#f8f8f8',
          'text-align': 'center',
        }"
        style="width: 100%"
        @row-click="toRectifydetail"
      >
        <el-table-column prop="cd" min-width="150" label="整改单号">
        </el-table-column>
        <el-table-column
          min-width="200"
          prop="rectifyDemand"
          align="center"
          label="整改内容"
          show-overflow-tooltip="true"
        >
        </el-table-column>
        <el-table-column
          min-width="150"
          prop="orgEnterNm"
          align="center"
          label="整改单位"
        >
        </el-table-column>
        <el-table-column
          min-width="150"
          prop="orgTestEnterNm"
          align="center"
          label="检验检测单位"
        >
        </el-table-column>
        <el-table-column
          min-width="110"
          prop="issueTm"
          align="center"
          label="下发日期"
        >
        </el-table-column>
        <el-table-column
          prop="status"
          fixed="right"
          min-width="110"
          align="center"
          label="当前状态"
        >
          <template slot-scope="scope">
            <div
              v-if="scope.row.state == 2"
              style="color: #ff3c00; border: 1px solid #ff3c00"
            >
              待执行
            </div>
            <div
              v-else-if="scope.row.state == 3"
              style="color: #2778be; border: 1px solid #2778be"
            >
              待结案
            </div>
            <div v-else-if="scope.row.state == 4">已完成</div>
          </template>
        </el-table-column>
      </el-table>

      <!-- 分页 -->
      <div class="Footer">
        <el-pagination
          background
          @current-change="handleCurrentChange"
          :current-page.sync="currentPage"
          :page-size="pageSize"
          layout="prev, pager, next, jumper"
          :total="total"
        >
        </el-pagination>
      </div>
    </div>
  </div>
</template>

<script>
import tobbar from "../../components/person/tobbar";
import moment from "moment";

import { mapState } from "vuex";
export default {
  name: "allShip",
  components: {
    tobbar,
  },
  data() {
    return {
      time: "",
      content: "",
      tabId: 1,
      title: "整改单",
      // 待执行等状态
      state: "",
      searchText: "",
      searchDate: "",
      tabList: [
        {
          id: 1,
          nm: "全部",
        },
        {
          id: 2,
          nm: "待执行",
        },
        {
          id: 3,
          nm: "待结案",
        },
        {
          id: 4,
          nm: "已完成",
        },
      ],
      // 每页显示条数
      pageSize: 10,
      total: 0,
      // 当前页
      currentPage: 1,
      List: [],
    };
  },
  layout: "person",
  async mounted() {
    this.tabId = this.until.getQueryString("cdType") || 1;
    if (this.tabId == 1) {
      this.state = "";
    } else {
      this.state = this.tabId;
    }
    this.currentPage = 1;
    this.List = [];
    this.getList();
  },
  computed: {
    ...mapState(["currentRole"]),
  },
  watch: {
    $route() {
      this.tabId = this.until.getQueryString("cdType");
      console.log(this.tabId);
      if (this.tabId == 1) {
        this.state = "";
      } else {
        this.state = this.tabId;
      }
      this.currentPage = 1;
      this.List = [];
      this.getList();
    },
  },
  methods: {
    handleTime(e) {
      this.time = moment(e).format("YYYY-MM-DD");
    },
    chooseTab(item, index) {
      if (item.id != this.tabId) {
        this.tabId = item.id;
        this.$router.push("../personal/rectification?cdType=" + item.id);
        // this.currentPage = 1;
        // this.List = [];
        // this.state = item.id;
        // this.getList();
      }
    },
    toAdd() {
      this.$router.push("../personal/rectificationAdd");
    },

    toDetail(id) {
      this.$router.push("./rectificationDetail?id=" + id);
    },
    toRectifydetail(row) {
      this.$router.push(`./rectificationDetail?id=${row.id}`);
    },
    Search() {
      this.currentPage = 1;
      this.List = [];
      this.getList();
    },
    handleCurrentChange(val) {
      this.currentPage = val;
      this.getList();
    },
    async getList() {
      let qry = this.query.new();
      this.query.toP(qry, this.currentPage, this.pageSize);
      this.query.toW(qry, "status", "0", "EQ");
      if (this.state !== 1) {
        this.query.toW(qry, "state", this.state, "EQ");
      }
      if (this.currentRole.identityCd == "identity30") {
        this.query.toW(qry, "orgEnterId", this.currentRole.id.toString(), "EQ");
      } else if (this.currentRole.identityCd == "identity50") {
        this.query.toW(
          qry,
          "orgTestEnterId",
          this.currentRole.id.toString(),
          "EQ"
        );
      }
      let query = this.query.toEncode(qry);
      if (!this.time) {
        this.time = "";
      }
      let data = await this.api.getrectifyList(
        query,
        this.searchText,
        this.time
      );
      this.List = data.data.list;
      this.total = data.page.total;
    },
  },
};
</script>

<style scoped lang="less">
.main {
  width: 100%;

  .line1 {
    width: calc(100% - 60px);
    margin: 20px auto;
    display: flex;
    align-items: center;

    .endTm {
      display: flex;
      align-items: center;
      height: 38px;
      box-sizing: content-box;
      margin-right: 20px;

      > span {
        margin-right: 10px;
      }
    }

    .slt {
      .el-input {
        width: 310px;
      }
    }

    .search {
      display: flex;
      align-items: center;
      flex: 1;
      margin-left: 30px;
      justify-content: space-between;

      > p {
        height: 38px;
        line-height: 38px;
        padding: 0 38px;
        font-size: 16px;
        color: #fff;
        background-color: #2778be;
        border-radius: 5px;
        cursor: pointer;
      }
    }
  }

  .line2 {
    margin-top: 15px;
    width: 100%;
    .active2 {
      color: #2572b5;
    }

    div {
      // padding: 0 30px;
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

  .container {
    margin-top: 20px;
    // margin: 0 36px 0 30px;
    .th {
      display: flex;
      align-items: center;
      height: 60px;
      padding-right: 28px;
      // padding: 0 47px 0 12px;
      background: #f7f7f7;
      div:nth-of-type(1) {
        flex: 5;
        text-align: center;
      }
      div:nth-of-type(2) {
        flex: 10;
        word-wrap: break-word;
        text-align: center;
      }
      div:nth-of-type(3) {
        flex: 5;
        text-align: center;
      }
      div:nth-of-type(4) {
        flex: 5;
        text-align: center;
      }
      div:nth-of-type(5) {
        flex: 4;
        text-align: center;
      }
      div:nth-of-type(6) {
        // border:1px solid red;
        // flex: 4;
        width: 74px;
        text-align: center;
      }
    }

    .td {
      .border {
        cursor: pointer;
        border-bottom: 1px solid #dfdfdf;
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
    .Footer {
      display: flex;
      justify-content: center;
      margin: 44px 0 20px;
    }
  }
}
</style>
