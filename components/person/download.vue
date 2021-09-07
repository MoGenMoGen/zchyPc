<template>
  <!--投标报价弹窗-->
  <div id="offer" v-show="download1">
    <div class="body">
      <div class="title">
        <p>下载附件</p>
        <img @click="toClose" src="@/assets/img/close.png"/>
      </div>
      <div class="foot">
          <div v-for="(item,index) in fileList" :key="index" class="fileList">
            <p><img :src="item.imgUrl"/>{{index+1}}.{{item.nm}}</p>
          </div>
        <div class="button">
          <button @click="toClose">取消</button>
          <button @click="submit">立即下载</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
  import {mapState} from "vuex";
  import delImg from '@/assets/img/personal/del.png'
  import excel from '@/assets/img/personal/excel.png'
  import ppt from '@/assets/img/personal/ppt.png'
  import word from '@/assets/img/personal/word.png'
  import pdf from '@/assets/img/personal/pdf.jpg'
  import del from '@/assets/img/personal/del.png'

  export default {
    name: "download",
    data(){
      return{
        delImg,
        excel,
        ppt,
        word,
        pdf,
        del,
        fileList:[],
      }
    },
    props: {
      download1: {
        type: Boolean,
        default: false
      },
      applyInfo: {
        type: Object,
      }
    },
    computed:{
      ...mapState([
        'bWidth',
        'width',
        "currentRole"
      ])
    },
    watch:{
      //截取文件名称加后缀
      applyInfo(){
        this.fileList=[]
        if(this.applyInfo){
          let list = this.applyInfo.attachment.split(',')
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
        console.log(this.fileList)//添加后的数组

      },
    },
    created(){


    },
    mounted(){
      // console.log(this.applyInfo,1)
      console.log(this.applyInfo,1)


    },
    methods:{
      toClose() {
        this.$emit('toClose')
      },
      submit(){
        console.log(this.applyInfo)
        let url = `/general/oss/aliDownload?urls=${this.applyInfo.attachment}&zipName=${this.applyInfo.nm}`
        window.open(url)
      },

    },
  }
</script>

<style lang="less">

</style>
<style lang="less" scoped>
  @import url("../../assets/css/init.less");
  #offer{
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: rgba(0,0,0,.5);
    z-index: 10;
    .body{
      background: #fff;
      border-radius: 8px;
      /*flex: 1;*/
      /*overflow-y: scroll;*/
      position: fixed;
      top: 50%;
      left: 50%;
      transform: translate(-50%,-50%);
      max-height: 597px;
      padding-bottom: 20px;
      width: 440px;
      .title{
        height: 77px;
        display: flex;
        align-items: center;
        justify-content: center;
        position: relative;
        border-bottom: 1px solid #DFDFDF;
        >p{
          color: #333333;
          font-size: 18px;
        }
        >img{
          width: 20px;
          position: absolute;
          top: 19px;
          right: 21px;
          cursor: pointer;
        }
      }
    }
    .foot{
      .form{
        padding: 0 38px;
      }
      .button{
        display: flex;
        padding: 0 27px;
        justify-content: space-between;
        margin-top: 50px;
        button:nth-child(1){
          width: 138px;
          height: 52px;
          background-color: #FFFFFF;
          border: 1px solid #DDDDDD;
          font-size: 16px;
          color: #666;
          cursor: pointer;
        }
        button:nth-child(2){
          width: 238px;
          height: 52px;
          background-color: #2778BE;
          font-size: 16px;
          border: 0;
          color: #FFF;
          cursor: pointer;
        }
      }
    }
  }
  .fileList{
    width: 100%;
    display: flex;
    flex-direction: column;
    padding-top: 30px;
    padding-left: 30px;
    >p{
      color: #333333;
      height: 25px;
      line-height: 25px;
      font-size: 15px;
      >img{
        width: 16px;
        height: 18px;
        vertical-align: sub;
        margin-right: 10px;
      }
    }
  }
</style>
