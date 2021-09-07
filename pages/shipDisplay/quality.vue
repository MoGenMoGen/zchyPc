<template>
<div id="home" :style="{width:bWidth + 'px'}">
  <img class="adert" alt="" src="../../assets/img/shipDisplay/lunbot.png" :style="{width:bWidth + 'px'}"/>
  <div class="header main" :style="{width:width + 'px'}">
    <div class="title">
      <p><span class="line"></span>品质保障</p>
    </div>
    <div class="content">
      <div class="left">
        <el-image
          v-if="adertTop.length>0"
          :src="adertTop[0].imgUrl"
          @click="toPage(adertTop[0].locUrl)"
          fit="cover"></el-image>
          <!-- <img :src="adertTop[0].imgUrl"
               v-if="adertTop.length>0"
               class="el-images"
               ref="img"
               @click="toPage(adertTop[0].locUrl)"
          /> -->
      </div>
      <div class="right">
        <div class="list" v-for="(item,index) in list" :key="index" :style="{height:width*0.66*0.32+'px'}" @mouseenter="changeLiTrans(index,1)" @mouseleave="changeLiTrans(index,0)"  @click="toPage(item.href)">
          <div v-show="item.ifTrans"  class="transContent toScale" :style="{width:width*0.66*0.32+8+'px',background:item.color}">
            <img :src="item.img2"/>
            <p>{{item.nm}}</p>
          </div>
          <div class="showContent" v-show="!item.ifTrans">
            <img :src="item.img"/>
            <p>{{item.nm}}</p>
          </div>
        </div>
      </div>
    </div>
  </div>
