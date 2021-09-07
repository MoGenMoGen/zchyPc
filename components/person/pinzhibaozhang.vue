<template>
<!--  品质保障-->
  <div>

<!--    船舶信息-->
    <div class="info" v-if="!ifProDetail">
      <img :src="info.logo">
      <div>
        <p>船舶编号:<span>{{shipCd}}</span></p>
        <p>监理方:<span>{{info.company}}</span></p>
        <p>船舶状态:<span style="color: #2778BE">{{shipStatus}}</span></p>
      </div>
    </div>
    <div class="jianzaoBox">
      <div class="topBox">
        <p v-for="(item,index) in options" :key="index" @click="checkIndex=index" :class="{clickP:checkIndex==index}">
          {{item.nm}}

        </p>
      </div>
      <!-- 不是照片 -->
      <div class="bottomBox" v-if="this.options[this.checkIndex] && this.options[this.checkIndex].cd!='DOCS_SURVEY_CYCLE.70'">
        <div v-for="(item,index) in list" :key="index">
          <div class="title" @click="item.show=!item.show">

            <span>{{item.title}}（ 时间 : {{item.actDt}} ）</span>
            <p>
              <span style="color: #2778BE;font-weight: bold">点击查看内容</span>
              <img :src="arrowDown" v-if="item.show">
              <img :src="arrowUp" v-else>
            </p>
          </div>



          <div class="rich" v-if="item.show">
            <p style="margin-bottom: 30px;" v-show="item.fileList">期报：</p>
            <div class="doc">
              <p v-for="j in item.fileList" @click="toLink(j.url)">
                <img :src="j.img">
                <span>{{j.fileNm}}</span>
              </p>
            </div>
            <!--<p class="desc" v-show="item.imgList">-->
              <!--{{item.nm}}报告：-->
            <!--</p>-->
            <div class="report" v-viewer>
              <p v-for="j in item.imgList">
                <img :src="j.img">
              </p>
            </div>

            <p style="margin-top: 20px" v-show="item.attachmentList">附件：</p>
            <div class="doc">
              <p v-for="j in item.attachmentList" @click="toLink(j.url)">
                <img :src="j.img">
                <span>{{j.fileNm}}</span>
              </p>
            </div>
          </div>

        </div>
      </div>
      <!-- 照片 -->
      <div class="bottomBox" v-if="this.options[this.checkIndex] && this.options[this.checkIndex].cd=='DOCS_SURVEY_CYCLE.70'">
        <div v-for="(item,index) in list" :key="index">
          <div class="title" @click="item.show=!item.show">

            <span>{{item.title}}</span>
            <p>
              <span style="color: #2778BE;font-weight: bold">点击查看内容</span>
              <img :src="arrowDown" v-if="item.show">
              <img :src="arrowUp" v-else>
            </p>
          </div>



          <div class="rich" v-if="item.show">
            <!-- <p v-if="item.description" style="margin-bottom: 30px;">详细描述:{{item.description}}</p> -->
            <p class="desc" v-show="item.imgList1">
              时间 : {{item.actDt}}
            </p>
            <div class="report" v-viewer>
              <p v-for="j in item.imgList1">
                <img :src="j">
              </p>
            </div>
          </div>

        </div>
      </div>
    </div>


  </div>

</template>

