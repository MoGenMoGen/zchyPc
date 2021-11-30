<template>
  <div class="bg">
    <div class="container">
      <img :src="headCancel" class="cancel" @click="hideModal">
      <div class="header">
        {{ info.nm }}
      </div>
      <div class="cont" v-html="info.cont"></div>
    </div>
  </div>
</template>

<script>
  import headCancel from "../../assets/img/personal/delete.png";
  import {
    mapState
  } from "vuex";

  export default {
    name: "tips",
    data() {
      return {
        headCancel,
        info: {},
      }
    },
    props: {},
    computed: {
      ...mapState([
        'bWidth',
        'width',
        "currentRole"
      ])
    },
    mounted() {
      this.getData()
    },
    methods: {
      async getData() {
        this.info = await this.api.ruleDetail('5724851888591872')
      },
      hideModal() {
        this.$emit('hideTips')
      },
    }
  }
</script>

<style lang="less" scoped>
  @import url("../../assets/css/init.less");

  .bg {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: rgba(0, 0, 0, .5);
    z-index: 10;
    display: flex;
    display: -webkit-flex;
  }

  .container {
    background: #fff;
    border-radius: 8px;
    // overflow-y: auto;
    margin: auto;
    width: 886px;
    position: relative;

    .cancel {
      position: absolute;
      top: 20px;
      right: 20px;
      width: 24px;
      height: 24px;
      cursor: pointer;
    }
  }

  .header {
    width: 476px;
    height: 22px;
    background: url(../../assets/img/personal/tipsBg.png) no-repeat;
    background-size: 100% 100%;
    margin: 40px auto;
    font-size: 20px;
    color: #333333;
    display: flex;
    align-items: center;
    justify-content: center;
    // border-bottom: 1px solid #DFDFDF;
  }
  .cont {
    width: 824px;
    margin: 0 auto;
    margin-bottom: 40px;
  }
</style>
