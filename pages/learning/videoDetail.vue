<template>
<div id="home" :style="{width:bWidth + 'px'}" v-show="show">
  <div class="header main">
    <el-breadcrumb class="main" separator-class="el-icon-arrow-right" :style="{width:width + 'px'}">
      <el-breadcrumb-item :to="{path: '../learning/learning'}">在线学习</el-breadcrumb-item>
      <el-breadcrumb-item :to="{path: '../learning/liveList?cid='+info.cid+'&nm='+info.cnm}" v-if="info.cnm">{{info.cnm}}</el-breadcrumb-item>
      <el-breadcrumb-item ><p class="textBlue">直播详情</p></el-breadcrumb-item>
    </el-breadcrumb>
  </div>
  <div v-if="showShare" id="pop">
    <div class="pup">
      <vue-qr :text="shareUrl" :size="150" :margin="8"></vue-qr>
      <i class="el-icon-close" @click="showShare = false"></i>
    </div>
  </div>
  <div class="content main" :style="{width:width + 'px'}">
    <div class="left">
      <img :src="info.coverImg" :style="{height:width*0.435*0.66 + 'px'}"/>
    </div>
    <div class="right">
      <p>{{info.name}}</p>
      <p v-html="info.anchorIntro"></p>

      <p>
        <span><img src="@/assets/img/learning/浏览数.png"/>浏览数：{{info.visitQty}}</span>
        <!--<span><img src="@/assets/img/learning/播放时长.png"/>播放次数：{{info.playQty}}</span>-->
        <span @click="collectGoods"><img src="@/assets/img/learning/收藏.png"/>{{info.collected?'已收藏':'收藏'}}</span>
        <span @click="openShare"><img src="@/assets/img/微信.png"/>微信分享</span><span @click="shareSian"><img src="@/assets/img/微博.png"/>微博分享</span></p>
      <button  @click="computedPlayer" v-if="info.hasPerm">立即观看</button>
      <button v-else class="gay">{{info.memLevelNm}}</button>
    </div>
  </div>
  <div class="body main" :style="{width:width + 'px'}">
    <div class="lf">
      <p>直播介绍</p>
      <p v-html="info.courseIntro"></p>
    </div>
    <div class="rt">
      <p>推荐直播</p>
      <div v-for="(v,s) in list" :key="s" @click="toPage('./learning/videoDetail?id='+v.id)">
        <img :src="v.coverImg"/>
        <p>{{v.name}}</p>
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
    export default {
        name: "lessonDetail",
      data(){
          return{
            img,
            show:false,
            list:[],
            info:{},
            currentPage:1,
            shareUrl:'',
            ifColllect: false,

            pageSize: 4,
            id:'',
            showShare:false,//微信分享
          }
      },
      computed: {
        ...mapState([
          'bWidth',
          'width',
          'ifLogin',
          'currentRole'
        ])
      },
      created(){


      },
      mounted(){
        if(!this.until.seGet('token')){
          this.show = false
          this.$alert('登录后查看，请立即登录', '提示', {
            confirmButtonText: '确定',
            callback: action => {
              window.location.href = './login/login'
            }
          });
        }else {
          this.show = true
          this.shareUrl = window.location.origin+'/mob/views/learning/detail.html?id='+this.until.getQueryString('id')
          this.id = this.until.getQueryString('id')
          this.getStudyContDetail();
        }
      },
      methods:{
          //右侧推荐课程列表
        async getStudyCont(){
            let qry = this.query.new()
            this.query.toW(qry,'cid',this.info.cid,'EQ')
            this.query.toP(qry,1,4)
            let data = await this.api.studyLive(this.query.toEncode(qry))
            this.list = data.data.list
        },
        //收藏商品
        collectGoods(){
          if(!this.ifLogin){
            this.$confirm('您未登录，立即登录?', '提示', {
              confirmButtonText: '确定',
              cancelButtonText: '取消',
              type: 'warning'
            }).then(() => {
              this.$router.push('../login/login')
            })
            return
          }
          if (this.info.collected === 1) {
            this.api.delFollow(this.id).then(()=>{
              this.info.collected = 0;
              this.$set(this.info,'collected',0)
            })
          } else {
            //收藏商品
            let param = {
              targetId: this.id,
              type: 6,
            }
            this.api.followShop(param).then(()=>{
              this.info.collected = 1;
              this.$set(this.info,'collected',1)
            })
          }
        },
        //推荐课程详情列表
        async getStudyContDetail(){
          let data = await this.api.livePlay(this.id)
          this.info = data.data
          this.getStudyCont();

          // this.info.tm = this.info.tm.split(' ')[0]
        },
        //直播观看
        async computedPlayer(){
          let url = './learning/video2?id='+this.id
          window.open(url)
        },
        toPage(url){
          window.location.href = url
        },
        openShare(){
          this.showShare = true
        },
        shareSian(title, url, pic){
          var param = {
            url: url || window.location.href,
            type: "3",
            count: "1" /** 是否显示分享数，1显示(可选)*/,
            appkey: "" /** 您申请的应用appkey,显示分享来源(可选)*/,
            title: "分享的标题" /** 分享的文字内容(可选，默认为所在页面的title)*/,
            pic: pic || "" /**分享图片的路径(可选)*/,
            ralateUid: "" /**关联用户的UID，分享微博会@该用户(可选)*/,
            rnd: new Date().valueOf()
          };
          var temp = [];
          for (var p in param) {
            temp.push(p + "=" + encodeURIComponent(param[p] || ""));
          }
          var targetUrl =
            "http://service.weibo.com/share/share.php?" + temp.join("&");
          window.open(targetUrl, "sinaweibo");
        },
      },
    }
