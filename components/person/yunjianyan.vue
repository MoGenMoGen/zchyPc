<template>
  <!-- 云检验组件 -->

  <div class="container">
    <!-- 表头 -->
    <div class="th">
      <div>序号</div>
      <div>检查起止时间</div>
      <div>检查时长</div>
      <!-- <div>整改单</div> -->
      <div>检查图片</div>
      <div>检查视频</div>
      <div v-if="currentRole && currentRole.identityCd == 'identity50'">
        检验入口
      </div>
      <div>整改单</div>
      <div>备注</div>
    </div>
    <!-- 表格 -->
    <div class="td">
      <div
        class="th"
        style="height: 77px; background: #fff; border-bottom: 1px solid #dfdfdf"
        v-for="(item, index) in List"
        :key="index"
      >
        <div>{{ index + 1 }}</div>
        <div>
          <div>{{ item.startTm }}</div>
          <div>{{ item.endTm }}</div>
        </div>
        <div>{{ item.runTime }}</div>
        <!-- <div>{{ item.zgno }}</div> -->
        <!-- <div>
          <img
            style="width: 94px; height: 57px; cursor: pointer"
            :src="item.imgUrl"
            alt=""
            v-viewer
          />
        </div> -->
        <div>
          <el-carousel height="77px" v-viewer>
            <el-carousel-item
              v-for="(item1, index1) in item.imgUrl
                ? item.imgUrl.split(',')
                : []"
              :key="index1"
            >
              <img
                style="
                  width: 94px;
                  height: 57px;
                  cursor: pointer;
                  margin: 10px 0;
                "
                :src="item1"
                alt=""
              />
            </el-carousel-item>
          </el-carousel>
        </div>

        <div
          style="position: relative; cursor: pointer"
          @click="handlePlay(item.vedioUrl)"
        >
          <img
            style="width: 94px; height: 57px"
            :src="item.imgUrl.split(',')[0]"
            alt=""
          />
          <img class="play" src="~@/assets/img/learning/播放.png" alt="" />
        </div>
        <div
          style="position: relative"
          v-if="currentRole && currentRole.identityCd == 'identity50'"
        >
          <div
            style="
              background: red;
              color: #fff;
              font-size: 15px;
              margin-bottom: 15px;
              width: 77px;
              padding: 5px;
              cursor: pointer;
              position: absolute;
              left: 50%;
              top: 50%;
              transform: translate(-50%, -50%);
              border-radius: 5px;
            "
            @click="tofpxpert"
          >
            检验入口
          </div>
        </div>
        <div>
          <!-- 
            status:0
            船厂:空
            检测：新增
            status:1
            船厂：查看
            检测：查看
           -->
          <div
            style="
              margin: 0 auto;
              padding: 5px;
              width: 60px;
              font-size: 15px;
              color: #fff;
              background-color: #2778be;
              border-radius: 5px;
              cursor: pointer;
            "
            v-if="
              item.status == 0 &&
              currentRole &&
              currentRole.identityCd == 'identity50'
            "
            @click="newRectify(item.id)"
          >
            新增
          </div>
          <div
            v-else-if="item.status == 1"
            style="width: 40px; font-size: 15px; color: #2778be;cursor:pointer;"
          >
            查看
          </div>
          <div v-else></div>
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
      pic,
      pageSize: 5,
      total: 0,
      // 当前页
      currentPage: 1,
      id: "",
      List: [],
      currentRole: "",
    };
  },
  methods: {
    getList() {
      let qry = this.query.new();
      this.query.toP(qry, this.currentPage, this.pageSize);
      this.query.toO(qry, "crtTm", "desc");
      this.query.toW(qry, "docsId", this.id, "EQ");
      this.api.getCloudTestAdoptList(this.query.toEncode(qry)).then((res) => {
        this.total = res.page.total;
        this.List = [...this.List, ...res.data.list];
        this.List.forEach(item=>{
          this.$set(item,'status',0)
        })
      });
    },
    handleCurrentChange(val) {
      // console.log(`当前页: ${val}`);
      this.currentPage = val;
      this.getList();
    },
    handlePlay(url) {
      window.open(url);
    },
    tofpxpert() {
      // let uid = "03939";
      // let ps = "zchy54321";
      let uid = "";
      let ps = "";
      let server = "www.fpxpert.cn";
      // let url = encodeURI(
      //   `esfp://login?uid=${uid}&password=${ps}&server=${server}`
      // );
      let url = encodeURI("esfp://");
      window.location.replace(url);
    },
    newRectify(id){
     this.$router.push(`./rectificationAdd?inspId=${id}`)
    }
  },
  created() {},
  mounted() {
    this.id = this.until.getQueryString("id");
    this.List = [];
    this.getList();
    this.currentRole = this.until.seGet("currentRole");
    this.currentRole = JSON.parse(this.currentRole);
    console.log(2353545, this.currentRole);
  },

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
      flex: 2;
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
      flex: 3;
      text-align: center;
    }
    div:nth-of-type(7) {
      flex: 2;
      text-align: center;
    }
    div:nth-of-type(8) {
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

  .Footer {
    margin-top: 40px;
    width: 100%;
    display: inline-flex;
    justify-content: center;
  }
}
</style>

