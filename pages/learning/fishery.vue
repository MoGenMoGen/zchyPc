<template>
<div id="home" :style="{width:bWidth + 'px'}">
<!--  <img alt="" src="@/assets/img/learning/渔业装备轮播图.png" :style="{width:bWidth + 'px'}"/>-->
  <div class="header main" :style="{width:width + 'px'}">
    <el-breadcrumb class="board"  separator-class="el-icon-arrow-right" >
      <el-breadcrumb-item :to="{path: '../learning/learning'}">在线学习</el-breadcrumb-item>
      <el-breadcrumb-item ><p class="textBlue">{{nm}}</p></el-breadcrumb-item>
    </el-breadcrumb>
    <div class="searched">
      <input v-model="search" placeholder="请输入课程名称" />
      <button @click="toSearch"><i class="el-icon-search"></i></button>
    </div>
  </div>
  <div class="body main" :style="{width:width + 'px'}" v-if="type==2">
    <div class="title">
      <p><span class="line"></span>{{nm}}</p>
<!--      <p @click="toPage('../learning/lesson?type='+type+'&cid='+cid+'&catNm='+nm)" >查看全部<img src="@/assets/img/shipDisplay/更多.png"/></p>-->
    </div>
    <div class="mid">
      <div class="part" v-for="(item,index) in list" :key="index" :style="{height:width*0.186*1 + 'px'}" @click="toPage('../learning/lessonDetail?id=' + item.id+'&type='+type)">
        <div class="ad">
          <img :alt="item.nm" :src="item.coverUrl"/>
        </div>
        <p class="child1">{{item.nm}}</p>
        <p class="child2"><span>上传日期：</span>{{item.tm}}</p>
      </div>
    </div>
  </div>
  <div class="body main" :style="{width:width + 'px'}" v-if="type==1">
    <div class="text" v-for="(item,index) in list"  @click="toPage('../learning/infoDetail?id=' + item.id)">
      <div class="cont">
        <p>{{item.nm}}</p>
      </div>
      <!--<div class="ad">-->
        <!--<img :src="item.coverUrl"-->
             <!--class="el-images"-->
             <!--:ref="'img'+index"-->
        <!--/>-->
      <!--</div>-->
      <!--<div class="cont">-->
       <!--<span></span>-->
        <!--<p>{{item.nm}}</p>-->
        <!--<p v-html="item.cont"></p>-->
        <p>{{item.updTm}}</p>
      <!--</div>-->

    </div>
  </div>
  <div class="main" :style="{width:width+'px'}">
    <el-pagination
      background
      layout="prev, pager, next"
      @current-change="handleCurrentChange"
      :current-page.sync="pageNo"
      :page-size="pageSize"
      :total="total">
    </el-pagination>
  </div>

  <!--<div class="body2 main" :style="{width:width + 'px'}" v-if="nm=='视频学习'">-->
    <!--<div class="title">-->
      <!--<p><span class="line"></span>视频</p>-->
      <!--<p  @click="toPage('../learning/lesson?type=2&cid='+cid+'&catNm='+nm)" >查看全部<img src="@/assets/img/shipDisplay/更多.png"/></p>-->
    <!--</div>-->
    <!--<div class="mid">-->
      <!--<div class="part" v-for="(item,index) in list" :key="index" :style="{height:width*0.186*1 + 'px'}" v-if="item.type == 2" @click="toDetail('../learning/lessonDetail?id='+item.id)">-->
        <!--<div class="ad">-->
          <!--<img :alt="item.nm" :src="item.coverUrl"/>-->
        <!--</div>-->
        <!--<p class="child1">{{item.nm}}</p>-->
        <!--<p class="child2"><span>上传日期：</span>{{item.tm}}</p>-->
        <!--<img class="ok" src="@/assets/img/learning/播放.png" />-->
      <!--</div>-->
    <!--</div>-->
  <!--</div>-->
  <div class="footerImg" :style="{width:bWidth+'px'}">
    <img src="../../assets/img/footer.png" alt=""/>
  </div>
</div>
</template>

