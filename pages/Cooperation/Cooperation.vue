<template>
<div id="home" :style="{width:bWidth + 'px'}">
  <div class="Carousel">
    <el-carousel :height="bannerHeight+'px'" trigger="click" :style="{width: bWidth + 'px'}" >
      <el-carousel-item v-for="item in adertList" :key="item.id">
        <div class="bannerImg" :style="{backgroundImage:'url('+item.imgUrl+')'}">
        </div>
      </el-carousel-item>
    </el-carousel>
  </div>
<!--  <img  alt="" src="../../assets/img/协同创新.png" :style="{width:bWidth + 'px'}"/>-->

  <div class="header main" :style="{width:width+'px'}" >
    <img :src="imgList[0]" @click="toDetail()"/>
    <img :src="imgList[1]" @click="toDetail()"/>
  </div>
</div>
</template>

<script>
  import {mapState} from "vuex";

  export default {
        name: "Cooperation",
        data(){
              return{
                imgList:['https://sinovat.oss-cn-shanghai.aliyuncs.com/e9ef4ca80cc24009b13d69d6b9340b48-%E5%9B%BE1.png',
                'https://sinovat.oss-cn-shanghai.aliyuncs.com/45e9636bce3a45b3aeebfd00cb5b3ae0-%E5%9B%BE2.png'],
                bannerHeight:400,
                adertList:[],
              }
        },
        computed: {
          ...mapState([
            'bWidth',
            'width',
            'currentRole'
          ])
        },
    watch:{

    },
    mounted() {
      this.getData()
    },
    methods:{
          toDetail(){
            this.$router.push('/Cooperation/cooperationDetail')
          },
          async getData(){
            this.adertList = await this.api.getAdert('ideaAdert')
            if(this.adertList.length>0){
              let myImg = new Image()
              myImg.src = this.adertList[0].imgUrl
              myImg.onload = ()=>{
                this.bannerHeight = myImg.height
              }
            }
          }

        }
    }
</script>

<style lang="less" scoped>
  @import url("../../assets/css/init.less");
#home{
 background-color: #FFFFFF;
  background-image: url("../../assets/img/footer.png");
  background-position: center bottom;
  background-repeat: repeat-x;
  padding-bottom: 80px;
  .Carousel{
    width: 100%;
    margin-bottom: 10px;
    .bannerImg{
      height: 100%;
      background-position: center;
      width: 100%;
      /*object-fit: cover;*/
    }
  }
}
  .footerImg{
    margin-top: 60px;
  }
</style>
