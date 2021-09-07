<template>
<!--  消息详情-->
    <div class="main">
      <div style="padding:0 30px">
        <tobbar :title="title" :showAll="false"></tobbar>
      </div>
      <p style="border-bottom: 1px solid rgba(0,0,0,0.1);"></p>

      <div class="line4">
        <p>{{info.nm}}</p>
        <p>{{info.sendTm}}</p>
        <div>
          <p v-html="info.cont"></p>
          <span @click="toDetail" v-if="info.targetUrl">查看详情</span>
        </div>
      </div>



    </div>
</template>

<script>
    import tobbar from "../../components/person/tobbar";
    import searchIc from '../../assets/img/personal/search.png'
    import deleteIc from '../../assets/img/personal/delete.png'
    import successIc from '../../assets/img/success.png'
    export default {
        name: "allShip",
        components:{
            tobbar,
        },
        mounted(){
            let id=this.$route.query.id
            let that = this
             this.api.msgDetail(id).then(res=>{
               that.info=res
               that.api.msgUnRead()
             })
            },
        methods:{
          toDetail(){
             window.location.href = this.info.targetUrl
          },

        },
        data(){
            return{
                title:'消息详情',
                searchIc,
                deleteIc,
                successIc,
                value: '',
                info:{},


            }
        },
        layout:'person',

    }
</script>

<style scoped lang="less">
  @import url("../../assets/css/init.less");
  .main{
    width: 100%;


    .line4{
     span{
       text-decoration: underline;
       color: @themeColor;
     }
      margin-top: 20px;
      p:nth-of-type(1){
        text-align: center;
        color: #333333;
        font-size: 20px;
      }
      p:nth-of-type(2){
        margin-top: 10px;
        margin-bottom: 20px;
        text-align: center;
        color: #999999;
      }
      div{
        padding:0 40px;
      }

    }
  }




</style>
