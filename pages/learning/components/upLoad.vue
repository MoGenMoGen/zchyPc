<template>
<div id="bg" v-show="show">
  <div class="upload">
    <div class="header">
      <p>
        上传（课件或知识包）
        <img :src="close" @click="toClose" class="point">
      </p>
    </div>
    <div class="content">
      <el-form :label-position="labelPosition" label-width="100px" :model="form">
        <el-form-item label="课程名称：">
          <el-input v-model="form.nm" placeholder="请输入课程名称"></el-input>
        </el-form-item>
        <el-form-item label="课程简介：">
          <el-input type="textarea" v-model="form.cont" id="long" placeholder="课程简介"></el-input>
        </el-form-item>
        <el-form-item label="课程摘要：">
          <el-input type="textarea" v-model="form.summary" placeholder="课程摘要"></el-input>
        </el-form-item>
        <el-form-item label="所属分类：">
          <el-select v-model="form.cid" class="eqiupName" >
            <el-option
              v-for="item in list"
              :key="item.id"
              :label="item.nm"
              :value="item.id">
            </el-option>
          </el-select>
        </el-form-item>
        <el-form-item label="课程封面：">
          <div class="img">
            <div class="uploadImg" :style="{height:imgBoxHeight+'px'}">
              <div>
                <p :style="{lineHeight:(imgBoxHeight-15)+'px'}">+</p>
                <input ref="upload" type="file" name="file"   multiple="multiple" @change="upImg($event,2)"/>
              </div>
            </div>
            <div v-for="(item,index) in imgList2" :key="index" :style="{height:imgBoxHeight+'px'}">
              <img :src="del" class="delImg" @click="deleteImg(index,2)"/>
              <img :src="item.imgUrl" @click="toLink(item)"/>
            </div>
          </div>
        </el-form-item>
      </el-form>
      <div>
        <p>上传附件：</p>
        <div class="imgBox">
          <div class="img">
            <div class="uploadImg" :style="{height:imgBoxHeight+'px'}">
              <div>
                <p :style="{lineHeight:(imgBoxHeight-15)+'px'}">+</p>
                <input ref="upload" type="file" name="file"   multiple="multiple" @change="upImg($event,1)"/>
              </div>
            </div>
            <div v-for="(item,index) in imgList" :key="index" :style="{height:imgBoxHeight+'px'}">
              <img :src="del" class="delImg" @click="deleteImg(index,1)"/>
              <img :src="item.imgUrl" @click="toLink(item)"/>
            </div>
          </div>
          <p :style="{color: '#FF4545'}">* 上传格式： 限制文档、PPT、图片</p>
          <div class="run">
            <button @click="toClose">取消</button>
            <button @click="submit">确认</button>
          </div>
        </div>
      </div>
    </div>
  </div>
</div>
</template>

