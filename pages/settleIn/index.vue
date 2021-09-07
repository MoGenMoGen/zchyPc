<template>
  <div class="settleIn" :style="{width:bWidth+'px'}">
      <my-header></my-header>
    <div class="main" :style="{width:width+'px'}">
      <div class="title">
         <img :src="left"/>
         我要入驻
         <img :src="right"/>
      </div>
      <scale-list :list="list"></scale-list>
    </div>
  </div>
</template>

<script>
import myHeader from './component/myHeader'
import scaleList from '@/components/scaleList2'
import {mapState} from "vuex";
import left from './component/img/left.png'
import right from './component/img/right.png'
import sz1 from './component/img/11.png'
import szh1 from './component/img/11a.png'
import sz2 from './component/img/12.png'
import szh2 from './component/img/12a.png'
import sz3 from './component/img/13.png'
import szh3 from './component/img/13a.png'
import sz4 from './component/img/14.png'
import szh4 from './component/img/14a.png'
import sz5 from './component/img/21.png'
import szh5 from './component/img/21a.png'
import sz6 from './component/img/22.png'
import szh6 from './component/img/22a.png'
import sz7 from './component/img/23.png'
import szh7 from './component/img/23a.png'
import sz8 from './component/img/24.png'
import szh8 from './component/img/24a.png'
import sz9 from './component/img/31.png'
import szh9 from './component/img/31a.png'
export default {
  head () {
    return {
      title: '我要入驻',
    }
  },
  data(){
    return{
      left,
      right,
      list:[{
        nm:'船东入驻',
        ifTrans:false,
        color:'#2778BE',
        colorRGB:'39,120,190',
        // img:'https://fuss10.elemecdn.com/8/27/f01c15bb73e1ef3793e64e6b7bbccjpeg.jpeg',
        img:sz1,
        imgH:szh1,
        href:'chuandong',
        cont:'船舶管理是新材料船舶数字化平台的核心功能之一，是全生命周期服务、从设计到造船、维保等全过程管控，可以使船东享受到规模化集采、DIY报价、在线发包、在线云监造、在线检测等省心、放心的一站式服务。'
      },{
        color:'#3EA708',
        colorRGB:'62,167,8',
        nm:'船厂入驻',
        img:sz2,
        imgH:szh2,
        href:'chuanchang',
        cont:'可以同时满足船东、船厂、原材料供应商、设备供应商、服务商的全天候的展示、交易、下单、结算、物流、售后、业务推广等服务'
      },{
        color:'#FF9A28',
        colorRGB:'255, 154, 40',
        nm:'设计院入驻',
        img:sz3,
        imgH:szh3,
        href:'shejiyuan',
        cont:'金融服务是新材料船舶产业的短板和痛点。'
      },{
        color:'#8C41DA',
        colorRGB:'140, 65, 218',
        nm:'供应商入驻',
        img:sz4,
        imgH:szh4,
        href:'gongyingshang',
        cont:'创新是一个企业的核心竞争力之一。'
      },{
        color:'#00BAB4',
        colorRGB:'0, 186, 180',
        nm:'检验检测机构入驻',
        img:sz5,
        imgH:szh5,
        href:'jiancijigou',
        cont:'智慧船舶集成平台包括智慧航行、智慧远程服务、智慧能耗管理等系统，形成船上数据与应用的统一集成平台。'
      },{
        color:'#EC51EE',
        colorRGB:'236, 81, 238',
        nm:'建造监理机构入驻',
        img:sz6,
        imgH:szh6,
        href:'jianlijigou',
        cont:'全生命周期服务，是新材料船舶数字化平台的重要特色。包括船舶报废、人力资源管理（劳务公司）、资源共享、安全管理、维保服务、二手船交易、供应链金融等服务。'
      },{
        color:'#2982FA',
        colorRGB:'41, 130, 250',
        nm:'经销商入驻',
        img:sz7,
        imgH:szh7,
        href:'jingxiaoshang',
        cont:'用户可以在平台发布行业分类信息，不限于造船指标拍卖、闲置设备转让、资源共享、劳务需求等信息'
      },{
        color:'#FF5931',
        colorRGB:'255, 89, 49',
        nm:'代理商入驻',
        img:sz8,
        imgH:szh8,
        href:'dailishang',
        cont:'现在是知识经济时代，在线学习板块可以让所有使用本平台的会员，利用移动互联网的优势, 利用碎片化的时间，在任何时刻、任何地方让船东、船厂等企业员工继续学习'
      },{
        color:'#FF5151',
        colorRGB:'255, 81, 81',
        nm:'服务商入驻',
        img:sz9,
        imgH:szh9,
        href:'fuwushang',
        cont:'现在是知识经济时代，在线学习板块可以让所有使用本平台的会员，利用移动互联网的优势, 利用碎片化的时间，在任何时刻、任何地方让船东、船厂等企业员工继续学习'
      }],
    }
  },
  computed: {
    ...mapState([
      'bWidth',
      'width'
    ])
  },
  components: {
    myHeader,
    scaleList
  },
  async mounted(){
    this.list = await this.api.getRoleList()

  },
  methods:{


    //页面跳转
    toPage(url){

    }
  }
}
</script>
<style lang="less">


</style>
<style lang="less" scoped>
  @import url("../../assets/css/init.less");
  .settleIn{
    background-color: #ffffff;
    background-image: url("../../assets/img/bg.png");
    padding-bottom: 137px;
    background-position: bottom center;
    background-repeat: repeat-x;
    .title{
      display: flex;
      display: -webkit-flex;
      width: 100%;
      align-items: center;
      justify-content: center;
      font-size: 24px;
      color: #333333;
      padding-top: 40px;
      padding-bottom: 40px;
      img{
        width: 148px;
        margin: 0 10px;
      }
    }
  }
</style>
