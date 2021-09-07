<template>
<!--  选型记录详情-->
  <div id="home">
    <div class="title">
      <p><span class="lineC"></span>选型详情</p>
      <p @click="back"><i class="el-icon-arrow-left"></i>返回&nbsp;<span>&nbsp;选型记录</span></p>
    </div>
    <div class="header">
      <img alt="" src="@/assets/img/personal/左.png"/>
      <p>基本参数</p>
      <img alt="" src="@/assets/img/personal/右.png"/>
    </div>
    <div class="content1">
      <div class="detail" >
        <p><span>船舶类型：</span>{{info.typesNm}}</p>
        <p><span>船舶总长：</span>{{info.lengthNm}}</p>
        <p><span>船舶材质：</span>{{info.hullMaterialNm}}</p>
        <p><span>船舶户籍：</span>{{info.registry}}</p>
        <p><span>船舶航区：</span>{{info.sailingAreaNm}}</p>
        <p><span>船舶定员：</span>{{info.complement}}</p>
        <p><span>船舶型宽：</span>{{info.width}}</p>
        <p><span>船舶型深：</span>{{info.deep}}</p>
        <p><span>公约船长：</span>{{info.pactLen}}</p>
        <p><span>船舶总吨数：</span>{{info.tonnage}}</p>
        <p><span>船舶航速：</span>{{info.speed}}</p>
        <p><span>冷冻系统：</span>{{info.freezingSys == 1?'有':'无'}}</p>
        <p><span>主机品牌：</span>{{info.hostBrandNm}}</p>
        <p><span>制冷机组品牌：</span>{{info.refrigeratorBrandNm}}</p>
        <p><span>发电机组品牌：</span>{{info.electricGeneratorBrandNm}}</p>
        <p><span>主机功率：</span>{{info.hostPowerNm}}</p>
        <p><span>制冷机组功率：</span>{{info.refrigeratorPowerNm}}</p>
        <p><span>发电机功率：</span>{{info.electricGeneratorPowerNm}}</p>
        <p v-for="(item,index) in levelList" :key="index"><span>{{item.split('：')[0]}}：</span>{{item.split('：')[1]}}</p>
        <p><span>其他要求：</span>{{info.intro}}</p>

      </div>
      <div class="footer" >
        <p><span>提交时间：</span>{{info.crtTm}}</p>
        <p><span>整船参考价格：</span>{{info.price?'￥'+info.price+'起':'暂无报价，详情请咨询客服'}}</p>
      </div>
    </div>

  </div>
</template>

<script>
  import {mapState} from "vuex";

  export default {
    layout:'person',
    name: "recordDetail",
    components: { },
    data(){
      return{
        id:4992971867640832,
        info:{},
        levelList:[],
      }
    },
    computed:{
      ...mapState([
        'bWidth',
        'width'
      ])
    },
    mounted() {
      this.id = this.$route.query.id
      this.getData()

      },
    methods:{
      async getData(id){
        id = this.id
        let data = await this.api.diyDetail(id)
        this.info = data
        if(this.info.coefCont){
          this.levelList = this.info.coefCont.split(',')
        }
        console.log(data)
      },
      back(){
        this.$router.go(-1)
      },
    },
  }
</script>

<style lang="less" scoped>
  @import url("../../assets/css/init.less");
  #home{

  }
  .title{
    height: 59px;
    border-bottom: 1px solid #F3F3F3;
    width: 100%;
    display: inline-flex;
    justify-content: space-between;
    line-height: 59px;
    p:nth-child(1){
      color: #333333;
      font-size: 20px;
      display: flex;
      align-items: center;
      margin-left: 30px;
      .lineC{
        display: block;
        height: 18px;
        width: 3px;
        background-color: #2778BE;
        margin-right: 8px;
      }
    }
    p:nth-child(2) {
      color: #666666;
      font-size: 14px;
      margin-right: 24px;
      cursor: pointer;
    }
  }
  .header{
    width: 100%;
    display: inline-flex;
    align-items: center;
    justify-content: center;
    padding-top: 60px;
    padding-bottom: 50px;
    >img{
      width: 130px;
    }
    >p{
      font-size: 24px;
      color: #303030;
      padding: 0 40px;
    }
  }
  .content1{
    width: 100%;
    background: #FFFFFF;
    display: flex;
    flex-wrap: wrap;
    align-items: center;
    .detail{
      padding-left: 80px;
      width: 100%;
      display: flex;
      align-items: center;
      flex-wrap: wrap;
      >p{
        width: 33.3%;
        font-size: 15px;
        color: #666666;
        margin-bottom: 40px;
      }
    }
    .footer{
      background-color: #F3F3F3;
      height: 52px;
      padding:0 30px ;
      display: inline-flex;
      align-items: center;
      justify-content: space-between;
      margin: 50px auto;
      width: 93.8%;
      span{
        font-size: 14px;
        color: #666666;
      }
      p:nth-child(2){
        font-size: 24px;
        color: #E64347;
      }
    }
  }
</style>
