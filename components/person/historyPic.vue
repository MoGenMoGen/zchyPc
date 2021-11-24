<template>
  <div class="container">
    <div class="item" v-for="(item1, index1) in imglist" :key="index1">
      <div class="title">
        {{ item1.title }}
        <div class="down" @click="hanglefold(item1)">
          <img
            :class="{
              arrowTransform: !item1.isshow,
              arrowTransformReturn: item1.isshow,
            }"
            src="~@/assets/img/personal/下拉.png"
            alt=""
          />
        </div>
      </div>
         
      <div class="picbox" v-if="item1.isshow"  v-viewer>
        <div
          class="picitem"
          v-for="(item2, index2) in item1.piclist"
          :key="index2"
        >
          <!-- <div
            class="picitem"
            style="display: inline-block"
            v-for="(item3, index3) in item2.imgUrl
              ? item2.imgUrl.split(',')
              : []"
            :key="index3"
          > -->
            <img :src="item2.imgUrl" alt="" style="cursor: pointer" />
            <p class="desc">{{ item2.name }}</p>
            <p class="date">{{ item2.imgDate }}</p>
          <!-- </div> -->
        </div>
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
import pic from "@/assets/img/pilotService/玻璃钢休闲渔船.png";
export default {
  name: "historyPic",
  data() {
    return {
      // 每页显示条数
      pageSize: 20,
      total: 0,
      // 当前页
      currentPage: 1,
      pic,
      // 船舶id
      id: "",
      // 存所有数据放一维数组，用于过渡
      List: [],
      imglist: [],
    };
  },
  created() {
    this.id = this.until.getQueryString("id");
    this.List = [];
    this.imglist = [];
    // 获取历史图片审核通过列表
    this.getList();
  },
  methods: {
    hanglefold(item) {
      item.isshow = !item.isshow;
      this.imglist.push();
    },
    handleCurrentChange() {
      // console.log(`当前页: ${val}`);
      this.currentPage = val;
      this.getList();
    },
    getList() {
      let query = {
        r: [
          {
            n: "a1",
            t: "and",
            w: [{ k: "docsId", v: "", m: "EQ" }],
          },
        ],
        o: [{ k: "crtTm", t: "desc" }],
        p: { n: 1, s: 20 },
      };
      query.r[0].w[0].v = this.id;
      query.p.s = this.pageSize;
      query.p.n = this.currentPage;

      this.api
        .getHisPicAdoptList(encodeURIComponent(JSON.stringify(query)))
        .then((res) => {
          this.total = res.page.total;
          console.log("res", res.data.list);
          this.List = [...this.List, ...res.data.list];
          console.log("一维全部展示数组", this.List);
          // 先去重再合并
          // 去重相同年月后的数组
          let uniquList = JSON.parse(JSON.stringify(this.List));
          for (let i = 0; i < uniquList.length; i++) {
            for (let j = i + 1; j < uniquList.length; j++) {
              if (
                uniquList[i].years == uniquList[j].years &&
                uniquList[i].months == uniquList[j].months
              ) {
                //第一个等同于第二个，splice方法删除第二个
                uniquList.splice(j, 1);
                j--;
              }
            }
          }
          console.log("去重后的数组", uniquList);
          uniquList.forEach((item1) => {
            //  相同年月的数组
            let likeList = this.List.filter((item2) => {
              return item1.years == item2.years && item1.months == item2.months;
            });
            console.log("相同年月数组", likeList);
            let obj = {};
            obj.title = `${likeList[0].years}年${likeList[0].months}月`;
            obj.isshow = true;
            obj.piclist = likeList;
            this.imglist.push(obj);
          });
          //为相同年月的一组数据添加一个date属性,值为相同的年月
          this.imglist.forEach((item1) => {
            if (item1.piclist.length > 0) {
              this.$set(item1, "date", item1.piclist[0].imgDate.slice(0, 7));
            }
          });
          //排序
          this.imglist = this.imglist.sort(function (a, b) {
            let x = a.date;
            let y = b.date;
            return y < x ? -1 : x > y ? 1 : 0; //从小到大排序
          });

          console.log("二维目标数组", this.imglist);
        });
    },
  },
};
</script>

<style lang="less" scoped>
.container {
  width: 100%;
  .item {
    .title {
      height: 40px;
      background: #f7f7f7;
      border: 1px solid #eeeeee;
      line-height: 40px;
      font-size: 14px;
      font-weight: bold;
      color: #666666;
      padding-left: 28px;
      // padding-right: 22px;
      .down {
        box-sizing: content-box;
        width: 12px;
        height: 40px;
        line-height: 40px;
        padding-right: 22px;
        float: right;
        img {
          width: 12px;
          height: 7px;
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

    .picbox {
      width: 100%;
      display: flex;
      flex-wrap: wrap;
      //   justify-content: space-between;
      padding: 20px 0 40px;
      .picitem {
        margin-right: 22px;
        img {
          width: 213px;
          height: 140px;
          object-fit: cover;
        }
        .desc {
          height: 16px;
          font-size: 16px;
          font-weight: 400;
          color: #303030;
          line-height: 16px;
          margin: 10px;
        }
        .date {
          height: 11px;
          font-size: 14px;
          font-weight: 400;
          color: #909090;
          line-height: 11px;
          margin: 10px;
        }
      }
      .picitem:nth-child(4n) {
        margin-right: 0;
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
</style>>
