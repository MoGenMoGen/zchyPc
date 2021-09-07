<template>
<div id="app" :style="{width:bWidth + 'px'}">
  <div class="header">
    <el-breadcrumb class="main" separator-class="el-icon-arrow-right" :style="{width:width + 'px'}">
      <el-breadcrumb-item :to="{ path: './index' }">全部结果</el-breadcrumb-item>
      <el-breadcrumb-item ><p class="textRed">{{search.keywords}}</p></el-breadcrumb-item>
    </el-breadcrumb>
  </div>
  <div class="content">

  </div>
</div>
</template>

<script>
  import {mapState} from "vuex";
  export default {
        name: "brandSearch",
      data(){
          return{
            list:[],
            search:{
              keywords:'',
              pageNo:1,
              size:10,
            },
          }
      },
    computed: {
      ...mapState([
        'bWidth',
        'width',
        'currentRole',
        'typeSize'
      ])
    },
    watch:{
      $route(e){
        let arr = this.until.getQueryString('keyWord')
        this.search.keywords = arr
        this.getData()
      },
    },
    mounted(){
      let arr = this.until.getQueryString('keyWord')
      this.search.keywords = arr
      // console.log('keyWord',this.keyWord)
      this.getData()
    },
    methods:{
      async getData(){
        let data = await this.api.brandSearch(this.search)
        this.list = data.data.result
      },
    }
  }
</script>

<style lang="less" scoped>
  @import url("../../assets/css/init.less");
  .header{
    height: 40px;
    display: flex;
    align-items: center;
    width: 100%;
    .textRed{
      color: #2778BE;
      font-weight: bold;
    }
  }
</style>
