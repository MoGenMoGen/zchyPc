<template>
<!--合同详情-->
<div id="home">
  <div class="title">
    <p><span class="lineC"></span>合同详情</p>
    <p @click="back"><i class="el-icon-arrow-left"></i>返回</p>
  </div>
  <div class="tract" v-if="ctList">
    <p>合同编号：<span>{{ctList.contractNo}}</span></p>
    <p>分期付款：<span>{{ctList.installment}}</span></p>
    <p>船舶名称：<span>{{ctList.nm}}</span></p>
    <p>已付金额：<span>{{fmoney(ctList.paid)}}</span></p>
    <p>签约日期：<span>{{ctList.signTm}}</span></p>
    <p>未付金额：<span>{{fmoney(ctList.totalPrice-ctList.paid)}}</span></p>
    <p>交付日期：<span>{{ctList.deliveryTm}}</span></p>
    <p>乙方公司：<span>{{ctList.partybNm}}</span></p>
    <p>合同总额：<span class="textRed">￥{{fmoney(ctList.totalPrice)}}</span></p>
  </div>
  <div class="ctDetail" v-for="(item,index) in list" :key="index">
    <div class="ct1">
      <p>{{item.nmI}}</p>
      <img :src="item.img" alt="" />
      <p>{{item.fileNm}}</p>
    </div>
    <button @click="download(item.url)">下载</button>
  </div>
  <div class="ctDetail" v-for="(item,index) in billList" :key="index">
    <div class="ct1">
      <p>{{item.nmI}}</p>
      <img :src="item.img" v-if="item.type==1"/>
      <img :src="item.img" v-if="item.type==2" v-viewer>
      <p>{{item.fileNm}}</p>
    </div>
    <button @click="download(item.url)">下载</button>
  </div>
</div>
</template>

<script>
    import {mapState} from "vuex";
    import excel from '@/assets/img/personal/excel.png'
    import ppt from '@/assets/img/personal/ppt.png'
    import word from '@/assets/img/personal/word.png'
    import pdf from '@/assets/img/personal/pdf.jpg'
    import other from '@/assets/img/personal/other.png'
    export default {
        name: "contractDetail",
      layout:'person',
      components: { },
      data(){
        return{
          excel,
          ppt,
          word,
          pdf,
          other,
          ctList: {},
          list:[],
          billList: [], //发票列表
          id:'',
        }
      },
      computed:{
        ...mapState([
          'bWidth',
          'width'
        ])
      },
      mounted() {
       this.id= this.until.getQueryString('id')
       this.getInfo()
      },
      methods:{
        fmoney(s, n) {
            n = n > 0 && n <= 20 ? n : 2;
            s = parseFloat((s + "").replace(/[^\d\.-]/g, "")).toFixed(n) + "";
            var l = s.split(".")[0].split("").reverse(), r = s.split(".")[1];
            var t = "";
            for (let i = 0; i < l.length; i++) {
                t += l[i] + ((i + 1) % 3 == 0 && (i + 1) != l.length ? "," : "");
            }
            return t.split("").reverse().join("") + "." + r;
        },
        async getInfo(){
          this.ctList=await this.api.contractDetail(this.id)
          this.list=this.ctList.attachment.split(',')
          if(this.ctList.invoice) {
            this.billList = this.ctList.invoice.split(',')
          }
          let imgList=[]
          this.list.forEach((item,index)=>{
            let type=item.split('.')[item.split('.').length - 1]
            let nmList=item.split('.com/')  //分割出url后的内容
            let nm=""
            nmList.forEach((v,k)=>{       //防止文件名中有 .com/ 所以循环加入
              if(k!=0){
                nm+=v
              }
            })
            nmList=nm.split('_')        //分割随机字符后的内容
            nm=""
            nmList.forEach((v,k)=>{   //防止文件名中有 - 所以循环
              if(k!=0){
                nm+=v
              }
            })
            nm=nm.split('.'+type)[0]
            console.log(nm)
            if(type=='pdf'){
            	imgList.push({url:item,img:this.pdf,'nmI':'合同附件'+(index+1),'fileNm':nm})
            }else if(type=='doc'||type=='docx'){
            	imgList.push({url:item,img:this.word,'nmI':'合同附件'+(index+1),'fileNm':nm})
            }else if(type=='ppt'||type=='pptx'){
            	imgList.push({url:item,img:this.ppt,'nmI':'合同附件'+(index+1),'fileNm':nm})
            }else if(type=='xls'||type=='xlsx'){
            	imgList.push({url:item,img:this.excel,'nmI':'合同附件'+(index+1),'fileNm':nm})
            }else{
            	imgList.push({url:item,img:item,'nmI':'合同附件'+(index+1),'fileNm':nm})
            }
          })
          this.list=imgList
          let imgList2 = []
          this.billList.forEach((item,index)=>{
            let type=item.split('.')[item.split('.').length - 1]
            let nmList=item.split('.com/')  //分割出url后的内容
            let nm=""
            nmList.forEach((v,k)=>{       //防止文件名中有 .com/ 所以循环加入
              if(k!=0){
                nm+=v
              }
            })
            nmList=nm.split('_')        //分割随机字符后的内容
            nm=""
            nmList.forEach((v,k)=>{   //防止文件名中有 - 所以循环
              if(k!=0){
                nm+=v
              }
            })
            nm=nm.split('.'+type)[0]
            console.log(nm)
            if(type=='pdf'){
            	imgList2.push({url:item,img:this.pdf,'nmI':'发票附件'+(index+1),'fileNm':nm,type:1})
            }else if(type=='png'||type=='jpg'||type=='jpeg'||type=='PNG'||type=='JPG'||type=='JPEG'){
            	imgList2.push({url:item,img:item,'nmI':'发票附件'+(index+1),'fileNm':nm,type:2})
            }else{
            	imgList2.push({url:item,img:this.other,'nmI':'发票附件'+(index+1),'fileNm':nm,type:1})
            }
          })
          this.billList=imgList2
          console.log(this.list,this.billList)
        },
        download(url){
          window.open(url)
        },
        back(){
          this.$router.go(-1)
        }
      }
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
  .tract{
    display: flex;
    flex-wrap: wrap;
    padding: 30px 0;
    border-bottom: 1px solid #F3F3F3;
    >p{
      width: 50%;
      padding-left: 37px;
      line-height: 34px;
      color: #333333;
      font-size: 15px;
    }
    .textRed{
      color: #FF3E3E;
    }
  }
  .ctDetail{
    width: 100%;
    display: flex;
    padding: 30px 34px;
    align-items: center;
    justify-content: space-between;
    border-bottom: 1px solid #F3F3F3;
    .ct1{
      display: flex;
      align-items: center;
      width: 100%;
      p:nth-child(1){
        font-size: 15px;
        color: #333333;
        // cursor: pointer;
      }
      p:nth-child(2){
        font-size: 14px;
        color: #333333;
        // cursor: pointer;
      }
      img{
        vertical-align: middle;
        width: 85px;
        height: 44px;
        padding-right: 20px;
        padding-left: 25px;
        // cursor: pointer;
      }
      .ctDetail-img {
        vertical-align: middle;
        width: 85px;
        height: 44px;
        padding-right: 20px;
        padding-left: 25px;
        cursor: pointer;
      }
    }
    >button{
      background-color: #2778BE;
      border-radius: 3px;
      color: #FFFFFF;
      width: 79px;
      height: 32px;
      border: 0;
      float: right;
      cursor: pointer;
    }
  }
</style>
