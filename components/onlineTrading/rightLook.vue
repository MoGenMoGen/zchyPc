<template>
    <div class="gallery-thumbsRIght" ref="RightthumbImg" :style="{height:width*3.5+40+'px'}">
      <!--:style="{height:(swiperOptionThumbsRIght.height+10)*3+32+'px'}"-->
      <swiper :options="swiperOptionThumbsRIght" :style="{height: width*3.5+'px'}">
        <!--:style="{height: (swiperOptionThumbsRIght.height+10)*3+'px'}"-->
        <swiper-slide v-for="(item,index) in list" :key="index" class="thumbImg" >
          <div class="img" @click="toProDetail(item.id)">
            <img :src="item.fmImgUrl"/>
          </div>
          <p  @click="toProDetail(item.id)">{{item.nm}}</p>
          <p class="red"  @click="toProDetail(item.id)" v-if="item.goodsMinPrice!=price && item.goodsMinPrice!==item.goodsMaxPrice">￥{{item.goodsMinPrice}}-{{item.goodsMaxPrice}}</p>
          <p class="red"  @click="toProDetail(item.id)" v-if="item.goodsMinPrice!=price && item.goodsMinPrice===item.goodsMaxPrice">￥{{item.goodsMinPrice}}</p>
          <p class="red"  @click="toProDetail(item.id)" v-if="item.goodsMinPrice==price">价格面议</p>
        </swiper-slide>
        <div class="swiper-button-next" slot="button-next"><i class="el-icon-arrow-down"></i> </div>
        <div class="swiper-button-prev" slot="button-prev"> <i class="el-icon-arrow-up"></i> </div>
      </swiper>

    </div>

</template>

<script>
  import {mapState} from "vuex";
    export default {
        name: "swiper-example-thumbs-gallery",
      title: 'Thumbs gallery with Two-way control',
      props:['list','type'],
      data() {
        return {
          width:100,

          swiperOptionThumbsRIght: {
            slidesPerView :'auto',
            autoplay:5000,
            direction:'vertical',
            grabCursor:true,
            autoplayDisableOnInteraction:false,
            mousewheelControl:true,
            autoHeight:true,
            speed:500,
            // height:400,
            spaceBetween: 10,
            loop: true,
            navigation: {
              nextEl: '.swiper-button-next',
              prevEl: '.swiper-button-prev'
            }
          },
        }
      },
      computed:{
        ...mapState([
          'proSize',
          'price'
        ]),
      },
      components: {
      },


      watch:{

      },
      mounted() {
         this.width = this.$refs.RightthumbImg.offsetWidth
         this.swiperOptionThumbsRIght.height = this.width/this.proSize+40

      },
      methods:{
        //跳转产品详情
        toProDetail(id){
          // this.until.href('../Trade/productDetail?id='+id+'&type='+this.type)
          this.$router.push({
            path:'/Trade/productDetail',
            query:{
              id:id,
              type:this.type
            }
          })
        },
      }
    }
</script>
<style lang="less">
  .gallery-thumbsRIght .swiper-container{
     top:18px;
  }
  .gallery-thumbsRIght .swiper-slide{
    display: block!important;
    box-sizing: border-box;
    font-size: 12px;
    .img{
      border: 1px solid #DDDDDD;
      margin-bottom: 5px;
      width: 100%;
      box-sizing: border-box;
      img{
        width: 100%;
        display: block;
      }
    }

  }
</style>
<style scoped lang="less">
  .gallery-thumbsRIght{
    width: 100%;
    position: relative;
    /deep/.swiper-wrapper{
      margin-top: 20px;
    }
    .thumbImg{
      cursor: pointer;
      text-align: center;
      p{
        display: block;
      }
    }
    .swiper-button-prev{
      bottom: auto;
      top:0;
    }
    .swiper-button-next{
      bottom: 0;
      top: auto;
    }
    .swiper-button-prev, .swiper-button-next{
      z-index: 8;
      position: absolute;
      left: 0;
      width: 100%;
      height: 16px;
      background: #e0e0e0;
      cursor: pointer;
      margin: 0;
      display: flex;
      display: -webkit-flex;
      align-items: center;
      justify-content: center;
    }
  }
</style>
