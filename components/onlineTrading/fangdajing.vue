<template>
  <div class="productLefts" ref="thumbImg">
    <!-- 左侧中图 -->
    <div class="mdImg">
      <!-- 遮罩层 -->
      <div v-show="isShow" class="marks" :style="{top:top+'px',left:left+'px',width:marksSize+'px',height:marksSize+'px'}"></div>
      <!-- 遮罩层 玻璃板 superMarks-->
      <div @mouseenter="enter" @mouseleave="leave"  @mousemove="marks" class="superMarks" ></div>
      <img :src="lgImgUrl" alt=""/>
    </div>

    <!--左侧小图 -->
    <div class="smImg" >
      <div class="gallery-thumbs">
        <swiper :options="swiperOptionThumbs" ref="swiperThumbs" :style="{height:((width-52)/swiperOptionThumbs.slidesPerView-10)*proSize+'px',width:(width-52)+'px'}">
          <swiper-slide :class="'slide-'+index+1" v-for="(item,index) in imgList" :key="index" class="thumbImg">
            <img :src="item" @click="enterLi(index)"/>
          </swiper-slide>

        </swiper>
        <div class="swiper-button-next2" slot="button-next"><i class="el-icon-arrow-right"></i> </div>
        <div class="swiper-button-prev2" slot="button-prev"> <i class="el-icon-arrow-left"></i> </div>
      </div>
      <!--&lt;!&ndash;左按钮 &ndash;&gt;-->
      <!--<div @click="prev" class="button-prev">-->
        <!--<-->
<!--&lt;!&ndash;        <img src="../assets/icon/prev.png" >&ndash;&gt;-->
      <!--</div>-->
      <!--<div class="smImgUl">-->
        <!--<ul :style="{'margin-left':marginLeft+'px'}">-->
          <!--<li @click="enterLi(index)" v-for="(item,index) of imgList" v-if="imgList.length>0" :key="index">-->
            <!--<img :src="item" alt=""/>-->
          <!--</li>-->
        <!--</ul>-->
      <!--</div>-->
      <!--&lt;!&ndash; 右按钮 &ndash;&gt;-->
      <!--<div @click="next" class="button-next">-->
        <!--&gt;-->
<!--&lt;!&ndash;        <img src="../assets/icon/next.png" >&ndash;&gt;-->
      <!--</div>-->
    </div>
    <!-- 左侧大图 -->
    <div v-show="isShow" class="lgImg">
      <img :src="lgImgUrl"  alt="" :style="{top:topLgImg+'px',left:leftLgImg+'px',width:imgWidth+'px',height:imgHeight+'px'}">
    </div>
  </div>
</template>

