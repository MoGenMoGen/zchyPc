<template>
  <div id="app">

    <div v-for="(item,index) in list" :key="index" class="info" :class="{active:choose == item.id}" @click="changeActive(item)">
      <p>{{item.nm}}</p>
    </div>
  </div>
</template>

<script>
    import {mapState} from "vuex";

    export default {
        name: "infoMenu",
      // props:["cd"],
      data(){
          return{
            list:[],
            choose:'',//当前选中
          }
      },
      watch:{
        choose(){
          if(!this.choose){
            this.$emit('changeActive',this.choose)
          }
        },
        $route(e){
          let arr = this.until.getQueryString('cid')
          this.choose = arr
          console.log('此时choose',this.choose)
        },
      },
      mounted() {
        let arr = this.until.getQueryString('cid')
        this.choose = arr
        this.getData()
      },
      computed:{
        ...mapState([
          'bWidth',
          'width',
          "currentRole"
        ])
      },
      methods:{
        //  选中项高亮
        changeActive(item){
          //将选中项的id赋值给this.choose
          //   this.choose = item.id
          console.log('选中项Id',item.id)
          this.$router.push('../tradeInfo/tradeInfo?cid='+ item.id)
        },
        async getData(){
          let qry = this.query.new()
          this.list = await this.api.infoList(this.query.toEncode(qry))
          this.list.push({
            id: 1,
            nm: "招标公告"
          })
          if(!this.choose){
            //给this.choose赋予一个初始值
            this.choose = this.list[0].id
            console.log('刚进入时的this.choose',this.choose)
            this.$emit('changeActive',this.choose)
          }
        },
      },
    }
</script>

<style lang="less" scoped>
  @import url("../../../assets/css/init.less");
  #app{
    border: 1px solid #F3F3F3;
    .info{
      width: 160px;
      height: 60px;
      >p{
        color: #333333;
        font-size: 16px;
        text-align: center;
        line-height: 60px;
      }
      border-bottom: 1px solid #F3F3F3;
      cursor: pointer;
    }
    .active{
      background-color: @themeColor;
      >p{
        color: #FFFFFF;
      }
    }
  }
</style>
