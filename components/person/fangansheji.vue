<template>
<!--  设计流程-方案设计-->
  <div>
    <p class="title">{{title}}</p>
    <div class="designDept">
      <p v-html="info.description">
        
      </p>
     <!-- <p>
        {{info.description}}
      </p> -->
      <!-- <div class="imgBox">
        <p v-for="j in info.imgList" @click="toLink(j.url)">
          <img :src="j.img">
        </p>
      </div>
      <div class="fileBox">
        <p v-for="j in info.fileList" @click="toLink(j.url)">
          <img :src="j.img">
          <span>{{j.fileNm}}</span>
        </p>
      </div> -->
    </div>
  </div>

</template>

<script>
    import excel from '@/assets/img/personal/excel.png'
    import ppt from '@/assets/img/personal/ppt.png'
    import word from '@/assets/img/personal/word.png'
    import pdf from '@/assets/img/personal/pdf.jpg'
    export default {
        name: "fangansheji",
        props:{
          title:{
              type:String,
              default:'设计'
          },
          flowCd:{
              type:Number,
              default:1
          },
        },
        data(){
          return{
            excel,
            ppt,
            word,
            pdf,
            id:"",
            list:[],
            info:{},
          }
        },
        watch:{
          flowCd(){
            console.log('变化'+this.flowCd)
            this.getInfo()
          },
        },
        mounted() {
           this.id = this.until.getQueryString('id')
           this.getInfo()
        },
        methods: {
          async getInfo(){
            let qry = this.query.new()
            // this.currentRole=JSON.parse(this.until.seGet('currentRole'))
            this.query.toW(qry,'docsId',this.id,'EQ')
            let designCd=''
            let data=await this.api.dataDictionary('DOCS_DESIGN_STATUS')
            data.forEach((item,index)=>{
              if(index+1==this.flowCd-10){
                designCd=item.cd
              }
            })
            this.query.toW(qry,'designFlowCd',designCd,'EQ')
            this.list=await this.api.designDeptList(this.query.toEncode(qry))
            if(this.list.length>0){
              this.info=this.list[0]
              // this.info.imgList=this.info.imgUrl.split(',')
              // this.info.fileList=this.info.attachment.split(',')

              // let imgList2=[]
              // this.info.imgList.forEach(v=>{
              //   imgList2.push({url:v,img:v})
              // })
              // this.info.imgList=imgList2

              // let fileList2=[]
              // this.info.fileList.forEach(v=>{
              //   let type=v.split('.')[v.split('.').length - 1]
              //   let nmList=v.split('.com/')  //分割出url后的内容
              //   let nm=""
              //   nmList.forEach((j,z)=>{       //防止文件名中有 .com/ 所以循环加入
              //     if(z!=0){
              //       nm+=j
              //     }
              //   })
              //   nmList=nm.split('-')        //分割随机字符后的内容
              //   nm=""
              //   nmList.forEach((j,z)=>{   //防止文件名中有 - 所以循环
              //     if(z!=0){
              //       nm+=j
              //     }
              //   })
              //   nm=nm.split('.'+type)[0]
              //   if(type=='pdf'){
              //   	fileList2.push({url:v,img:this.pdf,'fileNm':nm})
              //   }else if(type=='doc'||type=='docx'){
              //   	fileList2.push({url:v,img:this.word,'fileNm':nm})
              //   }else if(type=='ppt'||type=='pptx'){
              //   	fileList2.push({url:v,img:this.ppt,'fileNm':nm})
              //   }else if(type=='xls'||type=='xlsx'){
              //   	fileList2.push({url:v,img:this.excel,'fileNm':nm})
              //   }else{
              //   	fileList2.push({url:v,img:v,'fileNm':nm})
              //   }
              //   this.info.fileList=fileList2
              // })

            }else{
              this.info={}
            }
            console.log(this.info)
          },
          toLink(url){
            window.open(url)
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
  .designDept{
    padding: 24px 28px;
    .imgBox{
      >p{
        padding: 10px 20%;
      }
    }
    .fileBox{
      display: flex;
      flex-wrap: wrap;
      >p{
        display: flex;
        flex-direction: column;
        width: 75px;
        align-items: center;
        margin: 0 20px 15px 0;
        >img{
          height: 70px;
          width: 60px;
          margin-bottom: 10px;
        }
      }
    }
  }
</style>
