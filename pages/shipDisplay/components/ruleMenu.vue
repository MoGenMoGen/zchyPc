<template>
  <div id="app">
    <div v-for="(item,index) in list" :key="index" class="info" :class="{active:choose == item.cd}" @click="toPage(item.cd)" >
      <p>{{item.nm}}</p>
    </div>
  </div>
</template>

<script>
  import {mapState} from "vuex";

  export default {
    name: "ruleMenu",
    props:['type'],
    data(){
      return{
        list:[],
        choose:'',//当前选中
      }
    },
    watch:{
      $route(e){
        this.getData()
      },
    },
    mounted() {

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
    //   changeActive(item){
    //     //将选中项的id赋值给this.choose
    //     console.log('选中项Id',item.id)
    //     this.$router.push('./tradeInfo?cid='+ item.id)
    //   },
       async getData(){
         // let qry = this.query.new()
         // this.query.toW(qry,'pid','4942035725390848','EQ')
         // this.query.toO(qry,'seq','asc')
         // this.list = await this.api.getClassify(this.query.toEncode(qry))
         this.list = await this.api.dataDictionary('BD_RULE_TYPE')
         if(this.until.getQueryString('cid')){
           this.choose = this.until.getQueryString('cid')
         }else {
           this.choose = this.list[0].cd
           this.$emit('getList',this.choose)
         }
       },
      toPage(id){
         if(this.type==1){
           this.$router.push('/shipDisplay/designRule?cid='+id)
         }else {
           this.$router.push('/shipDisplay/buildRule?cid='+id)
         }
      }
    },
  }
</script>

<style lang="less" scoped>
  @import url("../../../assets/css/init.less");
  #app{
    border: 1px solid #F3F3F3;
    .info{
      width: 200px;
      height: 60px;
      >p{
        color: #666666;
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