<script>
  import {mapState} from "vuex";
  import img from '@/assets/img/测试.jpg';
  export default {
        name: "fishery",
      data(){
          return{
            type:'',//1纯文字 2图文
            search:'',
            list:[],
            list2:[],
            cid:'',//栏目名
            nm:'',
            pageNo:1,
            pageSize:30,
            total:0,
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
    mounted(){
      this.cid = this.until.getQueryString('cid')
      this.nm = this.until.getQueryString('nm')
      this.type = this.until.getQueryString('type')
      this.pageSize = this.type == 1 ? 20 : 30
      this.getStudyCont();
    },
    watch:{
      $route(e){
        this.cid = this.until.getQueryString('cid')
        this.nm = this.until.getQueryString('nm')
        this.type = this.until.getQueryString('type')
        this.pageSize = this.type == 1 ? 20 : 30
        console.log(this.pageSize)
        this.getStudyCont();
      },
    },
    methods:{
      handleCurrentChange(val){
        this.pageNo = val
        this.getStudyCont();
      },
      toSearch(){
        this.getStudyCont();
      },
      toPage(url){
        this.$router.push(url)
      },
      toDetail(url){
        if(this.ifLogin){
          this.$router.push(url)
        }else {
          this.$confirm('仅登录会员查看，立即登录?', '提示', {
            confirmButtonText: '确定',
            cancelButtonText: '取消',
            type: 'warning'
          }).then(() => {
            location.href = './login/login'
          }).catch(() => {

          });
        }
      },
      async getStudyCont(){
        let qry = this.query.new()
        this.query.toP(qry,this.pageNo,this.pageSize)
        // if(this.nm != '视频学习' && this.nm != '课件学习'){
        //   this.query.toW(qry,'cid',this.cid,'EQ')
        // }
        this.query.toW(qry,'cid',this.cid,'EQ')
        this.query.toW(qry,'nm',this.search,'LK')
        this.query.toO(qry,'tm','desc')
        let data = await this.api.getStudyCont(this.query.toEncode(qry))
        this.list = data.data.list
        this.total = data.page.total
        this.list.forEach(item =>{
          item.tm = item.tm ? item.tm.split(' ')[0] : ''
          item.updTm = item.updTm.split(' ')[0]
          item.cont = item.cont.replace(/<\/?[^>]*>/g, "").slice(0,100) + '...';
          item.cont = item.cont.replace(/(&nbsp;)/g, "");
          // if(this.type==1){
          //   this.objectFitImages(this.$refs['img'+index])
          // }
        })
      },
    },
    }
</script>

<style lang="less" scoped>
  @import url("../../assets/css/init.less");
  #home{
    background-color: #FFFFFF;
    .header{
      /*margin-top: 20px;*/
      height: 52px;
      display: flex;
      align-items: center;
      justify-content: space-between;
      flex: 1;
      .board{
        line-height: 35px;
        .textBlue{
          color: #2778BE;
        }
      }
      .searched{
        height: 38px;
        /*margin-right: 20px;*/
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
    .title{
      width: 100%;
      display: flex;
      justify-content: space-between;
      margin: 10px 0 0 0;
      p:nth-child(1){
        color: #303030;
        font-size: 18px;
        display: inline-flex;
        align-items: center;
        .line{
          display: block;
          width: 4px;
          height: 16px;
          background-color: #2778BE;
          margin: 0 8px 0 0;

        }
      }
      p:nth-child(2){
        color: #333333;
        font-size: 14px;
        cursor: pointer;
        >img{
          width: 14px;
          margin-left: 7px;
          align-items: center;
          vertical-align: middle;
        }
      }
    }
    .body{
      display: flex;
      flex-wrap: wrap;
      margin-bottom: 20px;
      .mid{
        width: 100%;
        display: flex;
        flex-wrap: wrap;
        .part{
          width: 18.6%;
          margin-right: 1.75%;
          background-color: #F1F2F4;
          margin-top: 30px;
          border-radius: 10px;
          cursor: pointer;
          .child1{
            font-size: 16px;
            color: #333333;
            padding-left: 11px;
            line-height: 30px;
            padding-top: 11px;
            overflow: hidden;
            text-overflow:ellipsis;
            white-space: nowrap;
          }
          .child2{
            padding-left: 11px;
            margin-top: 11px;
            font-size: 13px;
            color: #999999;
            overflow: hidden;
            text-overflow:ellipsis;
            white-space: nowrap;
          }
          .ad{
            width: 100%;
            height: 60%;
            border-radius: 10px 10px 0 0;
            >img{
              width: 100%;
              height: 100%;
              object-fit: cover;
              border-radius: 10px 10px 0 0;
            }
          }
          &:nth-of-type(5n){
            margin-right: 0;
          }
        }
      }
      .text{
        width: 100%;
        /*display: flex;*/
        /*flex-wrap: wrap;*/
        /*padding-top: 30px;*/
        /*padding-bottom: 30px;*/
        /*border-bottom: 1px solid rgba(0,0,0,0.1);*/
        display: flex;
        display: -webkit-flex;
        font-size: 18px;
        color: #333333;
        padding: 2px 0;
        height: 40px;
        line-height: 40px;
        .ad{
          width: 20%;
          /*height: 200px;*/
          margin-right: 15px;
        }
        /*>p:nth-child(1){*/
          /*font-size: 20px;*/
          /*line-height: 28px;*/
          /*color: #333333;*/
        /*}*/
        /*>p:nth-child(2){*/
          /*padding-top: 19px;*/
          /*!*flex: 1;*!*/
        /*}*/
        /*>p:nth-child(3){*/
          /*padding-top: 19px;*/
          /*font-size: 14px;*/
          /*color: #999999;*/
          /*!*padding-bottom: 20px;*!*/
        /*}*/
        .cont{
          flex: 1;
          position: relative;
          padding-left: 12px;
          height: 40px;
          line-height: 40px;
          overflow: hidden;
          >p{
            display: inline-block;
            cursor: pointer;
            &:hover{
              color: @themeColor;
              text-decoration: underline;
            }
          }
          &:before{
            position: absolute;
            left: 0;
            top: 20px;
            transform: rotateX(-50%);
            display: block;
            content: "";
            width: 3px;
            height: 3px;
            background: #333;
          }
          /*>p:nth-child(1){*/
            /*font-size: 20px;*/
            /*line-height: 28px;*/
            /*color: #333333;*/
          /*}*/
          /*>p:nth-child(2){*/
            /*padding-top: 19px;*/
            /*flex: 1;*/
          /*}*/
          /*>p:nth-child(3){*/
            /*padding-top: 19px;*/
            /*font-size: 14px;*/
            /*color: #999999;*/
            /*padding-bottom: 20px;*/
          /*}*/
        }

      }
    }
    .body2{
      display: flex;
      flex-wrap: wrap;
      margin-top: 60px;

      .mid{
        width: 100%;
        display: flex;
        flex-wrap: wrap;
        .part{
          width: 18.6%;
          margin-right: 1.75%;
          background-color: #F1F2F4;
          margin-top: 30px;
          border-radius: 10px;
          position: relative;
          cursor: pointer;

          .ok{
            position: absolute;
            max-height: 50px;
            max-width: 40px;
            top: 55px;
            left: 90px;
            opacity:0.55;
            cursor: pointer;
          }
          .child1{
            font-size: 16px;
            color: #333333;
            padding: 11px 11px 0 11px;
            line-height: 30px;
            overflow: hidden;
            text-overflow:ellipsis;
            white-space: nowrap;
          }
          .child2{
            padding-left: 11px;
            margin-top: 11px;
            font-size: 13px;
            color: #999999;
            overflow: hidden;
            text-overflow:ellipsis;
            white-space: nowrap;
          }
          .ad{
            width: 100%;
            height: 60%;
            border-radius: 10px 10px 0 0;
            >img{
              object-fit: cover;
              width: 100%;
              height:100%;
              border-radius: 10px 10px 0 0;
            }
          }
          &:nth-of-type(5n){
            margin-right: 0;
          }
        }
      }
    }
    .footerImg{
      margin-top: 40px;
    }
  }
</style>
