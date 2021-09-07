<template>
<!--  基本信息-->
  <div>
    <p class="title">设计院简介</p>
    <div class="info">
      <p style="font-size:16px;font-weight:bold;color: #333333">{{info.company}}</p>
      <p style="color: #666666;margin-top: 20px" ><span>优势设计:</span>{{info.business}}</p>
      <p style="margin-top: 29px;width: 633px;">
        <!-- <span>ISO9001:</span> -->{{info.summery}}
      </p>
      <p style="margin-top: 29px"><span>公司地址:</span>{{info.address}}</p>
      <p style="margin-top: 4px"><span>公司网址:</span>{{info.linkMan}} </p>
      <p style="margin-top: 4px"><span>联系电话:</span>{{info.mob}}</p>
      <p style="margin-top: 4px;margin-bottom: 20px"><span>联系邮箱:</span>{{info.email}}</p>

      <div style="display: flex">
        <div class="list" >
          <p>
            <img class="icon" :src="yingyezhizhao">
            <span>企业营业执照</span>
          </p>
          <img class="pre" :src="info.businessLicense" v-viewer>
        </div>


        <div class="list">
          <p>
            <img class="icon" :src="zizhizhengshu">
            <span>资质证书</span>
          </p>
          <img class="pre" v-show="info.qualification" :src="info.qualification" v-viewer>
        </div>

        <!-- <div class="list">
          <p>
            <img class="icon" :src="shenfenzheng">
            <span>法人身份证</span>
          </p>
          <img class="pre2" :src="info.idCard" v-viewer>
          <img class="pre2" :src="info.idCardReverse" v-viewer>
        </div> -->

      </div>
    </div>
    <p class="title">公司简介</p>
      <div v-html="info.intro">
      </div>
  </div>

</template>

<script>
   import  imgTest from '../../assets/img/car.png'
   import shenfenzheng from '../../assets/img/personal/shenfenzheng.png'
   import yingyezhizhao from '../../assets/img/personal/yingyezhizhao.png'
   import zizhizhengshu from '../../assets/img/personal/zizhizhengshu.png'
   import test1 from '../../assets/img/personal/test1.png'
    export default {
        name: "fangansheji",
        props:{
          title:{
              type:String,
              default:'设计'
          }
        },
        data(){
            return{
                imgTest,
                shenfenzheng,
                yingyezhizhao,
                zizhizhengshu,
                test1,
                info:{},
                list:[],
                id:'',
            }
        },
        mounted() {
          this.id= this.until.getQueryString('id')
          this.getInfo()
        },
        methods:{
          async getInfo(){
            this.currentRole=JSON.parse(this.until.seGet('currentRole'))
            let param={
              docsId:this.id,
              identityCd:'identity20'
            }
            this.info=await this.api.orgInfoBasic(param)
            console.log(this.info)
            // let dataList=JSON.parse(this.info.mainParams)
            // this.paraList=dataList.args
            // console.log(this.paraList)
          },
        }

    }
</script>

<style scoped lang="less">

  .title{
    width: 100%;
    height: 40px;
    background: #EEEEEE;
    color: #666666;
    padding-left: 28px;
    display: flex;
    align-items: center;
  }
  .info{
    margin-top: 30px;
    color: #999999;
    .list{
      display: flex;
      align-items: flex-start;
      margin-bottom: 24px;
      color: #666666;
      flex:1;
      p{
        display: flex;
        align-items: center;
        .icon{
          margin-right: 11px;
          width:19px;
          height:21px;
        }
      }
      .pre{
        // width:113px;
        width: auto;
        height:90px;
        margin-left: 11px;
      }
      .pre2{
        margin-left: 11px;
        width: 67px;
        height: 41px;
      }
    }
  }
</style>
