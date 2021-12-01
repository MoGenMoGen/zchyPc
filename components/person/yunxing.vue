<template>
  <!-- 运行记录 组件 -->

  <div class="container">
    <!-- 表头 -->
    <div class="th">
      <div>序号</div>
      <div>开始时间</div>
      <div>结束时间</div>
      <div>运行时长</div>
      <div>运行状态</div>
      <div>运行图片</div>
      <div>备注信息</div>
    </div>
    <!-- 表格 -->
    <div class="td">
      <div
        class="th border"
        style="height: 77px; background: #fff"
        v-for="(item, index) in List"
        :key="index"
      >
        <div>{{ index + 1 }}</div>
        <div>{{ item.startTm }}</div>
        <div>{{ item.endTm }}</div>
        <div>{{ item.runTime }}</div>
        <div>{{ item.runningStatusNm }}</div>
        <div>
          <el-carousel height="77px"  v-viewer>
            <el-carousel-item
              v-for="(item1, index1) in item.imgUrl
                ? item.imgUrl.split(',')
                : []"
              :key="index1"
            >
              <img
                style="width: 94px; height: 57px; cursor: pointer;margin:10px 0;"
                :src="item1"
                alt=""
              />
            </el-carousel-item>
          </el-carousel>
        </div>
        <div>{{ item.rmks }}</div>
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
</template>
<script>
import pic from "@/assets/img/safe/保险服务.png";
export default {
  name: "yunjianyan",
  props: {},
  data() {
    return {
      // 每页显示条数
      pageSize: 20,
      total: 0,
      // 当前页
      currentPage: 1,
      id: "",
      pic,
      List: [],
    };
  },
  created() {
    this.id = this.until.getQueryString("id");
    this.List = [];
    this.getList();
  },
  mounted() {},

  methods: {
    getList() {
      let qry = this.query.new();
      this.query.toP(qry, this.currentPage3, this.pageSize);
      this.query.toO(qry, "crtTm", "desc");
      this.query.toW(qry, "docsId", this.id, "EQ");
      this.api.getRunAdoptList(this.query.toEncode(qry)).then((res) => {
        this.total = res.page.total;
        this.List = res.data.list;
      });
    },
    handleCurrentChange(val) {
      // console.log(`当前页: ${val}`);
      this.currentPage = val;
      this.getList();
    },
  },
  watch: {},
};
</script>
<style lang="less" scoped>
.container {
  margin: 0 36px 0 30px;
  .th {
    display: flex;
    align-items: center;
    height: 60px;
    // padding: 0 47px 0 12px;
    background: #f7f7f7;
    div:nth-of-type(1) {
      flex: 1;
      text-align: center;
    }
    div:nth-of-type(2) {
      flex: 3;
      text-align: center;
    }
    div:nth-of-type(3) {
      flex: 3;
      text-align: center;
    }
    div:nth-of-type(4) {
      flex: 2;
      text-align: center;
    }
    div:nth-of-type(5) {
      flex: 2;
      text-align: center;
    }
    div:nth-of-type(6) {
      flex: 3;
      text-align: center;
    }
    div:nth-of-type(7) {
      flex: 2;
      text-align: center;
    }
    .play {
      width: 24px;
      height: 24px;
      position: absolute;
      top: 50%;
      left: 50%;
      transform: translate(-50%, -50%);
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
</style>

