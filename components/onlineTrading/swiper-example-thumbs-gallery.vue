<template>
  <div class="thumb-example" ref="thumbImg">
    <!-- swiper1 -->
    <div class="bigImg">
      <!--<p>{{currentImg}}</p>-->
      <!--<p>{{imgList[0]}}</p>-->
      <pic-zoom
        :imgWidth="width"
        :imgHeight="width*proSize"
        :width="200"
        :height="200*proSize"
        :url="currentImg?currentImg:imgList[0]"
        type="square"
        :scale="2"
        :out-zoomer="true"
      >
        <template slot="zoomer">
          <div class="ks-imagezoom-wrap"></div>
        </template>
      </pic-zoom>
    </div>
    <!-- swiper2 Thumbs -->
    <div class="gallery-thumbs">
      <div class="ulBox main midSwiper" style="width: 100%">

        <swiper class="swiper" :options="swiperOptionThumbs">
          <!-- <div style="width: 90%;overflow: hidden;"> -->
          <swiper-slide v-for="(item,index) in imgList" :key="index">
                <img :src="item" @click="currentImg=item"/>
          </swiper-slide>


          <div class="swiper-button-prev2 swiper-button-white swiperBtn" style="left: 0;" slot="button-prev">
            <img :src="btR" alt="" style="transform: rotate(180deg);"/>
          </div>
          <div class="swiper-button-next2 swiper-button-white swiperBtn" style="right: 0;" slot="button-next">
            <img :src="btR" alt=""/>
          </div>
        </swiper>


      </div>

      <!--<swiper :options="swiperOptionThumbs" :style="{height:((width-52-(swiperOptionThumbs.slidesPerView-1)*10)/swiperOptionThumbs.slidesPerView)*proSize+'px',width:(width-52)+'px'}">-->
        <!--<swiper-slide :class="'slide-'+index+1" v-for="(item,index) in imgList" :key="index" class="thumbImg">-->
          <!--<img :src="item" @click="currentImg=item"/>-->
        <!--</swiper-slide>-->

      <!--</swiper>-->
      <!--<div class="swiper-button-next2" slot="button-next"><i class="el-icon-arrow-right"></i> </div>-->
      <!--<div class="swiper-button-prev2" slot="button-prev"> <i class="el-icon-arrow-left"></i> </div>-->
    </div>

  </div>
</template>

<script>
  import btR from '@/assets/img/personal/rightArrow.png';
  // import test from '@/components/test'
  import PicZoom from '@/components/PicZoom'
  import {mapState} from "vuex";
    export default {
        name: "swiper-example-thumbs-gallery",
      title: 'Thumbs gallery with Two-way control',
      props:['imgList'],
      data() {
        return {
          btR,
          width:100,
          currentImg:'',
          swiperOption: {
            slidesPerView: 5,
            spaceBetween: 1,
            freeMode: true,
            navigation: {
              nextEl: '.swiper-button-next',
              prevEl: '.swiper-button-prev'
            }
          },
          swiperOptionThumbs: {
            spaceBetween: 0,
            slidesPerView: 4,
            touchRatio: 0.2,
            loop: false,
            loopedSlides: 5, //looped slides should be the same
            slideToClickedSlide: true,
            navigation: {
              nextEl: '.swiper-button-next2',
              prevEl: '.swiper-button-prev2'
            }
          }
        }
      },
      computed:{
        ...mapState([
          'proSize',
        ]),
      },
      components: {
        PicZoom,
        // test
      },


      watch:{

      },
      mounted() {
          // this.width = this.$refs.thumbImg.offsetWidth - 2
        this.$nextTick(function () {
          this.width = this.$refs.thumbImg.clientWidth;
        })

      }
    }
</script>
<style lang="less">
  .gallery-thumbs .swiper-slide{
    box-sizing: border-box;
    display: flex;
    display: -webkit-flex;
    img{
      width: 100%;
      height: 100%;
      border: 1px solid #DDDDDD;
    }
  }
</style>
<style scoped lang="less">
  .ulBox{
    .swiper-slide{
      margin-left: 20px;
      width: 16% !important;
    }
    .swiperBtn{
      top:50%!important;
      transform: translateY(-50%);
    }
  }
  .thumb-example{
    width: 100%;
    .gallery-top{
      border: 1px solid #F3F3F3;
      margin-bottom: 10px;
    }
  }
  .bigImg{
    width: 100%;

    margin-bottom: 10px;
    display: flex;
    display: -webkit-flex;
    box-sizing: content-box;
  }
  .imgNewsSwiper{
    width: 100%;
    /*display: flex;*/
    /*display: -webkit-flex;*/
     /*img{*/
       /*margin: auto;*/
       /*width: auto;*/
       /*max-width: 100%;*/
       /*height: auto;*/
       /*max-height: 100%;*/
     /*}*/
  }
  .gallery-thumbs{
    width: 100%;
    position: relative;
    .thumbImg{
        cursor: pointer;
    }
    .swiper-button-prev2{
      left: 0;
      right: auto;
    }
    .swiper-button-next2{
      left: auto;
      right: 0;
    }
    .swiper-button-prev2, .swiper-button-next2{
      position: absolute;
      height: 100%;
      width: 16px;
      text-align: center;
      background: #e0e0e0!important;
      z-index: 8;
      cursor: pointer;
      top:0;
      margin: 0;
      display: flex;
      display: -webkit-flex;
      align-items: center;
    }
  }
</style>
