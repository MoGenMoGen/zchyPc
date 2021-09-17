<template>
  <div class="main">
    <div style="padding:0 30px">
      <tobbar :title="title" :showAll="false"></tobbar>
    </div>
    <p style="border-bottom: 1px solid rgba(0,0,0,0.1);"></p>
    <div class="line1">
      <div class="endTm">
        <span>时间 :</span>
        <el-date-picker v-model="time" type="date" placeholder="选择日期"></el-date-picker>
      </div>
      <div class="slt">
        <el-input placeholder="整改单号、整改内容" v-model="content" clearable></el-input>
      </div>
      <div class="search">
        <p>查询</p>
        <p>新增整改单</p>
      </div>
    </div>
    <div class="line2">
      <div>
        <p v-for="(item,index) in tabList" :key="item.id" :class="{active2: tabId==item.id}"
          @click="chooseTab(item,index)">
          <span>{{item.nm}}</span>
        </p>
        <span class="btm" :style="{left:(tabId-1)*129+'px'}"></span>
      </div>
    </div>
    <div>
      <!-- 列表组件 -->
    </div>
  </div>
</template>

<script>
  import tobbar from "../../components/person/tobbar";
  import {
    mapState
  } from "vuex";
  export default {
    name: "allShip",
    components: {
      tobbar,
    },
    data() {
      return {
        time: '',
        content: '',
        tabId: 1,
        title: '整改单',
        tabList: [{
            id: 1,
            nm: '全部',
          },
          {
            id: 2,
            nm: '待执行',
          }, {
            id: 3,
            nm: '待结案',
          }, {
            id: 4,
            nm: '已完成',
          }
        ],
      }
    },
    layout: 'person',
    async mounted() {
      this.tabId = this.until.getQueryString('cdType')
      console.log(this.tabId)
    },
    computed: {
      ...mapState([
        'currentRole'
      ])
    },
    watch: {
      $route() {
        this.tabId = this.until.getQueryString('cdType')
        console.log(this.tabId)
      },
    },
    methods: {
      chooseTab(item, index) {
        if (item.id != this.tabId) {
          this.tabId = item.id
          this.$router.push('../personal/rectification?cdType=' + item.id)
        }
      }
    },
  }
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

        >span {
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

        >p {
          height: 38px;
          line-height: 38px;
          padding: 0 38px;
          font-size: 16px;
          color: #fff;
          background-color: #2778BE;
          border-radius: 5px;
          cursor: pointer;
        }
      }
    }

    .line2 {
      margin-top: 15px;
      width: 100%;

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
          border-bottom: 2px solid #2778BE;
          /*对left属性过渡5秒*/
          transition: left 0.5s;
        }
      }
    }

    .active2 {
      color: #2572B5;
    }
  }
</style>
