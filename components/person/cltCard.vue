<template>
<!--  收藏卡片卡片-->
    <div class="cont" @click="toPage(item)">
<!--      backgroundSize:'100% 100%'-->
      <img :src="item.fmImgUrl || item.solutionImg" >
      <p>{{item.nm || item.solutionNm}}</p>
      <p v-if="item.origPrice"><span>￥</span>{{item.origPrice}}</p>
      <p v-else><span>￥</span>{{item.solutionMinPrice}}~{{item.solutionMaxPrice}}</p>
    </div>
</template>

<script>
    import  deleteIc from '../../components/img/deleteIc.png'

    import tes1 from '../../assets/img/personal/test1.png'
    export default {
        name: "card",
        data(){
            return{
                deleteIc,
                tes1,

            }
        },

        watch:{

        },
        props:['item','type'],
        methods:{
          toPage(item){
            if(this.type=='manage'){
              let url = '/shipDisplay/showDetail?id='+item.solutionId
              this.$router.push(url)
            }else if(this.type=='ship' || this.type=='product'){
              let url = '/Trade/productDetail?id='+item.goodsId + "&type=" + this.type
              this.$router.push(url)
            }
          }
        }
    }
</script>

<style scoped lang="less">
.cont{
  width: 100%;
  height: 100%;
  transition: all 0.5s;
  &:hover{
    cursor: pointer;
    transform: scale(1.1);
  }
  /*background-color: #F9F9F9;*/
  img{
    width: 192px;
    height: 192px;
    object-fit: cover;
    display: block;
    margin: 0 auto;


  }
  p:nth-of-type(1){
    margin: 0 auto;
    width: 192px;
    height: 35px;
    margin-top: 20px;
    font-size:14px;
    font-weight:400;
    color:#666666;
  }

  p:nth-of-type(2){

    margin: 0 auto;
    width: 192px;
    margin-top: 21px;
    font-size:24px;
    font-weight:400;
    color:#FF3E3E;
    span{
      font-size: 14px;
    }
  }

}
</style>
