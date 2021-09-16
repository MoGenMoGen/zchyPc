<template>
  <div class="container">
    <div class="item" v-for="(item1, index1) in imglist" :key="index1">
      <div class="title">
        {{ item1.title }}
        <div class="down" @click="hanglefold(item1)">
          <img
            :class="{ arrowTransform: !item1.isshow, arrowTransformReturn: item1.isshow }"
            src="~@/assets/img/personal/下拉.png"
            alt=""
          />
        </div>
      </div>
      <div class="picbox" v-if="item1.isshow">
        <div
          class="picitem"
          v-for="(item2, index2) in item1.piclist"
          :key="index2"
        >
          <img :src="item2.pic" alt="" />
          <p class="desc">{{ item2.desc }}</p>
          <p class="date">{{ item2.date }}</p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import pic from "@/assets/img/pilotService/玻璃钢休闲渔船.png";
export default {
  name: "historyPic",
  data() {
    return {
      pic,
      imglist: [
        {
          title: "2020年9月",
          isshow: true,
          piclist: [
            { pic, desc: "船舶历史图片", date: "2020-4-23" },
            { pic, desc: "船舶历史图片", date: "2020-4-23" },
            { pic, desc: "船舶历史图片", date: "2020-4-23" },
            { pic, desc: "船舶历史图片", date: "2020-4-23" },
            { pic, desc: "船舶历史图片", date: "2020-4-23" },
            { pic, desc: "船舶历史图片", date: "2020-4-23" },
            { pic, desc: "船舶历史图片", date: "2020-4-23" },
            { pic, desc: "船舶历史图片", date: "2020-4-23" },
            { pic, desc: "船舶历史图片", date: "2020-4-23" },
          ],
        },
        {
          title: "2020年8月",
          isshow: true,
          piclist: [
            { pic, desc: "船舶历史图片", date: "2020-4-23" },
            { pic, desc: "船舶历史图片", date: "2020-4-23" },
            { pic, desc: "船舶历史图片", date: "2020-4-23" },
            { pic, desc: "船舶历史图片", date: "2020-4-23" },
            { pic, desc: "船舶历史图片", date: "2020-4-23" },
            { pic, desc: "船舶历史图片", date: "2020-4-23" },
            { pic, desc: "船舶历史图片", date: "2020-4-23" },
            { pic, desc: "船舶历史图片", date: "2020-4-23" },
            { pic, desc: "船舶历史图片", date: "2020-4-23" },
          ],
        },
      ],
    };
  },
  methods: {
    hanglefold(item) {
      item.isshow = !item.isshow;
      this.imglist.push();
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
          height: 120px;
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
}
</style>>
