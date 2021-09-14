<template>
  <div class="list-box">
    <p v-for="(item,idnex) in 20" :key="index">· 全额支付订单，没有发票，如何保修？</p>
  </div>
</template>

<script>
  export default {
    name: "billHelp",
    props: {

    },
    data() {
      return {

      }
    },
    methods: {

    }
  }
</script>

<style scoped lang="less">
  .list-box {
    display: flex;
    flex-direction: column;
    width: 100%;
    padding: 20px 30px;
    box-sizing: border-box;
    font-size: 14px;
    color: #333;
    line-height: 28px;
    >p {
      margin: 10px 0;
      cursor: pointer;
    }
  }
</style>
