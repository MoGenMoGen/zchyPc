<template>
<!--  基本信息-->
  <div>
    <p class="title">船厂简介</p>
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

      <p class="list" v-for="item in list" :key="item.id">
        <img :src="item.img">
        {{item.nm}}

      </p>
    </div>
    <p class="title" style="margin-bottom: 10px;">船厂展示图片</p>
      <div v-html="info.intro">

      </div>
  </div>

</template>

<script>
   import  imgTest from '../../assets/img/car.png'
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
                list:[],
                info:{},
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
              identityCd:'identity30'
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
      align-items: center;
      margin-bottom: 24px;
      color: #666666;
      img{
        margin-right: 11px;

        width:19px;
        height:21px;
      }
    }
  }
</style>
