<template>
  <!--  个人资料-->
  <div class="main">
    <div style="padding:0 30px">
      <tobbar :title="title" :showAll="false"></tobbar>
    </div>
    <p style="border-bottom: 1px solid rgba(0,0,0,0.1);"></p>
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
      <bill-apply v-if="tabId==1"></bill-apply>
      <bill-qua v-if="tabId==2"></bill-qua>
      <bill-help v-if="tabId==3"></bill-help>
    </div>
  </div>
</template>

<script>
  import tobbar from "../../components/person/tobbar";
  import billApply from "../../components/person/billApply";
  import billQua from "../../components/person/billQua";
  import billHelp from "../../components/person/billHelp";
  import {
    mapState
  } from "vuex";
  export default {
    name: "allShip",
    components: {
      tobbar,
      billApply,
      billApply,
      billHelp
    },
    data() {
      return {
        tabId: 1,
        title: '我的发票',
        tabList: [{
            id: 1,
            nm: '申请发票',
          },
          {
            id: 2,
            nm: '增票资质',
          }, {
            id: 3,
            nm: '发票帮助中心',
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
      $route(){
        this.tabId = this.until.getQueryString('cdType')
        console.log(this.tabId)
      },
    },
    methods: {
      chooseTab(item, index) {
        if (item.id != this.tabId) {
          this.tabId = item.id
          this.$router.push('../personal/myBill?cdType='+item.id)
        }
      }
    },
  }
</script>

<style scoped lang="less">
  .main {
    width: 100%;

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
