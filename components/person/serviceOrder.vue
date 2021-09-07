<template>
  <!--维保服务单-->
  <div id="offer" v-show="show">
    <div class="body">
      <div class="title">
        <p>维保服务单</p>
        <img @click="Close" src="@/assets/img/close.png"/>
      </div>
      <div class="foot">
        <div>
          <p><span>联系人姓名：</span>{{info.linkman}}</p>
          <p><span>联系电话：</span>{{info.mob}}</p>
          <p><span>公司名称：</span>{{info.company}}</p>
        </div>
        <div>
          <p><span>服务地点：</span>{{info.addr}}</p>
          <p><span>维保船舶：</span>{{info.docsNm}}</p>
          <!--<p><span>一级类别：</span>{{info.type}}</p>-->
          <p><span>产品名称：</span>{{info.deviceNms}}</p>
          <p><span>服务内容：</span>{{info.serviceItemNm}}</p>
          <p><span>服务内容说明：</span>{{info.serviceCont}}</p>
          <p><span>预约时间：</span>{{info.appointmentTm}} {{info.dateSlice}}</p>
        </div>

        <div>
          <span>维修服务内容：</span>
          <textarea v-model="form.serviceContProvider"></textarea>
          <div class="imgBox">
            <div class="img">
              <div class="uploadImg" :style="{height:imgBoxHeight+'px'}">
                <div>
                  <p :style="{lineHeight:(imgBoxHeight-15)+'px'}">+</p>
                  <input ref="upload" type="file" name="file"   multiple="multiple" @change="upImg($event)"/>
                </div>
              </div>
              <div v-for="(item,index) in imgList" :key="index" :style="{height:imgBoxHeight+'px'}">
                <img :src="del" class="delImg" @click="deleteImg(index)"/>
                <img :src="item" @click="toLink(item)"/>
              </div>
            </div>
          </div>
        </div>

        <p><span>负责人员：</span>{{info.handleRealNm}}</p>
      </div>
      <div class="button">
        <button @click="Close">取消</button>
        <button @click="submit">确认</button>
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
    name: "serviceOrder",
    data(){
      return{
        delImg,
        excel,
        ppt,
        word,
        pdf,
        del,
        imgBoxHeight:'85',

        imgList:[],
        form:{
          id: '',
          serviceContProvider: "",
          receiptsImg: "",
          types:'',
        },
        info:{}
      }
    },
    props: {
      show: {
        type: Boolean,
        default: false
      },
      id:{
        type:Number,
        default:null
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
      id:{
        handler:function(){
          if(this.id){
            this.getInfo()
          }
        },
        immediate:true,
      }
    },
    created(){


    },
    mounted(){


    },
    methods:{
      async getInfo(){
         this.info = await this.api.taskNoteDetail(this.id)
      },
      Close() {
        this.$emit('close')
      },
      deleteImg(index){
          this.imgList.splice(index,1)
      },
      //上传图片
      async upImg(e){
        if(e.target.files.length===0){
          return
        }
          let img = await this.api.uploadImg(e)
          let type=img.split('.')[img.split('.').length - 1]
          console.log(type)
        this.imgList.push(img)
          // if(type=='pdf'){
          //   this.imgList.push({url:img,imgUrl:this.pdf})
          // }else if(type=='doc'||type=='docx'){
          //   this.imgList.push({url:img,imgUrl:this.word})
          // }else if(type=='ppt'||type=='pptx'){
          //   this.imgList.push({url:img,imgUrl:this.ppt})
          // }else if(type=='xls'||type=='xlsx'){
          //   this.imgList.push({url:img,imgUrl:this.excel})
          // }else{
          //   this.imgList.push({url:img,imgUrl:img})
          // }
          console.log('照片列表')
          console.log('this.imgList',this.imgList)

      },
      submit(){
        this.store.commit('changeLoading',true)
        this.form.id = this.id
        this.form.types = this.info.types
        this.form.receiptsImg = this.imgList.join(',')
        console.log(this.form)

        // return

        this.api.taskNoteFinish(this.form).then(()=>{
          this.$message({
            message: '操作成功',
            type: 'success'
          });
          this.$emit('confirm')
        })

      }

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
      position: fixed;
      top: 50%;
      left: 50%;
      transform: translate(-50%,-50%);
      /*max-height: 597px;*/
      padding-bottom: 20px;
      width: 660px;
      height: 80%;
      display: flex;
      display: -webkit-flex;
      flex-direction: column;
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
      padding: 0 35px;
      flex: 1;
      overflow-y: scroll;
      >div:nth-child(1){
        padding-top: 20px;
        display: flex;
        flex-wrap: wrap;
        >p{
          width: 50%;
          height: 30px;
          line-height: 30px;
        }
      }
      >div:nth-child(2){
        padding-bottom: 15px;
        >p{
          line-height: 30px;
        }
        border-bottom: 1px solid #DFDFDF;
      }
      >div:nth-child(3){
        padding-top: 20px;
        >span{
          float: left;
          line-height: 30px;
          height: 100px;

        }
        >textarea{
          float: right;
          width: 80%;
          height: 100px;
          margin-bottom: 10px;
          padding: 10px;
          box-sizing: border-box;
        }
        .imgBox{
          margin-left: 20%;
          .img{
            width: 100%;
            >div{
              width: 85px;
              height: 85px;
              margin-right: 3%;
              margin-bottom: 10px;
              float: left;
              position: relative;
              img{
                width: 100%;
                height: 100%;
              }
              .delImg{
                position: absolute;
                top: -7.5px;
                right: -7.5px;
                z-index: 200;
                width: 15px;
                height: 15px;
              }
            }
            .uploadImg{
              display: flex;
              display: -webkit-flex;
              align-items: center;
              // width: 85px;
              >p{
                color: #333333;
                font-size: 15px;
                float: left;
                line-height: 85px;
                margin-left: 3%;
              }
              >div{
                display: flex;
                display: -webkit-flex;
                color: #C6C6C6;
                width: 100%;
                height: 100%;
                p{
                  width: 100%;
                  text-align: center;
                  border: 1px dashed #C6C6C6;
                  font-size: 50px;
                }
                input{
                  width: 100%;
                  height: 100%;
                  opacity: 0;
                  position: absolute;
                  left: 0;
                  top: 0;
                }
              }
            }
          }
          .img::after{
            content: "";
            display: block;
            clear: both;
          }
        }
      }
    }
    .button{
      /*padding-left: 20%;*/
      display: flex;
      padding: 0 27px 0 22%;
      margin-top: 30px;
      button:nth-child(1){
        width: 179px;
        height: 52px;
        background-color: #FFFFFF;
        border: 1px solid #DDDDDD;
        font-size: 16px;
        color: #666;
        cursor: pointer;
        margin-right: 20px;
      }
      button:nth-child(2){
        width: 285px;
        height: 52px;
        background-color: #2778BE;
        font-size: 16px;
        border: 0;
        color: #FFF;
        cursor: pointer;
      }
    }
  }

</style>
