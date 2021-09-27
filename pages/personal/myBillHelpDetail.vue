<template>
  <div class="main">
    <div style="padding:0 30px">
      <tobbar :title="title" :showAll="false"></tobbar>
    </div>
    <p style="border-bottom: 1px solid rgba(0,0,0,0.1);"></p>
    <div class="cont" v-html="info.cont"></div>
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
      tobbar
    },
    data() {
      return {
        title: '发票帮助中心详情',
        info: {}
      }
    },
    layout: 'person',
    async mounted() {
      this.id = this.until.getQueryString('id')
      this.api.getBillHelpDetail(this.id).then(res => {
        this.info = res
      })
    },
    computed: {
      ...mapState([
        'currentRole'
      ])
    },
    watch: {
    },
    methods: {
    },
  }
</script>

<style scoped lang="less">
  .main {
    width: 100%;
    .cont {
      width: 90%;
      margin: 20px auto;
    }
  }
</style>
