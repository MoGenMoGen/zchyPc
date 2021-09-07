<template>
<!--  优惠券卡片-->
    <div class="cont">
<!--      backgroundSize:'100% 100%'-->
      <div class="line1" :style = "{background:'url('+imgUrl+')'}" >

        <img class="delete1" :src="deleteIc">
        <div style="height: 50px"></div>
        <p><span>￥</span>{{item.price}}</p>
        <p>{{item.desc}}</p>
        <p>{{item.time1}}</p>
      </div>

      <p class="desc1">{{item.limit1}}</p>
      <p class="desc2">{{item.limit2}}</p>
      <p class="btn" v-if="type==1">立即使用</p>
      <p class="btn2" v-if="type==2">已使用</p>
      <p class="btn2" v-if="type==3">已过期</p>
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
        watch:{
            type(newVal){
              if(newVal==1){
                  this.imgUrl=this.youhuiquan
              }else{
                  this.imgUrl=this.youhuiquan2
              }
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

  .line1{
    position: relative;
    width:100%;
    height: 178px;
    /*background: url("../img/youhuiquan.png");*/
    background-size: 100% 100% !important;
    .bg{
      width: 100%;
      height: 100%;
      position: absolute;
      top:0;
      left: 0;
      right: 0;
      bottom: 0;
    }

    .delete1{
      position: absolute;
      width: 20px;
      top:12px;
      right: 12px;
      &:hover{
        cursor: pointer;
      }
    }
    p:nth-of-type(1){
      text-align: center;
      font-size:49px;
      font-family:Arial;
      font-weight:bold;
      color:rgba(255,255,255,1);
      vertical-align: top;


      span{
        vertical-align: top;
        font-size:23px;
        font-family:Microsoft YaHei;
        font-weight:400;
        color:rgba(255,255,255,1);
      }
    }
    p:nth-of-type(2){
      margin-top: 17px;
      text-align: center;
      font-size:16px;
      font-family:Microsoft YaHei;
      font-weight:400;
      color:rgba(254,254,254,1);
      span{
        font-size:23px;
        font-family:Microsoft YaHei;
        font-weight:400;
        color:rgba(255,255,255,1);
      }
    }
    p:nth-of-type(3){
      text-align: center;
      margin-top: 25px;
      font-size:14px;
      font-family:Microsoft YaHei;
      font-weight:400;
      color:rgba(255,255,255,1);
      opacity:0.5;
    }
  }
  .desc1{
    white-space: nowrap;
    padding:0 19px;
    margin-top: 19px;
    font-size:14px;
    font-family:Microsoft YaHei;
    font-weight:400;
    color:rgba(153,153,153,1);

  }
  .desc2{
    padding:0 19px;
    margin-top: 16px;
    font-size:14px;
    font-family:Microsoft YaHei;
    font-weight:400;
    color:rgba(153,153,153,1);

  }
  .btn{
    border:1px solid rgba(39, 120, 190, 1);
    border-radius:17px;
    width:132px;
    height:34px;
    margin: 17px auto;

    display: flex;
    align-items: center;
    justify-content: center;
    color: #2778BE;
    &:hover{
      cursor: pointer;
    }
    &:active{
      opacity: 0.5;
    }
  }
  .btn2{
    border:1px solid #BFBFBF;
    border-radius:17px;
    width:132px;
    height:34px;
    margin: 17px auto;

    display: flex;
    align-items: center;
    justify-content: center;
    color: #BFBFBF;
    &:hover{
      cursor: pointer;
    }

  }
}
</style>
