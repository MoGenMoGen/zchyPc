<template>
<div id="home" :style="{width:bWidth + 'px'}">
  <up-load :show="show" @close="close" @submit="submit"></up-load>
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
             v-for="(item,index) in list4"
             @click="toPage('/learning/fishery?cid='+item.id+'&nm='+item.nm+'&type='+item.showType)">
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
  <div class="content main" :style="{width:width+ 'px'}">
    <div class="top">
      <button @click="openUpload"><img src="@/assets/img/learning/上传.png" alt=""/>上传（课件或知识包）</button>
    </div>
    <div class="mid">
      <div class="nav">
        <ul>
          <li v-for="(q,w) in navList" :key="w" :class="{active:icd == w}" @click="changeLiveStatus(q,w)">{{q.nm}}</li>
        </ul>
        <div class="searched">
          <input v-model="search" placeholder="请输入课程名称" />
          <button @click="toSearch"><i class="el-icon-search"></i></button>
        </div>
      </div>
      <div class="body">
        <div class="liveList" v-for="(item,index) in liveList" :key="index">
          <div class="title">
            <p><span class="line"></span>{{item.nm}}</p>
            <span @click="toPage('/learning/liveList?cid='+item.id+'&nm='+item.nm)" v-if="!item.ifMore">查看更多</span>
          </div>
          <live-list :list="item.list"></live-list>
        </div>

      </div>
      <!--视觉上、交互上暂时没有讲师详情页-->
      <div class="foot">
        <div class="title">
          <p><span class="line"></span>金牌讲师</p>
        </div>
        <div class="section">
          <div class="sectionA" v-for="(item,index) in list3" :key="index">
            <img :style="{height:(width*0.25-40)*0.466 + 'px'}" :src="item.imgUrl"/>
            <p class="nmP">{{item.nm}}</p>
            <p>[{{item.teachNm}}]</p>
            <p v-html="item.intro"></p>
          </div>
        </div>
      </div>
    </div>
  </div>
  <div class="footerImg" :style="{width:bWidth+'px'}">
    <img src="../../assets/img/footer.png" alt=""/>
  </div>
</div>
</template>

<script>
  import {mapState} from "vuex";
  import img from '@/assets/img/测试.jpg';
  import liveList from "@/components/learning/liveList";
  import UpLoad from "./components/upLoad";
  import query from "../../assets/js/query";
  export default {
        name: "learning",
    components: {UpLoad,liveList},
    data(){
          return{
            bannerHeight:400,
            adertList:[],
            icd:0,
            search:'',
            show:false,
            navImgHeight: 160,
            list:[],
            list2:[],
            //讲师列表
            list3:[],
            list4:[],
            liveList:[],//直播列表
            navList:[{
              nm:'往期回顾',
            },{
              nm:'在线直播',
            }],
            liveStatus:3,//直播状态[r](1:未开始, 2:直播中, 3:已结束, 4:暂停中, 5:已断开)
            currentPage:1,
            pageSize: 20,
            currentPage2:1,
            pageSize2: 15,
          }
      },
      computed: {
        ...mapState([
          'bWidth',
          'width',
          'currentRole',
          'ifLogin'
        ])
      },
    watch:{

    },
    mounted() {
      this.getData();
      this.getTeacher();
      this.getList();
      // this.getStudyLive();
      this.liveClassify()
    },
    methods:{
      async getData(){
        this.adertList = await this.api.getAdert('learnAdert')
        if(this.adertList.length>0){
          let myImg = new Image()
          myImg.src = this.adertList[0].imgUrl
          myImg.onload = ()=>{
            // this.bannerHeight = myImg.height/myImg.width*this.bWidth
            this.bannerHeight = myImg.height
          }
        }
        this.list = await this.api.getAdert('ZXXX')
        this.list.forEach((item,index)=>{
          this.objectFitImages(this.$refs['img'+index])
        })
      },
      //获取在线学习栏目列表
      async getList(){
        let qry = this.query.new()
        let data = await this.api.studyContCat(this.query.toEncode(qry))
        this.list4 = data.data.list
      },
      //获取直播列表（分页）
      // async getStudyLive(){
      //   let qry = this.query.new()
      //   this.query.toW(qry,'name',this.search,'LK')
      //   if(this.liveStatus != 3){
      //     this.query.toW(qry,'liveStatus',3,'NE')
      //   }else{
      //     this.query.toW(qry,'liveStatus',3,'EQ')
      //   }
      //   this.query.toP(qry,this.currentPage2,this.pageSize2)
      //   let data = await this.api.studyLive(this.query.toEncode(qry))
      //   this.liveList = data.data.list
      // },
      //讲师库-列表
      async getTeacher(){
        let qry = this.query.new()
        this.query.toP(qry,this.currentPage,this.pageSize)
        let data = await this.api.getTeacherPage(this.query.toEncode(qry))
        this.list3 = data.data.list
        this.list3.forEach(item=>{
          item.intro = item.intro.replace(/<\/?[^>]*>/g, "");
        })
        console.log('讲师')
        console.log(this.list3)
      },

      //直播分类及列表
      async liveClassify(){
         this.liveList = await this.api.studyCat()
         for(let i=0;i<this.liveList.length;i++){
           let qry = this.query.new()
           this.query.toW(qry,'cid',this.liveList[i].id,'EQ')
           this.query.toW(qry,'name',this.search,'LK')
           if(this.liveStatus != 3){
             this.query.toW(qry,'liveStatus',3,'NE')
           }else{
             this.query.toW(qry,'liveStatus',3,'EQ')
           }
           this.query.toP(query,1,10)
           let data = await this.api.studyLive(this.query.toEncode(qry))
           this.liveList[i].list = data.data.list
           this.liveList[i].ifMore = data.page.total>10 ? true : false
           this.$set(this.liveList,i,this.liveList[i])
         }
      },
      toPage(url){
        if(url){
          this.$router.push(url)
        }
      },
      toSearch(){
        this.liveClassify()
      },
      openUpload(){
        this.show = true
      },
      close() {
        // 取消弹窗回调
        this.show = false
      },
      submit() {
        // 确认弹窗回调
        this.show = false
      },
      changeLiveStatus(q,index){
        this.icd = index
        if(q.nm == '往期回顾'){
          this.liveStatus = 3
        }else{
          this.liveStatus = 1
        }
        this.liveClassify()
      },
      //鼠标滑动切换放大效果
      changeLiTrans(index,type,nm) {
        if(nm==1){
          this.list4[index].ifShow = type ? true : false
          this.$set(this.list4, index, this.list4[index])
        }

      },
    }
    }