<script>
    import test1 from '../../assets/img/personal/test1.png'
    import arrowUp from '../../components/img/arrowUp.png'
    import arrowDown from '../../components/img/arrowDown.png'
    import excel from '@/assets/img/personal/excel.png'
    import ppt from '@/assets/img/personal/ppt.png'
    import word from '@/assets/img/personal/word.png'
    import pdf from '@/assets/img/personal/pdf.jpg'

    export default {
        name: "fangansheji",
        data(){
            return{
                test1,
                excel,
                ppt,
                word,
                pdf,
                arrowDown,
                arrowUp,
                shipCd:"",
                shipStatus:"",
                list:[],
                info:{},
                currentRole:{},
                options:[], //分类
                checkIndex:0,
              ifProDetail:false,//是否商品详情
            }
        },
        watch:{
          checkIndex(){
            this.getInfo()
          },
        },
        async mounted() {
          this.ifProDetail = window.location.href.indexOf('/Trade/productDetail')===-1 ? false : true
          this.getDic()
          console.log(this.ifProDetail)
          if(!this.ifProDetail){

            this.id= this.until.getQueryString('id')
            this.shipCd=this.until.getQueryString('shipCd')
            this.shipStatus=this.until.getQueryString('shipStatus')
            let param={
              docsId:this.id,
              identityCd:'identity40'
            }
            this.info=await this.api.orgInfoBasic(param)
          }

        },
        methods:{
          async getDic(){
            this.options=await this.api.dataDictionary('DOCS_SURVEY_CYCLE')
            // console.log('获取数据字典')
            // console.log(this.options)
            // data.forEach(item=>{
            //   this.options.push({label:item.nm,value:item.cd})
            // })

            this.getInfo()
          },
          async getInfo(id){
            // console.log('品质保障')
            // this.currentRole=JSON.parse(this.until.seGet('currentRole'))
            this.list=[]
            let qry = this.query.new()
            if(id){
              this.id=id
              this.query.toW(qry,'docsId',id,'EQ')
            }else {
              this.query.toW(qry,'docsId',this.id,'EQ')
            }

            this.query.toW(qry,'cd',this.options[this.checkIndex].cd,'EQ')
            let data=await this.api.qualityList(this.query.toEncode(qry))
            let data1=[]
            if(data.length>0){

              data.forEach((item,index)=>{
                if(index==0){
                  item.show=true
                }else{
                  item.show=false
                }

                if(this.options[this.checkIndex].cd!='DOCS_SURVEY_CYCLE.70'){
                  let imgList1=item.imgUrl ? item.imgUrl.split(',') : []
                  let imgList2=[]
                  imgList1.forEach(v=>{
                    imgList2.push({url:v,img:v})
                  })
                  item.imgList=imgList2

                  let fileList1=item.fileUrl?item.fileUrl.split(','):[]
                  let fileList2=[]
                  let fileList11=item.attachment?item.attachment.split(','):[]
                  let fileList22=[]
                  fileList1.forEach(v=>{
                    let type=v.split('.')[v.split('.').length - 1]
                    let nmList=v.split('.com/')  //分割出url后的内容
                    let nm=""
                    nmList.forEach((j,z)=>{       //防止文件名中有 .com/ 所以循环加入
                      if(z!=0){
                        nm+=j
                      }
                    })
                    nmList=nm.split('_')        //分割随机字符后的内容
                    nm=""
                    nmList.forEach((j,z)=>{   //防止文件名中有 _ 所以循环
                      if(z!=0){
                        nm+=j
                      }
                    })
                    nm=nm.split('.'+type)[0]
                    if(type=='pdf'){
                    	fileList2.push({url:v,img:this.pdf,'fileNm':nm})
                    }else if(type=='doc'||type=='docx'){
                    	fileList2.push({url:v,img:this.word,'fileNm':nm})
                    }else if(type=='ppt'||type=='pptx'){
                    	fileList2.push({url:v,img:this.ppt,'fileNm':nm})
                    }else if(type=='xls'||type=='xlsx'){
                    	fileList2.push({url:v,img:this.excel,'fileNm':nm})
                    }else{
                    	fileList2.push({url:v,img:v,'fileNm':nm})
                    }
                    item.fileList=fileList2
                  })
                  fileList11.forEach(v=>{
                    let type=v.split('.')[v.split('.').length - 1]
                    let nmList=v.split('.com/')  //分割出url后的内容
                    let nm=""
                    nmList.forEach((j,z)=>{       //防止文件名中有 .com/ 所以循环加入
                      if(z!=0){
                        nm+=j
                      }
                    })
                    nmList=nm.split('_')        //分割随机字符后的内容
                    nm=""
                    nmList.forEach((j,z)=>{   //防止文件名中有 _ 所以循环
                      if(z!=0){
                        nm+=j
                      }
                    })
                    nm=nm.split('.'+type)[0]
                    if(type=='pdf'){
                      fileList22.push({url:v,img:this.pdf,'fileNm':nm})
                    }else if(type=='doc'||type=='docx'){
                      fileList22.push({url:v,img:this.word,'fileNm':nm})
                    }else if(type=='ppt'||type=='pptx'){
                      fileList22.push({url:v,img:this.ppt,'fileNm':nm})
                    }else if(type=='xls'||type=='xlsx'){
                      fileList22.push({url:v,img:this.excel,'fileNm':nm})
                    }else{
                      fileList22.push({url:v,img:v,'fileNm':nm})
                    }
                    item.attachmentList=fileList22
                  })
                }else{
                  item.imgList1=item.imgUrl ? item.imgUrl.split(',') : []
                  if(data1.length==0){
                    console.log(item)
                    data1[0]=item
                  }else{
                    if(item.actDt==data1[data1.length-1].actDt){
                      data1[data1.length-1].imgList1.push(...item.imgList1)
                    }else{
                      data1.push(item)
                    }
                  }
                }
              })
            }
            if(this.options[this.checkIndex].cd!='DOCS_SURVEY_CYCLE.70'){
              this.list=data
            }else{
              this.list=data1
            }
            console.log(this.list)

          },
          toLink(url){
            console.log(url)
            window.open(url)
          },
        }


    }