<!--  <div class="top">-->
<!--  </div>-->
  <div class="body" :style="{width:bWidth + 'px'}">
    <img :src="item.imgUrl" v-for="item in adertList"  @click="toPage(item.locUrl)"/>

  </div>
  <div class="foot main" :style="{width:width + 'px'}">
    <div class="title">
      <p>成功客户<img src="@/assets/img/shipDisplay/更多.png" alt=""/></p>
    </div>
    <div class="center">
      <div class="part" v-for="(item,index) in list2" :key="index">
        <img :src="item.imgUrl" alt=""/>
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
    import div from '@/assets/img/shipDisplay/DIV报价.png';
    import jljz from '@/assets/img/shipDisplay/监理监造机构.png';
    import zljc from '@/assets/img/shipDisplay/质量检测机构.png';
    import manage from '@/assets/img/shipDisplay/船舶管理.png';
    import design from '@/assets/img/shipDisplay/船舶设计.png';
    import build from '@/assets/img/shipDisplay/船舶建造.png';
    import div2 from '@/assets/img/shipDisplay/DIV报价-白.png';
    import jljz2 from '@/assets/img/shipDisplay/监理监造机构-白.png';
    import zljc2 from '@/assets/img/shipDisplay/质量检测机构-白.png';
    import manage2 from '@/assets/img/shipDisplay/船舶管理-白.png';
    import design2 from '@/assets/img/shipDisplay/船舶设计-白.png';
    import build2 from '@/assets/img/shipDisplay/船舶建造-白.png';
    export default {
        name: "quality",
      data(){
          return{
            list:[{
              nm:'DIV报价',
              img:div,
              img2:div2,
              color:'#EC51EE',
              href:'./shipDIY?tab=1',
            },{
              nm:'建造监理机构',
              img:jljz,
              img2:jljz2,
              color:'#3EA708',
              href:'',
            },{
              nm:'质量检测机构',
              img:zljc,
              img2:zljc2,
              color:'#FF5931',
              href:'',
            },{
              nm:'船舶管理',
              img:manage,
              img2:manage2,
              color:'#FF9A28',
              href:'./shipMan',
            },{
              nm:'船舶设计',
              img:design,
              img2:design2,
              color:'#2778BE',
              href:'./shipDesign',
            },{
              nm:'船舶建造',
              img:build,
              img2:build2,
              color:'#00BAB4',
              href:'./shipBuild',
            }],
            adertTop:[],
            adertList:[],
            list2:[],
        }
      },
      computed: {
        ...mapState([
          'bWidth',
          'width',
          'currentRole'
        ])
      },
      mounted() {
        if(this.until.getQueryString('pos')){
          document.getElementById(this.until.getQueryString('pos')).scrollIntoView()
        }
        this.getAdert()
      },
      methods:{
        async getAdert(){
          this.adertTop = await this.api.getAdert('PCqualityTop',1)
          this.adertList = await this.api.getAdert('PCquality')
          this.list2 = await this.api.getAdert('successCust')
        },
        toPage(url){
          if(url){
            this.$router.push(url)
          }
        },
        //鼠标滑动切换放大效果
        changeLiTrans(index,type){
          this.list[index].ifTrans = type ? true : false
          this.$set(this.list,index,this.list[index])
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
    background-color: #FFFFFF;
    .title{
      display: inline-flex;
      p{
        font-size: 26px;
        color: #333333;
        display: flex;
        align-items: center;
        line-height: 76px;
        .line{
          display: block;
          width: 5px;
          height: 22px;
          background-color: #2778BE;
          margin: 0 16px 0 20px;
        }
      }
    }
    .content{
      display: flex;
      .left{
        width: 33.3%;
        display: flex;
        border: 1px solid #F3F3F3;
        img{
          cursor: pointer;
        }
      }
      .right{
        flex: 1;
        display: flex;
        flex-wrap: wrap;
        flex-direction: row;
        justify-content: space-between;
        margin-left: 20px;
        .list{
          display: flex;
          flex-wrap: wrap;
          flex-direction: column;
          width: 32%;
          border: 1px solid #F3F3F3;
          align-items: center;
          justify-content: center;
          .showContent{
            width: 100%;
            height: 100%;
            display: flex;
            display: -webkit-flex;
            align-items: center;
            justify-content: center;
            flex-direction: column;
            >img{
              max-height: 51px;
              max-width: 56.7px;
            }
            >p{
              width: 100%;
              text-align: center;
              font-size: 22px;
              color: #666666;
              margin-top: 24px;
            }
          }
          .transContent{
            height:100%;
            box-shadow:0 5px 13px 0 rgba(86,86,86,0.33);
            display: flex;
            flex-wrap: wrap;
            display: -webkit-flex;
            align-items: center;
            justify-content: center;
            flex-direction: column;
            box-sizing: border-box;
            color: rgba(255,255,255,0.7);
            font-size: 12px;
            z-index: 9;
            cursor: pointer;
            >img{
              max-height: 51px;
              max-width: 56.7px;
            }
            >p{
              width: 100%;
              text-align: center;
              font-size: 22px;
              color: #FFFFFF;
              margin-top: 24px;
            }
          }

          &:first-of-type{
            margin-bottom: 20px;
          }
          &:nth-of-type(2){
            margin-bottom: 20px;
          }
          &:nth-of-type(3){
            margin-bottom: 20px;
          }

        }
      }
    }
  }
  .body{
    margin-top: 90px;
    img{
      width: 100%;
      cursor: pointer;
    }
  }
  .foot{
    margin-top: 70px;
    .title{
      display: inline-flex;
      width: 100%;
      justify-content: center;
      >p{
        font-size: 30px;
        color: #333;
        line-height: 80px;
        >img{
          width: 21px;
          height: 21px;
          vertical-align: middle;
          margin-left: 17px;
        }
      }
    }
    .center{
      display: flex;
      flex-direction: row;
      flex-wrap: wrap;
      justify-content: space-between;
      width: 100%;
      .part{
        border: 1px solid #D9D9D9;
        width: 18%;
        height: 100px;
        margin-bottom: 30px;
        display: flex;
        justify-content: center;
        padding: 10px 20px;
        // >img{
        //   margin: auto;
        //   max-width: 100%;
        //   max-height: 100px;
        // }
      }
    }
  }
  .footerImg{
    width: 100%;
    margin-top: 53px;
  }
/*.top{*/
/*  width:100px;*/
/*  height:100px;*/
/*  background:blue;*/
/*  transition:width 2s;*/
/*  -moz-transition:width 2s; !* Firefox 4 *!*/
/*  -webkit-transition:width 2s; !* Safari and Chrome *!*/
/*  -o-transition:width 2s; !* Opera *!*/
/*}*/
/*  .top:hover{*/
/*    width: 1200px;*/
/*  }*/
</style>