</script>

<style lang="less" scoped>
  @import url("../../assets/css/init.less");
#home{
  background-color: #FFFFFF;
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
      /*background-color: #F5F5F5;*/
      .nav{
        display: flex;
        justify-content: space-between;
        padding:30px 0;
        margin: 0 auto;
        .navImgBox{
          width: 19%;
          position: relative;
          overflow: hidden;
          cursor: pointer;
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
    padding-top: 15px;
    .top{
      width: 100%;
      display: flex;
      justify-content: center;
      >button{
        width: 18%;
        height: 40px;
        background-color: #2778BE;
        color: #FFFFFF;
        border: none;
        border-radius: 5px;
        font-size: 14px;
        cursor: pointer;
        >img{
          width: 17px;
          vertical-align: bottom;
          margin-right: 10px;
        }
      }
    }
    .mid{
      padding-top: 55px;
      .nav{
        border-bottom: 1px solid #F3F3F3;
        display: flex;
        justify-content: space-between;
        width: 100%;
        >ul{
          width: 21.6%;
          display: flex;
          >li{
            display: flex;
            width: 50%;
            height: 40px;
            justify-content: center;
            align-items: center;
            cursor: pointer;
          }
          .active{
            background-color: #2778BE;
            color: #FFFFFF;
            border: 0;
          }
        }
        .searched{
          height: 38px;
          line-height: 38px;
          border-right: 0;
          border-radius: 5px;
          display: flex;
          display: -webkit-flex;
          align-items: center;
          >input{
            flex: 1;
            height: 100%;
            text-indent: 15px;
            border: 1px solid #DFDFDF;
            border-right: 0;
            box-sizing: border-box;
            border-radius: 5px 0 0 5px;
            width: 370px;
            color: #999999;
          }
          button{
            background: @themeColor;
            border: 1px solid @themeColor;
            color: #ffffff;
            height: 100%;
            padding: 0 20px;
            border-radius: 0 5px 5px 0;
            cursor: pointer;
            box-sizing: border-box;
          }
        }
      }
      .body{
        width: 100%;

        .liveList{
          width: 100%;
          overflow: hidden;
        }

      }
    }
    .title{
      display: flex;
      line-height: 76px;
      p{
        font-size: 18px;
        color: #333333;
        display: flex;
        align-items: center;
        flex: 1;
        .line{
          display: block;
          width: 4px;
          height: 16px;
          background-color: #2778BE;
          margin: 0 8px 0 0;
        }
      }
      >span{
        cursor: pointer;
      }
    }
    .foot{

      .section{
        display: flex;
        flex-wrap: wrap;
        width: 100%;
        .sectionA{
          width: 25%;
          /*justify-content: center;*/
          align-items: center;
          display: flex;
          flex-direction: column;
          padding: 0 20px;
          box-sizing: border-box;
          >img{
            width: 46.6%;
            border-radius: 50%;
            margin-top: 20px;
            border: 3px solid #61B5FF;
            box-shadow: 0px 0px 0 8px #D7ECFF;
          }
          .nmP{
            margin-top: 19px;
            font-size: 20px;
            color: #333333;
            font-weight: 600;
          }
          >p{
            margin-top: 10px;
            font-size: 14px;
            color: #333333;
          }
        }
      }
    }
  }
  .footerImg{
    margin-top: 40px;
  }
}

</style>
