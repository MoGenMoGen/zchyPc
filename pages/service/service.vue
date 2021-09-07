<template>
  <div id="home" :style="{width:bWidth + 'px'}">
    <div class="Carousel">
      <el-carousel trigger="click" :height="bannerHeight+'px'">
        <el-carousel-item v-for="item in adertList" :key="item.id">
          <div class="bannerImg" :style="{backgroundImage:'url('+item.imgUrl+')'}" @click="toPage(item.locUrl)">
          </div>
        </el-carousel-item>
      </el-carousel>
    </div>
    <div class="header main" :style="{width:width+ 'px'}">
      <div class="navBox">
        <div class="nav" :style="{width:width+'px'}">
          <div class="navImgBox point"
               @mouseenter="changeLiTrans(index,1,1)"
               @mouseleave="changeLiTrans(index,0,1)"
               :style="{height:navImgHeight+'px'}"
               v-for="(item,index) in navList"
               @click="toPage(item.locUrl)">
            <!-- < el-image
              :class="{toScale:item.ifShow}"
              :src="item.imgUrl"
              fit="cover"></el-image> -->
              <img :src="item.imgUrl"
                   class="el-images"
                   :ref="'img'+index"
                   :class="{toScale:item.ifShow}"
              />
            <p><span>——</span> <span class="textS">{{item.nm}}</span> <span>——</span></p>
          </div>
        </div>
      </div>
    </div>
    <div class="bgFoot" :style="{width:bWidth + 'px'}">
      <div class="footer main" :style="{width:width + 'px'}">
        <p>服务商介绍</p>
        <div class="center">
          <div class="unitBox" :style="{width:width+'px'}">
            <p v-for="(item,index) in unitList" style="cursor: pointer;">
              <img :src="item.logo">
              <span>
              {{item.company}}
            </span>
            </p>
          </div>
        </div>
      </div>
    </div>
    <div class="footerImg" :style="{width:bWidth+'px'}">
      <img src="@/assets/img/footer.png" alt=""/>
    </div>
  </div>
</template>

