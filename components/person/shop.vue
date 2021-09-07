<template>
<!--  优惠券卡片-->
    <div class="cont" @click="toDetail(item.targetId)">
       <div class="logo">
         <img :src="item.shopImg"/>
       </div>
       <p>{{item.nm}}</p>
    </div>
</template>

<script>
    import  deleteIc from '../../components/img/deleteIc.png'
    import  youhuiquan from '../../components/img/youhuiquan.png'
    import  youhuiquan2 from '../../components/img/youhuiquan2.png'
    export default {
        name: "card",
        data(){
            return{
                deleteIc,
                youhuiquan,
                youhuiquan2,
                imgUrl:youhuiquan,


            }
        },
        methods:{
          toDetail(id){
            this.$router.push('/Trade/storeHome?shopId='+id)
          }
        },
        props:['item','type']
    }
</script>

<style scoped lang="less">
.cont{
  width: 100%;
  height: 100%;
  background-color: #F9F9F9;
  cursor: pointer;
  .logo{
    width: 180px;
    height: 180px;
    border-radius: 50%;
    overflow: hidden;
    margin: 20px auto;
    display: flex;
    display: -webkit-flex;
    img{
      margin: auto;
    }
  }
  p{
    padding: 0 10px;
  }

}
</style>
