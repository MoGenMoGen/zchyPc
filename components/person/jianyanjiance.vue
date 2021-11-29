<template>
<!--  检验检测(查看)-->
  <div>
<!--    船舶信息-->
    <div class="info" v-if="!ifProDetail">
      <div style="display: flex;">
        <p style="margin-right: 60px;">船舶编号：<span>{{shipCd}}</span></p>
        <p>船舶状态：<span style="color: #2778BE">{{shipStatus}}</span></p>
      </div>
      <div v-for="(item,index) in info" :key="index" class="comBox">
        <img :src="item.logo" v-if="item.logo">
        <div v-else style="width:100px;height:66px;"></div>
        <p>{{item.unitNum==0?"检验机构：":"检测单位："}}<span>{{item.company}}</span></p>
      </div>
    </div>

    <div class="certificate">
      <!-- <div class="title">检验检测证书</div> -->
      <div v-for="(item,index) in list" :key="index">
        <div class="title" @click="item.show=!item.show">
          <div>
          <span style="margin-right: 30px">{{ item.nm }}</span>
          <span>证件有效期:{{ item.validUntil }}</span>
        </div>
          <p>
            <span style="color: #2778BE;font-weight: bold">点击查看内容</span>
            <img :src="arrowDown" v-if="item.show">
            <img :src="arrowUp" v-else>
          </p>
        </div>
        <div class="rich" v-if="item.show">
          <div class="imgBox" v-viewer>
            <p v-for="j in item.imgList">
              <img :src="j" >
            </p>
          </div>
          <div class="doc">
            <p v-for="j in item.fileList">
              <img :src="j.img" @click="toLink(j.url)">
              <span @click="toLink(j.url)">{{j.fileNm}}</span>
            </p>
          </div>
        </div>

      </div>


      <!-- <ul> -->
        <!-- <li v-for="(item,index) in list" :key="index"> -->
          <!-- <div class="report">
            <p v-for="j in item.imgList">
              <img :src="j.img"  v-viewer>
            </p>
          </div> -->
          <!-- <div style="display: flex;align-items: flex-end;">
            <div class="img">
              <img :src="item.imgList[0]" style="cursor: pointer;">
              <viewer :images="item.imgList" class="viewer" :style="{height:width*0.16*size+'px',width:width*0.16+'px'}">
                <img
                  v-for="(src,k) in item.imgList"
                  :src="src"
                  :key="k"
                >
              </viewer>
            </div>
            <div class="doc">
              <p v-for="j in item.fileList" @click="toLink(j.url)">
                <img :src="j.img">
              </p>
            </div>
          </div>

          <p class="zsNm">{{item.nm}}</p>
          <p class="tips">*点击图片可查看更多</p> -->
          <!-- <p>证书有效期：{{item.validUntil}}</p> -->
        <!-- </li> -->
      <!-- </ul> -->
    </div>

  </div>

</template>

