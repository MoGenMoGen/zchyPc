<template>
  <!-- 云检验组件 -->

  <div class="container">
    <!-- 表头 -->
    <div class="th">
      <div>序号</div>
      <div>检查起止时间</div>
      <div>检查时长</div>
      <div>整改单</div>
      <div>检查图片</div>
      <div>检查视频</div>
      <div>备注</div>
    </div>
    <!-- 表格 -->
    <div class="td">
      <div
        class="th"
        style="height: 77px"
        v-for="(item, index) in List"
        :key="index"
      >
        <div>{{ item.no }}</div>
        <div>
          <div>{{ item.st }}</div>
          <div>{{ item.et }}</div>
        </div>
        <div>{{ item.duration }}</div>
        <div>{{ item.zgno }}</div>
        <div>
          <img style="width: 94px; height: 57px" :src="item.pic" alt="" />
        </div>
        <div style="position: relative">
          <img style="width: 94px; height: 57px" :src="item.pic" alt="" />
          <img
            class="play"
            src="~@/assets/img/learning/播放.png"
            alt=""
          />
        </div>
        <div>{{ item.remark }}</div>
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
      pageSize: 5,
      total: 0,
      // 当前页
      currentPage: 1,
      pic,
      List: [
        {
          no: 1,
          st: "2021-07-12 00:00:00",
          et: "2021-07-12 23:59:59",
          duration: "03:01:12",
          zgno: "ZC20210712060001",
          pic,
          remark: "备注1",
        },
        {
          no: 1,
          st: "2021-07-12 01:00:00",
          et: "2021-07-12 22:59:59",
          duration: "03:01:12",
          zgno: "ZC20210712060002",
          pic,
          remark: "备注2",
        },
        {
          no: 1,
          st: "2021-07-12 02:00:00",
          et: "2021-07-12 21:59:59",
          duration: "03:01:12",
          zgno: "ZC20210712060003",
          pic,
          remark: "备注3",
        },
      ],
    };
  },
  methods: {
    handleCurrentChange() {
      console.log(`当前页: ${val}`);
      this.currentPage3 = val;
      this.getList();
    },
  },
  created() {},
  mounted() {},

  methods: {},
  watch: {},
};
</script>
<style lang="less" scoped>
.container {
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
      flex: 3;
      text-align: center;
    }
    div:nth-of-type(5) {
      flex: 2;
      text-align: center;
    }
    div:nth-of-type(6) {
      flex: 2;
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
      top:50%;
      left:50%;
      transform: translate(-50%,-50%);
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

