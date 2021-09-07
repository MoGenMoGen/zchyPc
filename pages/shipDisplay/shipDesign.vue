<template>
  <!-- 船舶设计 -->
  <div id="home" :style="{width:bWidth + 'px'}">
    <design-appo :show="show" @close="close" @submit="submit"></design-appo>
    <img alt="" src="../../assets/img/shipDisplay/lunbot.png" :style="{width:bWidth + 'px'}"/>
    <div class="header main" :style="{width:width + 'px'}">
      <el-breadcrumb separator-class="el-icon-arrow-right" :style="{width:width+'px'}" class="bc main">
        <el-breadcrumb-item :to="{ path: './shipMan' }">船舶管理</el-breadcrumb-item>
        <el-breadcrumb-item ><p class="textRed">船舶设计</p></el-breadcrumb-item>
      </el-breadcrumb>
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
              style="width: 100%; height: 100%"
              :src="item.imgUrl"
              fit="cover"></el-image> -->
              <img :src="item.imgUrl"
                   class="el-images"
                   style="width: 100%; height: 100%"
                   :ref="'img'+index"
                   :class="{toScale:item.ifShow}"
              />
            <p><span>——</span> <span class="textS">{{item.nm}}</span> <span>——</span></p>
          </div>
        </div>
      </div>
<!--      <div class="title">-->
<!--        <p><span class="line"></span>船舶设计</p>-->
<!--      </div>-->
<!--      <div class="content">-->
<!--        <div class="left"  v-for="item in adertList" :style="{height:width*0.66*0.32*2+20+'px'}">-->
<!--          <p>{{item.nm}}<span>{{item.nm}}</span></p>-->
<!--          < el-image-->
<!--            :src="item.imgUrl"-->
<!--            fit="cover"></el-image>-->
<!--        </div>-->
<!--        <div class="right">-->
<!--          <div class="list" v-for="(item,index) in list" :key="index" :style="{height:width*0.66*0.32+'px'}" :class="{active:item.ifTrans}" @mouseenter="changeLiTrans(index,1,1)" @mouseleave="changeLiTrans(index,0,1)"  @click="toPage(item)">-->
<!--            <div v-show="item.ifTrans"  class="transContent toScale" :style="{width:width*0.66*0.32+8+'px',background:item.color}">-->
<!--              <p><img :src="item.img2"/>-->
<!--                {{item.nm}}</p>-->
<!--              {{item.cont}}-->
<!--            </div>-->
<!--            <div class="showContent">-->
<!--              <img :src="item.img"/>-->
<!--              <p>{{item.nm}}</p>-->
<!--            </div>-->
<!--          </div>-->
<!--        </div>-->
<!--      </div>-->
    </div>
    <div class="content1 main" :style="{width:width + 'px'}">
      <div class="title">
        <p>
          <img :src="left"/>
          设计流程
          <img :src="right"/>
        </p>
      </div>
      <div class="cont">
        <img src="https://sinovat.oss-cn-shanghai.aliyuncs.com/9ae4946e74794576a431e7a87962a65e-步骤.png" />
      </div>
    </div>
    <div class="body main" :style="{width:width+'px'}">
      <recomm></recomm>
    </div>
<!--    <div class="body2 main" :style="{width:width+'px'}">-->
<!--      <div class="title">-->
<!--        <p>中创海洋合作设计院<img src="@/assets/img/shipDisplay/更多.png" alt=""/></p>-->
<!--      </div>-->
<!--      <div class="sfq main"  :style="{width:width+'px'}">-->
<!--        <div v-for="(item,index) in roleAllList" :key="index"-->
<!--             :style="{backgroundImage:`url('${item.imgUrl}')`}"-->
<!--             :class="{hiddenItem:!item.ifShow,showItem:item.ifShow}"-->
<!--             @mouseenter="changeLiTrans(index,1,2)"-->
<!--             @mouseleave="changeLiTrans(index,0,2)">-->
<!--          <div class="showContent"  v-show="item.ifShow">-->
<!--            <p>{{item.company}}</p>-->
<!--            <p class="cont">{{item.business}}</p>-->
<!--            <span  @click="toDetail(`/shipDisplay/design?id=${item.id}`)">了解详情 ></span>-->
<!--          </div>-->
<!--          <div class="hiddenContent" v-show="!item.ifShow">-->
<!--            <p>{{item.company}}</p>-->
<!--          </div>-->
<!--        </div>-->
<!--      </div>-->
<!--    </div>-->
    <div class="footerImg" >
      <img src="../../assets/img/footer.png" alt=""/>
    </div>
  </div>
</template>