<script>
  import {mapState} from "vuex";
  export default {
    props:['imgList'],
    data() {
      return {
        width:200,
        height:200,
        list:[],
        mdImgUrl:"",
        lgImgUrl:"",
        scale:1,
        imgWidth:100,
        imgHeight:100,
        marksSize:200,
        ulIndex:0,//向左移动几个li
        marginLeft:0,  //向左向右移动的距离
        isShow:false,   //控制遮罩层marks和大图片是否显示"
        left:0,       //marks左移位置
        top:0,         //marks下移位置
        leftLgImg:0,       //大图lgImg移动的位置
        topLgImg:0,        //大图lgImg移动的位置
        swiperOptionThumbs: {
          spaceBetween: 10,
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
    methods: {
      //鼠标进入小图时事件，显示对应的中图
      enterLi(e){
        //e是对应的下标
        //console.log(e);
        this.lgImgUrl=this.imgList[e];
      },

      //点击左右按钮事件ul的li移动,每个li宽74px，ul宽370显示5个li
      prev(){
        //向左移动-
        if(this.ulIndex>-(this.list.length-5)){
          this.ulIndex--;
          this.marginLeft=this.ulIndex*74;
          //console.log(this.ulIndex)
        }
      },
      next(){
        //向右移动++
        if(this.ulIndex<0){
          this.ulIndex++;
          this.marginLeft=this.ulIndex*74;
          //console.log(this.ulIndex)
        }
      },

      //鼠标进入和离开
      enter(){
        this.isShow=true
      },
      leave(){
        this.isShow=false
      },
      //遮罩层放大镜
      marks(e){
        console.log(e.offsetX,e.offsetY)   //鼠标移入时的位置·
        var marksWidth=this.marksSize;//marks的宽
        var marksHeight=this.marksSize;//marks的高

        this.left=e.offsetX-marksWidth/2;
        this.top=e.offsetY-marksHeight/2;
        //console.log(this.left,this.top)
        if(this.left<0){
          this.left=0;
        }else if(this.left>this.width-marksWidth){
          this.left=this.width-marksWidth;
        }
        if(this.top<0){
          this.top=0;
        }else if(this.top>this.height-marksHeight){
          this.top=this.height-marksHeight;
        }
        //console.log(this.left,this.top)

        //中图片div宽高450，大图片div宽高800
        this.leftLgImg=-this.left*this.scale;
        this.topLgImg=-this.top*this.scale;
      }
    },
    computed:{
      ...mapState([
        'proSize',
      ]),
    },
    components:{
    },
    mounted() {
      this.width = this.$refs.thumbImg.offsetWidth - 2
      console.log('width:'+this.width)
    },
    watch: {
      imgList(){
        this.lgImgUrl= this.imgList[0]
      },
      lgImgUrl(){
        if(!this.lgImgUrl){
          return
        }
        console.log('=================')
        let img = new Image()
        img.src = this.lgImgUrl
        img.onload = () =>{
          this.imgWidth =img.width
          this.scale = this.imgWidth/this.width
          this.imgHeight =img.height
          this.height = this.imgHeight/this.scale
          this.marksSize= 400/this.scale
          console.log(this.width-this.marksSize)
        }
      }
    },
  }

</script>

<style scoped lang="less">

  /* 左侧大小图样式 */
  .productLefts{
    width: 100%;
    position: relative;
  }
  /* 左侧中图 */
  .mdImg{
    position: relative;
  }
  .mdImg{
    width: 100%;
    border: 1px solid #DDDDDD;
    img{
      display: block;
    }
  }
  /*遮罩层superMarks */
  .superMarks{
    width: 100%;
    height: 100%;
    background-color:rgba(220, 220, 220, 0);
    position:absolute;
    top:0px;
    left:0px;
  }
  /* 遮罩层 */
  .marks{
    position:absolute;
    background-color:rgba(220, 220, 220, 0.5);
    /*top:0px;  //内联设置了动态的top，left
    left:0px;*/
  }
  /* 左侧小图 */
  .smImg{
    margin-top: 10px;
    position: relative;
    /deep/.gallery-thumbs .swiper-slide{
      border: 1px solid #DDDDDD;
      box-sizing: border-box;
      img{
        width: 100%;
        height: 100%;
        cursor: pointer;
      }
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
      background: rgba(0,0,0,0.2);
      cursor: pointer;
      top:0;
      margin: 0;
      display: flex;
      display: -webkit-flex;
      align-items: center;
    }
  }

  /* 左侧隐藏大图 */
  .lgImg{
    width:400px;
    height:400px;
    overflow: hidden;
    position:absolute;
    top:0px;
    left:450px;
    border:2px solid #aaa;
    background-color:#fff;
  }
  .lgImg img{
    position:absolute;
    max-width: none;
    max-height: none;
    /*top:100px;
    left:100px;*/
  }

  /* product右侧 */
  .productRight{
    width:700px;

  }
  /* 左右按钮 */
  .button-prev,.button-next{
    width:35px;
    height:35px;
    line-height: 30px;
    border:1px solid #ddd;
    border-radius:50%;
    text-align:center;
  }
  .button-prev:hover,.button-next:hover{
    background-color:#eee;
  }
  .button-prev>img,.button-next>img{
    width:20px;
    height:20px;
  }
</style>
