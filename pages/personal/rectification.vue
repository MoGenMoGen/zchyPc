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
        ></el-date-picker>
      </div>
      <div class="slt">
        <el-input
          placeholder="整改单号、整改内容"
          v-model="content"
          clearable
        ></el-input>
      </div>
      <div class="search">
        <p @click="query">查询</p>
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
      <!-- 表头 -->
      <div class="th">
        <div>整改单号</div>
        <div>整改内容</div>
        <div>整改单位</div>
        <div>检验检测单位</div>
        <div>下发日期</div>
        <div>当前状态</div>
      </div>
      <!-- 表格 -->
      <div class="td">
        <div
          class="th border"
          style="height: 77px; background: #fff"
          v-for="(item, index) in List"
          :key="index"
          @click="toDetail(item.id)"
        >
          <div>{{ item.no }}</div>
          <div>{{ item.content }}</div>
          <div>{{ item.unit1 }}</div>
          <div>{{ item.unit2 }}</div>
          <div>{{ item.issuedate }}</div>
          <!-- 待执行 -->
          <div
            v-if="item.status == 1"
            style="color: #ff3c00; border: 1px solid #ff3c00"
          >
            待执行
          </div>
          <!-- 待结案 -->
          <div
            v-else-if="item.status == 2"
            style="color: #2778be; border: 1px solid #2778be"
          >
            待结案
          </div>
          <!-- 已完成 -->
          <div v-else-if="item.status == 3">已完成</div>
        </div>
      </div>
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
      pageSize: 5,
      total: 0,
      // 当前页
      currentPage: 1,
      List: [
        {
          no: "ZG20210330001",
          content:
            "【澳新船厂有限公司】的消防器材未按照标准规范摆放，消防通道有障碍物存放",
          unit1: "澳新船厂有限公司",
          unit2: "船级社检测机构",
          issuedate: " 2020-01-15",
          status: 1,
          id:1,
        },
        {
          no: "ZG20210330002",
          content:
            "【澳新船厂有限公司】的消防器材未按照标准规范摆放，消防通道有障碍物存放",
          unit1: "澳新船厂有限公司",
          unit2: "船级社检测机构",
          issuedate: " 2020-01-15",
          status: 2,
          id:2,

        },
        {
          no: "ZG20210330003",
          content:
            "【澳新船厂有限公司】的消防器材未按照标准规范摆放，消防通道有障碍物存放",
          unit1: "澳新船厂有限公司",
          unit2: "船级社检测机构",
          issuedate: " 2020-01-15",
          status: 3,
          id:3,

        },
      ],
    };
  },
  layout: "person",
  async mounted() {
    this.tabId = this.until.getQueryString("cdType");
    console.log(this.tabId);
  },
  computed: {
    ...mapState(["currentRole"]),
  },
  watch: {
    $route() {
      this.tabId = this.until.getQueryString("cdType");
      console.log(this.tabId);
    },
  },
  methods: {
    chooseTab(item, index) {
      if (item.id != this.tabId) {
        this.tabId = item.id;
        this.$router.push("../personal/rectification?cdType=" + item.id);
      }
    },
    toAdd() {
      this.$router.push("../personal/rectificationAdd");
    },
    toDetail(id){
      this.$router.push("./rectificationDetail?id="+id);

    },
    query() {
    },
    handleCurrentChange() {
      console.log(`当前页: ${val}`);
      this.currentPage3 = val;
      this.getList();
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
        border-bottom: 1px solid #dfdfdf;
      }
    }

    .Footer {
      margin-top: 40px;
      width: 100%;
      display: inline-flex;
      justify-content: center;
    }
  }
}
</style>
