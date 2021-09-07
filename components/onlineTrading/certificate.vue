<template>
    <div class="certificate">
        <!-- <div class="title">船舶检验证书</div> -->
        <div v-for="(item,index) in list1" :key="index">
          <div class="title" @click="toOpen(index)">
            <span>{{item.nm}}</span>
            <p>
              <!-- <span style="color: #2778BE;font-weight: bold">完成检验</span> -->
              <img :src="arrowDown" v-if="item.show">
              <img :src="arrowUp" v-else>
            </p>
          </div>
          <div class="rich" v-show="item.show">
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

        <!-- <ul>
          <li v-for="(item,index) in list1" :key="index" :style="{width:width*0.16+'px'}"> -->
            <!-- <div class="img" :style="{height:width*0.16*size+'px'}">
              <img :src="item.imgUrl" v-viewer>
            </div> -->
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
                <p v-for="j in item.fileList" @click.stop="toLink(j.url)">
                  <img :src="j.img">
                </p>
              </div>
            </div>
            <p>{{item.nm}}</p>
            <p class="tips">*点击图片可查看更多</p> -->
            <!-- <p>证书有效期：{{item.validUntil}}</p> -->
          <!-- </li>
        </ul> -->
      <!-- <div class="title">产品证书</div>
      <ul>
        <li v-for="(item,index) in list2" :key="index" :style="{width:width*0.16+'px'}">
          <div class="img" :style="{height:width*0.16*size+'px'}">
            <img :src="item.imgUrl">
          </div>
          <p>{{item.nm}}</p>
          <p>证书有效期：{{item.validUntil}}</p>
        </li>
      </ul> -->
    </div>
</template>

<script>
  import {mapState} from "vuex";
    import arrowUp from '../../components/img/arrowUp.png'
    import arrowDown from '../../components/img/arrowDown.png'
    import excel from '@/assets/img/personal/excel.png'
    import ppt from '@/assets/img/personal/ppt.png'
    import word from '@/assets/img/personal/word.png'
    import pdf from '@/assets/img/personal/pdf.jpg'
    export default {
      props:['list'],
        name: "productList",
      data(){
          return{
            excel,
            ppt,
            word,
            pdf,
            arrowDown,
            arrowUp,
            size:1,//证书图片的宽高比
            list1:[],
            list2:[],
            certificateList:[],
          }
      },
      computed:{
        ...mapState([
          'bWidth',
          'width'
        ]),

      },
      watch:{
        // list:{
        //   handler:function () {
        //     this.list1 = []  //检验证书
        //     this.list2 = [] //产品证书
        //     this.list.forEach(item=>{
        //         if(item.types==0){
        //           this.list1.push(item)
        //         }else {
        //           this.list2.push(item)
        //         }
        //     })
        //   },
        //   deep:true
        // }
      },
      mounted() {
        this.id= this.until.getQueryString('id')
        this.getInfo()
      },
      methods:{
        toLink(url){
          console.log(url)
          window.open(url)
        },
        toOpen(index){
          console.log('hhh')
          this.list1[index].show=!this.list1[index].show
          this.$set(this.list1,index,this.list1[index])
          console.log(this.list1)
          // item.show=!item.show
        },
        async getInfo(){
          this.certificateList=[]
          let qry = this.query.new()
          // this.currentRole=JSON.parse(this.until.seGet('currentRole'))
          this.query.toW(qry,'docsId',this.id,'EQ')
          this.query.toW(qry,'types','0','EQ')
          this.query.toP(qry,'1','100')
          // console.log('路由')
          // console.log(window.location.href.indexOf('personal')>-1)
          let data
          if(window.location.href.indexOf('personal')>-1){
            this.certificateList=await this.api.jianyanList(this.query.toEncode(qry))
          }else{
            this.certificateList=await this.api.certNotLoginList(this.query.toEncode(qry))
          }

          // this.total=data.page.total

          // console.log(this.certificateList)
          this.certificateList.forEach(item=>{
            // item.validUntil=item.validUntil?item.validUntil.split(' ')[0]:"-"
            item.imgList=item.imgUrl?item.imgUrl.split(','):[]
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
            item.show=false
            this.list1.push(item)
          })
          console.log(this.list1)
        },
      }
    }
</script>

<style lang="less" scoped>
  @import url("../../assets/css/init.less");

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
            margin-right: 3%;
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
  }

</style>