<script>
  import im1 from '@/assets/img/service/安全服务.png';
  import im2 from '@/assets/img/service/安全服务-白.png';
  import im3 from '@/assets/img/service/玻璃钢后处理.png';
  import im4 from '@/assets/img/service/玻璃钢后处理-白.png';
  import im5 from '@/assets/img/service/人力资源管理.png';
  import im6 from '@/assets/img/service/人力资源管理-白.png';
  import im7 from '@/assets/img/service/资源共享.png';
  import im8 from '@/assets/img/service/资源共享-白.png';
  import {mapState} from "vuex";
    export default {
        name: "service",
        data(){
            return{
              adertList:[],
              bannerHeight:0,
              list:[],
              list2:[],
              navList:[],
              navImgHeight:'160',
              unitList:[],
              fText:'富文本富文本富文本富文本富文本富文本富文本富文本富文本富文本富文本富文本富文本富文本富文本富文本富文本富文本富文本富文本富文本富文本富文本富文本富文本富文本富文本富文本富文本富文本富文本富文本富文本富文本富文本富文本富文本富文本富文本富文本富文本富文本富文本富文本富文本富文本富文本富文本富文本富文本富文本富文本富文本富文本富文本富文本富文本富文本富文本富文本富文本富文本富文本富文本富文本富文本富文本富文本富文本富文本富文本富文本富文本富文本富文本富文本富文本富文本富文本富文本富文本富文本富文本富文本富文本富文本富文本富文本富文本富文本富文本富文本富文本富文本富文本富文本富文本富文本富文本富文本富文本富文本富文本富文本富文本富文本富文本富文本富文本富文本富文本富文本富文本富文本富文本富文本富文本富文本富文本富文本富文本富文本富文本富文本富文本富文本富文本富文本富文本富文本富文本富文本富文本富文本富文本富文本'
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
        width(){
          this.getHeight()
        },
      },
      async asyncData(context){
        let arr = await Promise.all([
          context.app.api.getAdert('PTFWAdert'),
          context.app.api.getAdert('PTFW'),
        ])
        return {
          arr:arr
        }
      },
      mounted(){
          this.getData()
      },
      methods:{

        async getData(){
          this.adertList = await this.api.getAdert('PTFWAdert')
          if(this.adertList.length>0){
            let myImg = new Image()
            myImg.src = this.adertList[0].imgUrl
            myImg.onload = ()=>{
              // this.bannerHeight = myImg.height/myImg.width*this.bWidth
              this.bannerHeight = myImg.height
            }
          }
          this.navList = await this.api.getAdert('PTFW')
          this.navList.forEach((item,index)=>{
            this.objectFitImages(this.$refs['img'+index])
          })
		  this.getLogo()
        },
        //鼠标滑动切换放大效果
        changeLiTrans(index,type,nm) {
          if(nm==1){
            this.navList[index].ifShow = type ? true : false
            this.$set(this.navList, index, this.navList[index])
          }
          if(nm==2 && index!=0){
            this.tabList1[index].ifTrans = type ? true : false
            this.$set(this.tabList1,index,this.tabList1[index])
          }
          if(nm==3 && index!=0){
            this.tabList2[index].ifTrans = type ? true : false
            this.$set(this.tabList2,index,this.tabList2[index])
          }
        },
        toPage(url){
          if(url && url.indexOf('http')>-1){
            window.location.href=url
          }else if(url){
            this.$router.push(url)
          }
        },
        //获取高度，达到1：1
        getHeight(){
          this.tabImgHeight=this.width*0.24;
          this.navImgHeight=this.width*0.24*9/16;
        },
        getLogo(){
            let qry = this.query.new()
            this.query.toW(qry,'identityCd','identity90','EQ')
            this.query.toP(qry,1,20)
            this.api.getRoleAll(this.query.toEncode(qry)).then(res=>{
              this.unitList = res
            })
        },
      }
    }
</script>

<style lang="less" scoped>
  @import url("../../assets/css/init.less");
  #home{
    background-color: #F5F5F5;
    .Carousel{
      width: 100%;
      .bannerImg{
        height: 100%;
        background-position: center;
      }
    }
    .header{
      /*margin-top: 45px;*/
      display: flex;
      flex-direction: row;
      justify-content: space-between;
      .navBox{
        .nav{
          display: flex;
          justify-content: space-between;
          padding:30px 0;
          margin: 0 auto;
          .navImgBox{
            flex: 1;
            position: relative;
            overflow: hidden;
            cursor: pointer;
            margin-right: 10px;
            &:last-of-type{
              margin-right: 0;
            }
            .el-image{
              width: 100%;
              height: 100%;
            }

            >p{
              width: 100%;
              background-color: rgba(0,0,0,0.35);
              position: absolute;
              padding: 8px 0;
              bottom: 0;
              color:rgba(255,255,255,1);
              text-align: center;
              .textS{
                margin: 0 20px;
                opacity: 1;
                font-size:20px;
              }
              >span{
                opacity: 0.2;
              }
            }
          }
        }
      }
    }
    .content{
      padding-top: 90px;
      p:nth-child(1){
        font-size: 30px;
        width: 100%;
        text-align: center;
        line-height: 70px;
        color: #333333;
      }
      p:nth-child(2){
        font-size: 15px;
        width: 100%;
        text-align: center;
        line-height: 30px;
        color: #666666;
      }
      p:nth-child(3){
        font-size: 15px;
        width: 100%;
        text-align: center;
        line-height: 30px;
        color: #666666;
        margin-bottom: 40px;
      }
      /*p:nth-child(4){*/
      /*  margin-top: 40px;*/
      /*}*/
    }
    .footer{
      /*margin-top: 30px;*/
      padding-top: 30px;
      p{
        width: 100%;
        text-align: center;
        font-size: 20px;
        color: #333333;
        line-height: 30px;
      }
      .center{
        width: 100%;
        display: flex;
        flex-direction: row;
        flex-wrap: wrap;
        .unitBox{
          margin: 0 auto;
          display: flex;
          flex-wrap: wrap;
          >p{
            margin-top: 30px;
            width: 18%;
            height: 110px;
            margin-right: 2.5%;
            display: flex;
            justify-content: center;
            align-items: center;
            background-color: #FFFFFF;
            border:1px solid rgba(217, 217, 217, 1);
            padding: 17px 0;
            flex-direction: column;

            >img{
              padding-top: 10px;
              // width: 100%;
              // height: 100%;
              // line-height: 100px;
            }
            >span{
              bottom: 2px;
              font-weight: 700;
              font-size: 13px;
              /*margin-top: 10px;*/
            }
          }
          p:nth-child(5n){
            margin-right: 0;
          }
        }
      }
    }
    .bgFoot{
      background-image: url("../../assets/img/shipDisplay/custBg.png");
    }
    .footerImg{
      padding-top: 40px;
      background-color: #FFFFFF;
      &:nth-of-type(5n){
        margin-right: 0;
      }
    }
  }
</style>