</script>

<style scoped lang="less">


  .info{
    display: flex;
    // height: 150px;
    width: 100%;
    padding-bottom: 30px;
    // margin-bottom: 30px;
    border-bottom: 1px solid rgba(0, 0, 0, 0.1);
    img{
      width: 150px;
      height: 150px;
      margin-right: 19px;
    }
    div{
      padding-bottom: 5px;
      display: flex;
      flex-direction: column;
      justify-content: space-between;
    }
  }

  .jianzaoBox{
    display: flex;
    flex-direction: column;
    .topBox{
      width: 100%;
      margin-bottom: 30px;
      // background-color: #f3f3f3;
      display: flex;
      align-items: center;
      border-bottom: 1px solid rgba(0,0,0,0.1);
      overflow-x: auto;
      >p{
        font-size: 14px;
        color: #666666;
        padding: 14px 20px;
        cursor: pointer;
        white-space: nowrap;
      }
      .clickP{
        font-size: 16px;
        color: #303030;
        font-weight: 600;
        border-bottom: 2px solid #2778BE;

      }
    }
    .bottomBox{
      // flex: 4;
    }
  }

  .title{
    margin-bottom: 10px;
    width: 100%;
    height: 40px;
    background: #EEEEEE;
    color: #666666;
    padding:0 28px;
    display: flex;
    align-items: center;
    justify-content: space-between;

    &:hover{
      cursor: pointer;
    }
    p:nth-of-type(1){
      display: flex;
      align-items: center;
      img{
        width: 12px;
        margin-left: 25px;
      }
    }
  }
  .rich{

      padding: 24px 28px;
      .desc{
        color: #666666;

      }

      .report{
        display: flex;
        flex-wrap: wrap;
        width: 100%;
        p{
          width: 180px;
          height: 120px;
          margin: 10px 10px 0 0;
          img{
            display: block;
            width: 180px;
            height: 120px;
            object-fit: cover;
            margin: 0 auto;
          }
        }
      }

      .doc{
        display: flex;
        // flex-direction: column;
        flex-wrap: wrap;
        width: 100%;
        p{
          margin-top: 10px;
          width: 16.6%;
          display: flex;
          flex-direction: column;
          align-items: center;
          img{
            width: 62px;
            height: 69px;
            margin-right: 3px;
            display: block;
            cursor: pointer;
            &:hover{
            }
          }
          span{
            // display: block;
            // width: 100%;
            // text-align: center;
            color: #0066cc;
            font-size: 13px;
            cursor: pointer;
            // margin-top: 5px;
          }
        }
      }
  }
</style>