<script>
  import {mapState} from "vuex";
    import test1 from '../../assets/img/personal/test1.png'
    import arrowUp from '../../components/img/arrowUp.png'
    import arrowDown from '../../components/img/arrowDown.png'
    import excel from '@/assets/img/personal/excel.png'
    import ppt from '@/assets/img/personal/ppt.png'
    import word from '@/assets/img/personal/word.png'
    import pdf from '@/assets/img/personal/pdf.jpg'

    export default {
        name: "fangansheji",

        computed:{
          ...mapState([
            'bWidth',
            'width'
          ])
        },
        data(){
            return{
                test1,
                excel,
                ppt,
                word,
                pdf,
                arrowDown,
                arrowUp,
                size:1,//证书图片的宽高比
                shipCd:"",
                shipStatus:"",
                list:[],
                info:[],
                currentRole:{},
                ifProDetail:false,//是否商城详情页
            }
        },
        async mounted() {
          this.ifProDetail = window.location.href.indexOf('/Trade/productDetail')===-1 ? false : true
          if(!this.ifProDetail){
            this.id= this.until.getQueryString('id')
            this.shipCd=this.until.getQueryString('shipCd')
            this.shipStatus=this.until.getQueryString('shipStatus')
            let param={
              docsId:this.id,
              identityCd:'identity50'
            }
            let data=await this.api.orgInfoBasic(param)
            this.info=data.list
            console.log(this.info)
            this.getInfo()

          }

        },
        methods:{
          async getInfo(id){
            console.log('检验检测')
            this.list=[]
            // this.currentRole=JSON.parse(this.until.seGet('currentRole'))
            let qry = this.query.new()
            if(id){
              this.query.toW(qry,'docsId',id,'EQ')

            }else {
              this.query.toW(qry,'docsId',this.id,'EQ')

            }
            this.query.toW(qry,'types','1','EQ')
            this.query.toP(qry,'1','999')
            // console.log('路由')
            // console.log(window.location.href.indexOf('personal')>-1)
            let data

            if(window.location.href.indexOf('personal')>-1){
              data=await this.api.jianyanList(this.query.toEncode(qry))
            }else{
              data=await this.api.certNotLoginList(this.query.toEncode(qry))
            }

            if(data.length>0){
              data.forEach((item,index)=>{
                if(index==0){
                  item.show=true
                }else{
                  item.show=false
                }
                item.imgList=item.imgUrl.split(',')

                // let imgList1=item.imgUrl.split(',')
                // let imgList2=[]
                // imgList1.forEach(v=>{
                //   imgList2.push({url:v,img:v})
                // })
                // item.imgList=imgList2

                let fileList1=item.attachment?item.attachment.split(','):[]
                let fileList2=[]
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
                item.validUntil=item.validUntil?item.validUntil.split(' ')[0]:'-'
                this.list.push(item)
                // if(item.types==1){

                //   item.imgList=item.imgUrl.split(',')
                //   this.list.push(item)
                // }
              })
            }
            // console.log('列表')
            // console.log(this.list)
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
    // justify-content: space-between;
    flex-direction: column;
    width: 100%;
    margin-bottom: 30px;


    .comBox{
      display: flex;
      align-items: center;
      margin-top: 25px;
      >img{
        width: 100px;
        height: 100%;
        margin-right: 10px;
      }
    }
  }


    .certificate{
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
        .imgBox{
          display: flex;
          flex-wrap: wrap;
          >p{
            width: 22.5%;
            // margin-right: 3%;
            &:nth-of-type(4n){
              margin-right: 0;
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
      // .title{
      //   width: 100%;
      //   background: #F7F7F7;
      //   border: 1px solid #eeeeee;
      //   height: 40px;
      //   line-height: 40px;
      //   text-indent: 28px;
      //   color: #666666;
      //   margin: 30px 0;
      // }
      ul{
        width: 100%;
        overflow: hidden;
        li{
          width: 22.75%;
          margin-right: 3%;
          // height: 150px;
          display: inline-block;
          &:nth-of-type(4n){
            margin-right: 0;
          }
          .report{
            display: flex;
            flex-wrap: wrap;
            width: 100%;
            p{
              // width: 330px;
              margin-bottom: 15px;
              img{
                display: block;
                width: 100%;
                // height: 397px;
                object-fit: cover;
                margin: 0 auto;
              }
            }

          }


          .img{
            // width: 100%;
            // height: 70%;
            flex: 3;
            display: flex;
            display: -webkit-flex;
            position: relative;
            margin-right: 15px;
            img{
              width: 100%;
              height: 100%;
              max-width: 100%;
              max-height: 100%;
              margin: auto;
            }
            .viewer{
              position: absolute;
              top: 0;
              left: 0;
              opacity: 0;
            }
          }
          .tips{
            font-size: 12px;
            color: #ff3e3e;
          }
          .zsNm{
            font-size: 14px;
            margin-top: 10px;
          }
          // p{
          //   line-height: 24px;
          // }
          // p:first-of-type{
          //   color: #666666;
          //   font-size: 14px;
          // }
          // p:last-of-type{
          //   font-size: 12px;
          //   color: #999999;
          // }
        }
      }
    }

</style>
