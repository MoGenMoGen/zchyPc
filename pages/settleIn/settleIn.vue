<template>
  <div class="settleIn" :style="{width:bWidth+'px'}">
      <my-header></my-header>
    <div class="main" :style="{width:width+'px'}">
      <div class="title">
         <img :src="left"/>
         我要入驻
         <img :src="right"/>
      </div>
      <div class="settleInMain">
        <div class="process">
            <div v-for="(item,index) in prcessList" :key="index" :class="{acitve:currentIndex>=item.index}">
               <img :src="currentIndex>=item.index ? arrowa : arrow" v-if="index>0"/>
               <p>
                 <img :src="currentIndex>=item.index ? item.imgA : item.img"/>
                 {{index+1+'.'+item.nm}}
               </p>
            </div>
        </div>
        <step1 :type="roleCd" @setInfo="setInfo" :info="ruleForm" v-show="currentIndex==0"></step1>
        <!--政府的第二步和提交成功-->
        <step2 :type="roleCd" @setInfo="setInfo" :info="ruleForm" v-show="ruleForm.identityCd=='identity100' && (currentIndex==1 || currentIndex==3)"></step2>
        <!--非政府的第二步-->
        <step2 :type="roleCd" @setInfo="setInfo" :info="ruleForm" v-show="ruleForm.identityCd!='identity100' && currentIndex==1 "></step2>
        <!--非政府的第三步和提交成功-->
        <step3 :type="roleCd" @setInfo="setInfo" :info="ruleForm" v-show="ruleForm.identityCd!='identity100' && (currentIndex==2 || currentIndex==3)"></step3>
      </div>
    </div>
  </div>
</template>

<script>
import myHeader from './component/myHeader'
import step1 from './component/step1'
import step2 from './component/step2'
import step3 from './component/step3'
import {mapState} from "vuex";
import left from './component/img/left.png'
import right from './component/img/right.png'
import process1 from './component/img/process1.png'
import process1a from './component/img/process1a.png'
import process2 from './component/img/process2.png'
import process2a from './component/img/process2a.png'
import process3 from './component/img/process3.png'
import process3a from './component/img/process3a.png'
import process4 from './component/img/process4.png'
import process4a from './component/img/process4a.png'
import arrow from './component/img/arrow.png'
import arrowa from './component/img/arrowa.png'
export default {
  head () {
    return {
      title: '我要入驻',
    }
  },
  data(){
    return{
      roleCd:'',
      left,
      right,
      arrow,
      arrowa,
      currentIndex:0,
      ruleForm:{
        logo:'',//logo
        imgUrl:'',//图片
        memId:'', //申请人ID(机构主账号会员id,userInfo.subId)
        identityCd:'',//入驻身份
        type:"1",//身份属性
        company:'',//公司名称
        business:'',//业务
        summery:'',//简介
        addrCd:'',//所在地区cd(格式: 110000-110100-110101)
        addrNm:'',//所在地区(格式: 北京-北京市-东城区)
        address:'',//详细地址
       linkMan:'',//联系人
       mob:'',//联系手机
       email:'',//电子邮箱
       businessLicense:'',//营业执照
      idCard:'',//法人身份证
        idCardReverse:'',//身份证反面
      qualification:'',//资质证明
      intro:'',//企业详细介绍
      cases:'',// 案例
      },
      prcessList:[
        {
          nm:'选择身份',
          img:process1,
          imgA:process1a,
          index:0
        },{
          nm:'填写基本信息',
          img:process2,
          imgA:process2a,
          index:1
        },{
          nm:'上传单位资料',
          img:process3,
          imgA:process3a,
          index:2
        },{
          nm:'提交成功',
          img:process4,
          imgA:process4a,
          index:3
        }
      ]
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
    step1,
    step2,
    step3
  },
  mounted(){
    console.log(this.until.seGet('userInfo'))
    this.roleCd = this.until.getQueryString('type')
    this.ruleForm.memId = JSON.parse(this.until.seGet('userInfo')).subId
  },
  watch:{
    'ruleForm.identityCd':{
      handler:function () {
        if(this.ruleForm.identityCd=='identity100'){
          this.prcessList = [ {
            nm:'选择身份',
            img:process1,
            imgA:process1a,
            index:0
          },{
            nm:'填写基本信息',
            img:process2,
            imgA:process2a,
            index:1
          },{
            nm:'提交成功',
            img:process4,
            imgA:process4a,
            index:3
          }]
        }else {
          this.prcessList = [ {
            nm:'选择身份',
            img:process1,
            imgA:process1a,
            index:0
          },{
            nm:'填写基本信息',
            img:process2,
            imgA:process2a,
            index:1
          },{
            nm:'上传单位资料',
            img:process3,
            imgA:process3a,
            index:2
          },{
            nm:'提交成功',
            img:process4,
            imgA:process4a,
            index:3
          }]
        }
      },
      immediate:true
    }
  },
  methods:{
    //上一步、下一步
    setInfo(data){
        this.currentIndex = data.nextIndex
        this.ruleForm = data.info
    },
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
      font-size: 28px;
      color: #333333;
      padding-top: 114px;
      padding-bottom: 78px;
      img{
        width: 148px;
        margin: 0 10px;
      }
    }
    .settleInMain{
      border: 1px solid #DFDFDF;
      box-sizing: border-box;
      min-height: 700px;
      .process{
        width: 100%;
        background: #F6F6F6;
        border-bottom: 1px solid #DFDFDF;
        display: flex;
        display: -webkit-flex;
        align-items: center;
        justify-content: center;
        padding: 23px 0;
        >div{
          display: flex;
          display: -webkit-flex;
          align-items: center;
          >img{
            width: 7px;
            margin: 0 40px;
          }
          p{
            display: flex;
            display: -webkit-flex;
            align-items: center;
            img{
              width: 40px;
              margin-right: 13px;
            }
          }
        }
        .acitve{
          color: @themeColor;
        }
      }
    }
  }
</style>