<script>
  import close from "@/assets/img/close.png";
  import del from '@/assets/img/personal/del.png';
  import excel from '@/assets/img/personal/excel.png'
  import ppt from '@/assets/img/personal/ppt.png'
  import word from '@/assets/img/personal/word.png'
  import pdf from '@/assets/img/personal/pdf.jpg'
  export default {
        name: "upLoad",
      data(){
            return{
              close,
              excel,
              ppt,
              word,
              pdf,
              del,
              labelPosition: 'left',
              form: {
                cid: '',
                cids: "",
                nm: "",
                summary: "",
                teacherId: "",
                teacher: "",
                icon: "",
                coverUrl: "",
                origNm: "",
                size: "",
                type: "1",
                url: "",
                cont: "",
                rmks: "备注"
              },
              imgList:[],
              imgList2:[],
              list:[],
              imgBoxHeight:'85',
            }
      },
    props: {
      show: {
        type: Boolean,
        default: false
      },
    },
    watch:{

    },
    mounted(){
      this.getList();
    },
    methods:{
      toClose(){
        this.$emit('close')
      },

      getHeight(){
        this.imgBoxHeight=((this.width*0.813)-60)*0.5*0.59*0.3;
        console.log('高度'+this.imgBoxHeight)
      },
      deleteImg(index,nm){
        if(nm == 1){
          this.imgList.splice(index,1)
        }
        if(nm == 2){
          this.imgList2.splice(index,1)

        }
      },
      async getList(){
        let qry = this.query.new()
        let data = await this.api.studyContCat(this.query.toEncode(qry))
        this.list = data.data.list
      },
      //上传图片
      async upImg(e,nm){
        if(e.target.files.length===0){
          return
        }
        if(nm == 1){
          let img = await this.api.uploadImg(e)

          let type=img.split('.')[img.split('.').length - 1]
          console.log(type)
          if(type=='pdf'){
            this.imgList.push({url:img,imgUrl:this.pdf})
          }else if(type=='doc'||type=='docx'){
            this.imgList.push({url:img,imgUrl:this.word})
          }else if(type=='ppt'||type=='pptx'){
            this.imgList.push({url:img,imgUrl:this.ppt})
          }else if(type=='xls'||type=='xlsx'){
            this.imgList.push({url:img,imgUrl:this.excel})
          }else{
            this.imgList.push({url:img,imgUrl:img})
          }
          console.log('照片列表')
          console.log('this.imgList',this.imgList)
        }
        if(nm == 2){
          if(this.imgList2.length >= 1){
            this.$message({
              message: '封面图片只能上传一张',
              type: 'warning'
            });
          }else{
            let img = await this.api.uploadImg(e)
            let type=img.split('.')[img.split('.').length - 1]
            console.log(type)
            if(type=='pdf'){
              this.imgList2.push({url:img,imgUrl:this.pdf})
            }else if(type=='doc'||type=='docx'){
              this.imgList2.push({url:img,imgUrl:this.word})
            }else if(type=='ppt'||type=='pptx'){
              this.imgList2.push({url:img,imgUrl:this.ppt})
            }else if(type=='xls'||type=='xlsx'){
              this.imgList2.push({url:img,imgUrl:this.excel})
            }else{
              this.imgList2.push({url:img,imgUrl:img})
            }
            console.log('头像')
            console.log(this.imgList2)
          }

        }
      },
      submit(){
        if(!this.form.nm){
          this.$message({
            message: '请输入课程名称！',
            type: 'warning'
          });
        }else if(!this.form.cont){
          this.$message({
            message: '请输入课程简介！',
            type: 'warning'
          });
        }else if(!this.form.summary){
          this.$message({
            message: '请输入课程摘要！',
            type: 'warning'
          });
        }else if(!this.form.cid){
          this.$message({
            message: '请选择课程所属分类！',
            type: 'warning'
          });
        }else if(this.imgList2.length == 0){
          this.$message({
            message: '请上传课程封面！',
            type: 'warning'
          });
        }else if(this.imgList.length == 0){
          this.$message({
            message: '请上传附件！',
            type: 'warning'
          });
        }else{
          this.form.coverUrl = this.imgList2.imgUrl;
          this.form.cids = this.form.cid;
          let imgList=[]
          this.imgList.forEach(item=>{
            imgList.push(item.imgUrl)
          })
          this.form.url=imgList.join(",")
          this.api.learningUpload(this.form).then(res =>{
            this.$message({
              message: '上传成功！',
              type: 'success'
            });
          })
          this.$emit('submit')
        }
      },
      toLink(url){
        window.open(url)
      },
    },
    }
</script>
<style lang="less">
  #bg{
    .content{
      .el-input__inner{
        height: 52px;
        width: 384px;
        border-radius: 0;
      }
      #long{
        height: 142px;
      }
      .el-textarea__inner{
        width: 384px;
        height: 82px;
        border-radius: 0;
        resize: none;
      }
    }
  }

</style>

<style lang="less" scoped>
#bg{
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0,0,0,.5);
  z-index: 100;
  display: flex;
  justify-content: center;
  align-items: center;
  .upload{
    width: 541px;
    height: 80%;
    background-color: #FFFFFF;
    display: flex;
    flex-direction: column;
    border-radius:8px;
    .header{
      display: flex;
      justify-content: center;
      align-items: center;
      height: 77px;
      width: 100%;
      padding: 0 24px 0 30px;
      border-bottom: 1px solid #DFDFDF;
      >p{
        color: #333333;
        font-size: 18px;
        width: 100%;
        text-align: center;
        position: relative;
        line-height: 40px;
        .point{
            width: 20px;
            height: 20px;
            position: absolute;
            right: 5px;
            top: 0;
            cursor: pointer;
        }
      }
    }
    .content{
      padding: 30px;
      flex: 1;
      overflow-y: scroll;
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
      >div{
        display: flex;
        margin-bottom: 28px;
        >p{
          margin-top: 20px;
          margin-right: 30px;
        }
        .imgBox{
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
  }
}
  .run{
    margin-top: 60px;
    button:nth-child(1){
      width: 169px;
      height: 52px;
      border: 1px solid #DDDDDD;
      background-color: #FFFFFF;
      color: #666666;
      font-size: 16px;
    }
    button:nth-child(2){
      width: 169px;
      height: 52px;
      background-color: #2778BE;
      color: #FFFFFF;
      font-size: 16px;
      border: none;
    }
  }
</style>
