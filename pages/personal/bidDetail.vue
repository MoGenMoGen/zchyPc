<template>
<div id="app">
  <div class="header">
    <p><span class="lineC"></span>投标详情</p>
    <span @click="back">< 返回 投标管理</span>
  </div>
  <div class="body">
    <div class="top">
      <p><span>项目名称：</span>{{info2.bidNm}}</p>
<!--      <p><span>发布方名称：</span>{{bidInfo.publisher}}</p>-->
      <p><span>发布时间：</span>{{info.publishTm}}</p>
      <p><span>完成时间：</span>{{info.completeTm}}</p>
    </div>
    <div class="mid">
      <div class="left">
        <p><span>报名时间：</span>{{info2.applyTm}}</p>
        <p><span>报价时间：</span>{{info2.offerTm}}</p>
        <p><span>报价金额：</span>{{info2.offerAmt}}元</p>
      </div>
      <div class="right">
        <span>上传附件：</span>
        <div>
          <p v-for="(item,index) in fileList" :key="index"><img :src="item.imgUrl"/>{{index+1}}.{{item.nm}}</p>
        </div>
      </div>
     </div>
   <div class="down">
    <p><span>中标状态：</span><span :class="[info.statusCd == 'BID_OFFER_STATUS.60'? 'green':'',info.statusCd == 'BID_OFFER_STATUS.70'? 'red':'']">{{info.statusNm}}</span></p>
    <p><span>评审时间：</span>{{info2.winBidTm}}</p>
  </div>
  </div>
</div>
</template>

<script>
  import {mapState} from "vuex";
  import excel from '@/assets/img/personal/excel.png'
  import ppt from '@/assets/img/personal/ppt.png'
  import word from '@/assets/img/personal/word.png'
  import pdf from '@/assets/img/personal/pdf.jpg'
  export default {
      name: "bidDetail",
      layout:'person',
      data(){
        return{
          excel,
          ppt,
          word,
          pdf,
          bidInfo:{

          },
          info:{
            bidId:'',
            orgId:'',
            statusCd:'',
            statusNm:'',
            publishTm:'',
            completeTm:'',
          },
          info2:{

          },
          fileList:[],
        }
      },
    watch:{
      $route(){
        this.info.bidId = this.$route.query.bidId
        this.info.orgId = this.$route.query.orgId
        this.info.statusCd = this.$route.query.statusCd
        this.info.statusNm = this.$route.query.statusNm
        this.info.publishTm = this.$route.query.publishTm
        this.info.completeTm = this.$route.query.completeTm
      },
        //截取文件名称加后缀
        info2(){
          if(this.info2){
            let list = this.info2.attachment.split(',')
            list.forEach(item =>{
              let type = item.split('.')[item.split('.').length - 1]
              if(type=='pdf'){
                this.fileList.push({imgUrl:this.pdf,nm:item.slice(item.indexOf('_') + 1), url: item})
              }else if(type=='doc'||type=='docx'){
                this.fileList.push({imgUrl:this.word,nm:item.slice(item.indexOf('_') + 1), url: item})
              }else if(type=='ppt'||type=='pptx'){
                this.fileList.push({imgUrl:this.ppt,nm:item.slice(item.indexOf('_') + 1), url: item})
              }else if(type=='xls'||type=='xlsx'){
                this.fileList.push({imgUrl:this.excel,nm:item.slice(item.indexOf('_') + 1), url: item})
              }
            })
          }
          console.log('11111',this.fileList)//添加后的数组

        },
    },
    mounted(){
      this.info.bidId = this.$route.query.bidId
      this.info.orgId = this.$route.query.orgId
      this.info.statusCd = this.$route.query.statusCd
      this.info.statusNm = this.$route.query.statusNm
      this.info.publishTm = this.$route.query.publishTm
      this.info.completeTm = this.$route.query.completeTm
      this.getData()

    },
    computed:{
      ...mapState([
        'bWidth',
        'width',
        "currentRole",
        'loading',
        'tel',
        'msgNum'
      ])
    },
    methods:{
        back(){
          this.$router.go(-1)
        },
      async getData(){
        let data = await this.api.bidOfferDetail(this.info)
        console.log(data,'data')
        this.info2 =data.data

      }
    },

  }
</script>

<style lang="less" scoped>
  @import url("../../assets/css/init.less");
#app{
  .header{
    display: flex;
    justify-content: space-between;
    align-items: center;
    height: 59px;
    width: 100%;
    padding: 0 24px 0 30px;
    border-bottom: 1px solid rgba(0,0,0,0.1);;
    >p{
      display: flex;
      align-items: center;
      font-size:20px;
      /*font-family:Microsoft YaHei;*/
      font-weight:400;
      color:#333333;
      height: 18px;
      .lineC{
        display: block;
        height: 18px;
        width: 3px;
        background-color: #2778BE;
        margin-right: 8px;
      }
    }
    >span{
      display: block;
      height:12px;
      font-size:14px;
      /*font-family:Microsoft YaHei;*/
      font-weight:400;
      color: #666666;
      cursor: pointer;
    }
  }
  .body{
    p{
      line-height: 35px;
      font-size: 15px;
      color: #333;
    }
    .top{
      display: flex;
      justify-content: center;
      flex-direction: column;
      padding-left: 35px;
      height:154px;
      border-bottom: 1px solid #DFDFDF;

    }
    .mid{
      padding: 20px 0 0 35px;
      width: 100%;
      height: 154px;
      border-bottom: 1px solid #DFDFDF;
      .left{
        width: 50%;
        float: left;
      }
      .right{
        width: 50%;
        float: right;
        display: flex;
        >span{
            line-height: 35px;
            font-size: 15px;
        }
        >div{
          display: flex;
          flex-direction: column;
          >p{
            color: #333333;
            height: 35px;
            line-height: 35px;
            font-size: 15px;
            overflow: hidden;
            text-overflow: ellipsis;
            white-space: nowrap;
            width: 300px;
            >img{
              width: 16px;
              height: 18px;
              vertical-align: sub;
              margin-right: 10px;
            }
          }
        }
      }
    }
    .down{
      padding: 20px 0 0 35px;
      width: 100%;
    }
  }
}
  .green{
    color: #21AE2B;
  }
  .red{
    color: #FF3E3E;
  }
</style>
