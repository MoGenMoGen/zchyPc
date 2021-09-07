<template>
<!--  模板页面-->
<div >
  合同信息
</div>
</template>

<script>
    import {mapState} from "vuex";

    export default {
      layout:'person',
        name: "shipowner",
      components: { },
      data(){
          return{

          }
      },
      computed:{
        ...mapState([
          'bWidth',
          'width'
        ])
      },
    }
</script>

<style lang="less" scoped>
  @import url("../../assets/css/init.less");

</style>
