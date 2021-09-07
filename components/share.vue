<template>
    <div class="share" >
      <div v-if="showShare" id="pop">
        <div class="pup">
          <vue-qr :text="shareUrl" :size="150" :margin="8"></vue-qr>
          <i class="el-icon-close" @click="showShare = false"></i>
        </div>
      </div>
      <p @click="showShare=true"><img src="@/assets/img/微信.png" />微信分享</p>
      <p @click="shareSian"><img src="@/assets/img/微博.png" />微博分享</p>
    </div>
</template>
<script>

  export default {
    props:[],
    data(){
      return{
        showShare:false,
        shareUrl:'',
      }
    },
    components: {
    },
    watch:{

    },

    mounted(){
      this.shareUrl = window.location.href
      if(window.location.href.indexOf('tradeInfo/infoDetail')>-1){ //新闻资讯详情
        this.shareUrl = window.location.origin+'/mob/views/tradeInfo/infoDetail.html?id='+this.until.getQueryString('id')
      }else if(window.location.href.indexOf('pilotService/infoDetail')>-1){ //试点服务详情
        this.shareUrl = window.location.origin+'/mob/views/pilot/referenceDetail.html?id='+this.until.getQueryString('id')
      }else if(window.location.href.indexOf('shipDisplay/showDetail')>-1){//船舶设计方案详情
        this.shareUrl = window.location.origin+'/mob/views/shipMan/shipDetail.html?id='+this.until.getQueryString('id')
      }else if(window.location.href.indexOf('/learning/infoDetail')>-1){//在线学习文字详情
        this.shareUrl = window.location.origin+'/mob/views/learning/referenceDetail.html?id='+this.until.getQueryString('id')
      }

    },
    methods:{
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
    }
  }
</script>

<style lang="less" scoped>
  @import url("../assets/css/init.less");
  .share{
    display: inline-block;
    p{
      display: inline-flex;
      align-items: center;
      padding-right: 25px;
      cursor: pointer;
      img{
        width: 20px;
        margin-right: 8px;
      }
    }

  }
  #pop{
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    background: rgba(0,0,0,0.1);
    display: flex;
    display: -webkit-flex;
    z-index: 99;
    .pup{
      position: relative;
      height: 170px;
      width: 170px;
      margin: auto;
      padding: 15px;
      background: #ffffff;
      border-radius: 3px;
      i{
        position: absolute;
        cursor: pointer;
        top:5px;
        right: 5px;
        color: #303030;
      }
    }
  }
</style>

