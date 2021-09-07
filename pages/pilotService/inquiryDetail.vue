<template>
  <!-- 补贴申请结果 -->
<div id="home" :style="{width:bWidth + 'px'}">
  <div class="header">
    <el-carousel :height="bannerHeight+'px'">
      <el-carousel-item v-for="item in adertList" :key="item.id">
        <img :src="item.imgUrl" style="width:100%">
      </el-carousel-item>
    </el-carousel>
    <el-breadcrumb separator-class="el-icon-arrow-right" :style="{width:width+'px'}" class="bc main">
      <el-breadcrumb-item :to="{ path: './pilotService' }">试点服务</el-breadcrumb-item>
      <el-breadcrumb-item :to="{ path: './onlineInquiry' }">补贴申请列表</el-breadcrumb-item>
      <el-breadcrumb-item ><p class="textRed">补贴申请结果</p></el-breadcrumb-item>
    </el-breadcrumb>
  </div>
  <div class="ad main detailBox" :style="{width:width+'px'}">
    <p v-if="info.type=='1'||info.type=='2'"><i class="iconfont icon-jinggao--" style="color: #FFA03C;"></i></p>
    <p v-if="info.type=='3'"><i class="iconfont icon-chenggong" style="color: #49BD3A;"></i></p>
    <p v-if="info.type=='4'"><i class="iconfont icon-jinggao--" style="color: #FF5E18;"></i></p>
    <p v-if="info.type=='3'" class="content">{{content}} <span style="color: #FF3535;">{{info.subsidyAmt}}</span>万元</p>
    <p v-else class="content">{{content}}</p>
  </div>
  <div class="footerImg" :style="{width:bWidth+'px'}">
    <img src="@/assets/img/footer.png" alt=""/>
  </div>
</div>
</template>

<script>
  import {mapState} from "vuex";
  export default {
        name: "vision",
      data(){
          return{
            bannerHeight:'300',
            adertList:[],
            id:'',
            type:'',
            info:{},
            content:'',
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
      this.id= this.until.getQueryString('id')
      // this.type= this.until.getQueryString('type')
      this.getAdert()
      this.getInfo()
    },
    methods:{
      async getInfo(){
        this.info=await this.api.subsidiesDetail(this.id)
        if(this.info.auditCd=='PILOT_STATUS.10'){  //待审核
          this.info.type=1
          this.content='审核中，请稍后在查询补贴申请结果'
        }else if(this.info.auditCd=='PILOT_STATUS.20'){  //审核中
          this.info.type=2
          this.content='审核中，请稍后在查询补贴申请结果'
        }else if(this.info.auditCd=='PILOT_STATUS.30'){  //审核通过
          this.info.type=3
          this.content='经审核，该项目符合财政补助资金申请条件，应补助金额总计为：'
        }else if(this.info.auditCd=='PILOT_STATUS.40'){  //审核未通过
          this.info.type=4
          this.content='经审核，该项目不符合财政补助资金申请条件。'
        }
        console.log(this.info)
      },
      async getAdert(){
        this.adertList = await this.api.getAdert('subsidies')
        if(this.adertList.length>0){
          let myImg = new Image()
          myImg.src = this.adertList[0].imgUrl
          myImg.onload = ()=>{
            if(myImg.width>this.bWidth){
              this.bannerHeight = myImg.height/myImg.width*this.bWidth
            }else {
              this.bannerHeight = myImg.height
            }
          }
        }
      },
      toPage(url){
        this.$router.push(url)
      }
    }
  }
</script>

<style lang="less" scoped>
  @import url("../../assets/css/init.less");
#home{
  background-color: #FFFFFF;
  .header{
    .bc{
      height: 52px;
      display: flex;
      align-items: center;
      .textRed{
        color: #0064B2;
      }
    }
    .ad{
      margin-top: 11px;
    }
  }
  .detailBox{
    display: flex;
    flex-direction: column;
    align-items: center;
    padding-top: 50px;
    .content{
      color: #2F2F2F;
      font-size: 18px;
    }
    >p{
      margin-bottom: 28px;
      .iconfont{
        font-size: 45px;
      }
    }
  }
  .footerImg{
    margin-top: 50px;
  }
}
</style>
