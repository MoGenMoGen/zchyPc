<template>
  <div class="main">
    <div style="padding:0 30px">
      <tobbar :title="title" :showAll="false"></tobbar>
    </div>
    <p style="border-bottom: 1px solid rgba(0,0,0,0.1);"></p>
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
        title: '整改单',
      }
    },
    layout: 'person',
    async mounted() {
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
  }
</style>
