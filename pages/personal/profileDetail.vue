<template>
    <!--船东(代理商)-客户档案-->
    <div class="main">
      <div style="padding:0 30px">
        <tobbar :title="title" :show-all="false"></tobbar>

      </div>
      <p style="border-bottom: 1px solid rgba(0,0,0,0.1);"></p>

      <div class="line2">
        <p> 客户名称：{{info.custNm}}</p>
        <p>联系人：{{info.linkman}}</p>
        <p>联系电话：{{info.mob}}</p>
        <p>联系邮箱：{{info.email}}</p>
        <p>客户地址：{{info.addrNm}}-{{info.address}}</p>
        <p>创建时间：{{info.crtTm }}</p>
        <p>备注：{{info.rmks}}</p>
      </div>
    </div>
</template>

<script>
    import tobbar from "../../components/person/tobbar";
    import searchIc from '../../assets/img/personal/search.png'
    import deleteIc from '../../assets/img/personal/delete.png'
    import successIc from '../../assets/img/success.png'
    import addr from "../../components/addr";
    export default {
        name: "allShip",
        components:{
            tobbar,
            addr,
        },
        methods:{
          async getData(id){
            this.info= await this.api.custInfoDetail(id)
          },



        },
        mounted(){
          let id= this.$route.query.id
          this.getData(id)
          // this.info=JSON.parse(this.$route.query.info)
        },

        data(){
            return{
                searchIc,
                deleteIc,
                successIc,
                title:'档案详情',
                info:{},


            }
        },
        layout:'person',

    }
</script>

<style scoped lang="less">
  .main{
    width: 100%;

    .line2{
      padding:31px;
      p{
        margin: 5px;
        color: #333333;
        font-size: 15px;
        font-weight: 400;
      }
    }
  }




</style>