</script>

<style lang="less" scoped>
  @import url("../../assets/css/init.less");
#home{
  background-color: #F3F3F3;
  .textBlue{
    color: #2778BE;
  }
}
  .header{
    height: 54px;
    display: flex;
    align-items: center;
  }
  .content{
    background-color: #FFFFFF;
    padding: 30px;
    display: flex;
    .left{
      width: 48%;
      >img{
        /*margin: auto;*/
        width: 100%;
        /*object-fit: cover;*/

      }
    }
    .right{
      flex: 1;
      display: flex;
      flex-direction: column;
      padding-left: 37px;
      p:nth-child(1){
        font-size: 24px;
        color: #333333;
        line-height: 64px;
      }
      p:nth-child(2){
        font-size: 16px;
        color: #666666;
        line-height: 17px;
        padding-bottom: 30px;
        border-bottom: 1px solid rgba(0,0,0,0.1);
        display: -webkit-box;
        -webkit-box-orient: vertical;
        -webkit-line-clamp: 2;
        overflow: hidden;
      }
      /*p:nth-child(3){*/
        /*font-size: 14px;*/
        /*color: #999999;*/
        /*line-height: 52px;*/
        /*border-bottom: 1px solid rgba(0,0,0,0.1);*/
      /*}*/
      p:nth-child(3){
        color: #333333;
        padding-top: 30px;
        line-height: 15px;
        padding-bottom: 75px;
        >span{
          margin-right: 20px;
          cursor: pointer;
          display: inline-flex;
          align-items: center;
          >img{
            width: 14px;
            margin-right: 10px;
          }
        }
      }
      p:last-of-type{
        flex: 1;
      }
      >button{
        width: 152px;
        height: 46px;
        color: #FFFFFF;
        background-color: #2778BE;
        border: 0;
        border-radius: 5px;
        cursor:pointer;
      }
      .gay{
        background: #e0e0e0;
        color: #666666;
      }
    }
  }
  .body{
    margin-top: 40px;
    display: flex;
    .lf{
      width: 80.8%;
      padding: 31px;
      background-color: #FFFFFF;
      p:nth-child(1){
        width: 100%;
        height: 40px;
        background-color: #F7F7F7;
        line-height: 40px;
        border: 1px solid #EEEEEE;
        color: #666666;
        text-indent: 18px;
      }
      p:nth-child(2){
        margin-top: 30px;
      }

    }
    .rt{
      flex: 1;
      p:nth-child(1){
       font-size: 16px;
        color: #666666;
        text-align: center;
        margin-bottom: 20px;
      }
      >p{
        font-size: 16px;
        color: #333333;
        text-align: center;

      }
      >div{
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
        cursor: pointer;
        >img{
          height: 184px;
          width: 184px;
          object-fit: cover;
        }
        >p{
          margin-top: 13px;
          margin-bottom: 30px;
          font-size: 16px;
          color: #333333;
        }
      }
    }
  }
  .footerImg{
    margin-top: 50px;
  }
  #pop{
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    background: rgba(0,0,0,0.1);
    z-index: 55;
    display: flex;
    display: -webkit-flex;
    .pup{
      margin: auto;
      background: #ffffff;
      border-radius: 5px;
      width: 170px;
      height: 170px;
      padding: 15px;
      position: relative;
      i{
        position: absolute;
        cursor: pointer;
        margin-left: 10px;
        top:3px;
        right: 3px;
      }
    }
  }

</style>