<script>
  import {mapState} from "vuex";
  import DesignAppo from "@/components/designAppo";
  import recomm from "@/components/recomm";
  import left from "../settleIn/component/img/left.png";
  import right from "../settleIn/component/img/right.png";
  export default {
    name: "shipDesign",
    components: {DesignAppo,recomm},
    data(){
      return{
        show:false,
        left,
        right,
        navList:[],
        adertList:'',
        roleAllList:[],
        navImgHeight: 160,
      }
    },
    computed: {
      ...mapState([
        'bWidth',
        'width',
        'currentRole',
        'typeSize',
        'ifLogin'
      ])
    },
    created(){
    },
    async asyncData(context){
      if(process.client){
        return
      }
      let qry = context.app.query.new()
      context.app.query.toW(qry,'pid','4942035725390848','EQ')
      let classify =await context.app.api.getClassify(context.app.query.toEncode(qry))
      for(let i=0;i<classify.length;i++){
        let qry2 = context.app.query.new()
        context.app.query.toW(qry2,'pid',classify[i].id,'EQ')
        classify[i].list =await context.app.api.getClassify(context.app.query.toEncode(qry2))
      }
      return{
        classify:classify,
      }
    },
    mounted() {
      this.getShipList()
      // this.getRoleList()
      this.getAdert()
    },
    methods:{
      async getAdert(){
        this.adertList=await this.api.getAdert('shipDesign',1)
        this.navList = await this.api.getAdert('CBSJ')
        this.navList.forEach((item,index)=>{
          this.objectFitImages(this.$refs['img'+index])
        })
      },
      async getShipList(){
        let qry = this.query.new()
        this.query.toW(qry,'pid','4942035725390848','EQ')
        this.query.toO(qry,'seq','asc')
        this.list2=await this.api.getClassify(this.query.toEncode(qry))
        console.log('船舶类型')
        console.log(this.list2)
        this.idNow=this.list2[0].id
        this.imgUrlNow=this.list2[0].imgUrl2
        this.changeActive(this.list2[0])
      },
      // async getRoleList(){
      //   let qry = this.query.new()
      //   this.query.toW(qry,'identityCd','identity20','EQ')
      //   this.query.toP(qry,1,4)
      //   this.roleAllList = await this.api.getRoleAll(this.query.toEncode(qry))
      //   //目前只有一家设计院，可取消
      //   // this.roleAllList.splice(0,1)
      //   this.roleAllList.splice(1)
      //   this.roleAllList[0].imgUrl= this.roleAllList[0].imgUrl.split(',')[0]
      // },
      close() {
        // 取消弹窗回调
        this.show = false
      },
      submit() {
        // 确认弹窗回调
        this.show = false
      },
      //设计院详情
      // toDetail(url){
      //   this.$router.push(url)
      // },
      toPage(url){
        if(!url){
          this.show = true
        }
        if(url.indexOf('shipDIY')>-1 && !this.ifLogin){
          this.$confirm('您未登录，立即登录?', '提示', {
            confirmButtonText: '确定',
            cancelButtonText: '取消',
            type: 'warning'
          }).then(() => {
            this.$router.push('../login/login')
          })
          return
        }
        // if(url.indexOf('shipDIY')>-1){
        //   this.$router.push(url + '?tad=1')
        // }
        this.$router.push(url)
      },
      // toPage(item){
      //   if(!item){
      //     this.show = true
      //   }else{
      //     if(!this.ifLogin && item.href=='./shipDIY'){
      //       this.$confirm('您未登录，立即登录?', '提示', {
      //         confirmButtonText: '确定',
      //         cancelButtonText: '取消',
      //         type: 'warning'
      //       }).then(() => {
      //         this.$router.push('../login/login')
      //       })
      //       return
      //     }
      //     this.$router.push(item)
      //   }
      // },
      async changeActive(item){
        console.log('id:'+item.id)
        this.idNow=item.id
        this.imgUrlNow=item.imgUrl2
        let qry = this.query.new()
        this.query.toW(qry,'pid',item.id,'EQ')
        this.query.toO(qry,'seq','asc')
        this.list3=await this.api.getClassify(this.query.toEncode(qry))
        console.log('船舶列表')
        this.list3=this.list3.slice(0,5)
        console.log(this.list3)
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

    },
  }
</script>

<style lang="less" scoped>
  @import url("../../assets/css/init.less");
  #home{
    background-color: #FFFFFF;
  }
  .header{
    .bc{
      height: 52px;
      display: flex;
      align-items: center;
    }
    .textRed{
      color: #0064B2;
    }
    .navBox{
      .nav{
        display: flex;
        justify-content: space-between;
        padding:30px 0;
        margin: 0 auto;
        .navImgBox{
          width: 24%;
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
    /*.title{*/
    /*  display: inline-flex;*/
    /*  p{*/
    /*    font-size: 26px;*/
    /*    color: #333333;*/
    /*    display: flex;*/
    /*    align-items: center;*/
    /*    line-height: 76px;*/
    /*    .line{*/
    /*      display: block;*/
    /*      width: 5px;*/
    /*      height: 22px;*/
    /*      background-color: #2778BE;*/
    /*      margin: 0 16px 0 20px;*/
    /*    }*/
    /*  }*/
    /*}*/
    /*.content{*/
    /*  display: flex;*/
    /*  .left{*/
    /*    width: 33.3%;*/
    /*    border: 1px solid #F3F3F3;*/
    /*    position: relative;*/
    /*    cursor: pointer;*/
    /*    display: flex;*/
    /*    display: -webkit-flex;*/
    /*    p{*/
    /*      position: absolute;*/
    /*      width: 100%;*/
    /*      bottom: 30px;*/
    /*      text-align: center;*/
    /*      font-size: 24px;*/
    /*      color: #ffffff;*/
    /*      span{*/
    /*        font-size: 18px;*/
    /*      }*/
    /*    }*/
    /*    >img{*/
    /*      margin: auto;*/
    /*    }*/
    /*  }*/
    /*  .right{*/
    /*    flex: 1;*/
    /*    display: flex;*/
    /*    flex-wrap: wrap;*/
    /*    flex-direction: row;*/
    /*    justify-content: space-between;*/
    /*    margin-left: 20px;*/
    /*    .list{*/
    /*      display: flex;*/
    /*      flex-wrap: wrap;*/
    /*      flex-direction: column;*/
    /*      width: 32%;*/
    /*      border: 1px solid #F3F3F3;*/
    /*      align-items: center;*/
    /*      justify-content: center;*/
    /*      cursor: pointer;*/
    /*      .showContent{*/
    /*        width: 100%;*/
    /*        height: 100%;*/
    /*        display: flex;*/
    /*        display: -webkit-flex;*/
    /*        align-items: center;*/
    /*        justify-content: center;*/
    /*        flex-direction: column;*/
    /*        >img{*/
    /*          max-height: 51px;*/
    /*          max-width: 56.7px;*/
    /*        }*/
    /*        >p{*/
    /*          width: 100%;*/
    /*          text-align: center;*/
    /*          font-size: 22px;*/
    /*          color: #666666;*/
    /*          margin-top: 24px;*/
    /*        }*/
    /*      }*/
    /*      .transContent{*/
    /*        height:100%;*/
    /*        box-shadow:0 5px 13px 0 rgba(86,86,86,0.33);*/
    /*        display: flex;*/
    /*        flex-wrap: wrap;*/
    /*        display: -webkit-flex;*/
    /*        align-items: center;*/
    /*        justify-content: center;*/
    /*        flex-direction: column;*/
    /*        box-sizing: border-box;*/
    /*        color: rgba(255,255,255,0.7);*/
    /*        font-size: 12px;*/
    /*        z-index: 9;*/
    /*        cursor: pointer;*/
    /*        p{*/
    /*          display: flex;*/
    /*          display: -webkit-flex;*/
    /*          align-items: center;*/
    /*          justify-content: center;*/
    /*          margin-bottom: 30px;*/
    /*          font-size: 16px;*/
    /*          color: rgba(255,255,255,1);*/
    /*          img{*/
    /*            width: 30px;*/
    /*            margin-right: 8px;*/
    /*          }*/
    /*        }*/
    /*      }*/
    /*      &:first-of-type{*/
    /*        margin-bottom: 20px;*/
    /*      }*/
    /*      &:nth-of-type(2){*/
    /*        margin-bottom: 20px;*/
    /*      }*/
    /*      &:nth-of-type(3){*/
    /*        margin-bottom: 20px;*/
    /*      }*/
    /*    }*/
    /*    .active{*/
    /*      box-shadow:0 5px 13px 0 rgba(86,86,86,0.33);*/

    /*    }*/
    /*  }*/
    /*}*/
  }
  .content1{
    margin-top: 10px;
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: column;
    .title{
      /*margin-top: 52px;*/
      p{
        color: #333333;
        font-size: 20px;
        >img{
          width: 94px;
          margin-right: 20px;
          margin-left: 20px;
        }
      }
    }
    .cont{
      margin-top: 30px;
      width: 100%;
      >img{
        width: 100%;
      }
    }

  }
  .body{
    margin-top: 30px;
    .title{
      display: inline-flex;
      align-items: center;
      width: 100%;
      >p{
        font-size: 26px;
        color: #333333;
        display: flex;
        flex-direction: row;
        align-items: center;
        line-height: 76px;
        width: 15%;
        .line{
          display: block;
          width: 5px;
          height: 22px;
          background-color: #2778BE;
          margin: 0 16px 0 20px;
        }
      }
      ul{
        flex: 1;
        display: flex;
        flex-direction: row;
        justify-content: space-between;
        margin-left: 30%;
        height: auto;
        li{
          width: auto;
          display: flex;
          align-items: center;
          cursor: pointer;
          line-height: 35px;
          >p{
            color: #666;
            font-size: 14px;
            line-height: 35px;
          }
          .line2{
            display: block;
            width: 1px;
            height: 14px;
            opacity:0.2;
            background-color: #000;
            margin-left: 20px;
          }
        }
        .active{
          >p{
            color: #0064B2;
            border-bottom: 2px solid #2778BE;
          }
        }
      }
      .more{
        font-size: 14px;
        line-height: 30px;
        color: #999999;
        margin-left: 20px;
        width: auto;
        cursor:pointer;
      }
    }
    .content{
      width: 100%;
      .down{
        margin-top: 5px;
        width: 100%;
        flex: 1;
        display: flex;
        justify-content: space-between;
        .part{
          flex: 1;
          margin-right: 5px;
          display: flex;
          flex-direction: column;
          // flex-wrap: wrap;
          position: relative;
          img{
            width: 100%;
            height: 100%;
            border: 1px solid #999999;
          }
          .ct{
            position: absolute;
            bottom: 13px;
            width: 100%;
            align-items: center;
            text-align: center;
            p:nth-child(1){
              line-height: 30px;
              font-size: 20px;
              text-align: center;
              color: #FFFFFF;
              // color: #999999;
            }
            p:nth-child(2){
              line-height: 20px;
              text-align: center;
              color: #FFFFFF;
              // color: #999999;
            }
          }
        }
        .part:last-child{
          margin-right: 0;
        }
      }
    }
  }
  .body2{
    margin-top: 60px;
    .title{
      width: 100%;
      >p{
        font-size: 30px;
        text-align: center;
        color: #333333;
        line-height: 90px;
        >img{
          width: 21px;
          height: 21px;
          vertical-align: middle;
          margin-left: 28px;
        }
      }
    }
    /*手风琴*/
    .sfq{
      display: flex;
      display: -webkit-flex;
      height: 500px;
      >div{
        flex: 1;
        height: 100%;
        overflow: hidden;
        cursor: pointer;
        background-size: cover;
        background-position: center;
        .hiddenContent{
          width: 100%;
          height: 100%;
          position: relative;
          p{
            position: absolute;
            width: 100%;
            left: 0;
            bottom: 25px;
            text-align: center;
            color: #ffffff;
            font-size: 16px;
          }
        }
        .showContent{
          width: 100%;
          height: 100%;
          background: rgba(39, 120, 190, 0.7);
          box-sizing: border-box;
          color: #ffffff;

          p{
            padding-top: 150px;
            text-align: center;
            font-size: 24px;
          }
          .cont{
            font-size: 16px;
            padding-top: 29px;
            height: 150px;
          }
          ul{
            padding-top: 29px;
            height: 150px;
            width: 70%;
            margin: 0 auto;
            li{
              display: inline-block;
              width: 32%;
              line-height: 30px;
            }
            li:hover{
              text-decoration: underline;
            }
          }
          span{
            display: block;
            text-align: center;
          }
          span:hover{
            text-decoration: underline;
          }
        }
      }
      .showItem{
        flex: 2.8;
        animation:toShow 0.5s;
        -webkit-animation:toShow 0.5s ; /* Safari 和 Chrome */
      }
      .hiddenItem{
        flex: 1;
        animation:toHidden 0.5s;
        -webkit-animation:toHidden 0.5s ; /* Safari 和 Chrome */
      }
      /*显示动画*/
      @keyframes toShow
      {
        from {flex: 1;}
        to {flex: 2.8;}
      }

      @-moz-keyframes toShow /* Firefox */
      {
        from {flex: 1;}
        to {flex: 2.8;}
      }

      @-webkit-keyframes toShow /* Safari 和 Chrome */
      {
        from {flex: 1;}
        to {flex: 2.8;}
      }

      @-o-keyframes toShow /* Opera */
      {
        from {flex: 1;}
        to {flex: 2.8;}
      }
      /*隐藏*/
      @keyframes toHidden
      {
        from {flex: 2.8; }
        to {flex: 1;}
      }

      @-moz-keyframes toHidden /* Firefox */
      {
        from {flex: 2.8; }
        to {flex: 1;}
      }

      @-webkit-keyframes toHidden /* Safari 和 Chrome */
      {
        from { flex: 2.8;}
        to {flex: 1;}
      }

      @-o-keyframes toHidden /* Opera */
      {
        from { flex: 2.8;}
        to {flex: 1;}
      }
    }

  }
  .footerImg{
    margin-top: 25px;
    width: 100%;
  }
</style>
