<template>
<!--船舶档案详情——————设计档案-->
  <div class="mainDetail">
    <div class="nav">
      <p v-for="item in options" @click="choose(item)" :class="{clickP:statusLV.value==item.value}" class="point">
        {{item.label}}
      </p>
    </div>
    <div class="main">
      <div v-if="statusLV.value==0"><!-- 设计院简介 -->
        <design-info></design-info>
      </div>

      <div v-else>                  <!-- 方案设计、详细设计、生产设计 -->
        <p class="title">{{statusLV.lable}}</p>
        <div class="content">
					<div style="width: 100%;">
					  <span>内容及附件 :</span>
					  <vue-ueditor-wrap v-model="form.description" style="z-index: 99;width: 87%"  :config="myConfig"></vue-ueditor-wrap>
					</div>
          <button @click="toSubmit">确认提交</button>


          <!-- <div class="left">
            <p class="left-title">{{statusLV.lable}}说明：</p>
            <p class="left-msg">
              <textarea cols="30" rows="10"  v-model="form.description" :placeholder="'为'+statusLV.lable+'做准备'"></textarea>
              <button @click="toSubmit">确认提交</button>
            </p>
          </div>
          <div class="right">
            <div>
              <p>完成时间：</p>
              <el-date-picker
                v-model="form.completeTm"
                type="datetime"
                placeholder="选择日期时间">
              </el-date-picker>
            </div>
            <div>
              <p>上传图片：</p>
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
                    <img :src="item.imgUrl" @click="toLink(item)"/>
                  </div>
                </div>
              </div>
            </div>
            <div>
              <p>上传附件：</p>
              <div class="imgBox">
                <div class="img">
                  <div class="uploadImg" :style="{height:imgBoxHeight+'px'}">
                    <div>
                    	<p  :style="{lineHeight:(imgBoxHeight-15)+'px'}">+</p>
                  <input ref="upload" type="file" name="file"   multiple="multiple" @change="upFile($event)"/>
                    </div>
                  </div>
                  <div v-for="(item,index) in fileList" :key="index" :style="{height:imgBoxHeight+'px'}">
                    <img :src="del" class="delImg" @click="deleteFile(index)"/>
                    <img :src="item.imgUrl" @click="toLink(item)"/>
                  </div>
                </div>
              </div>
            </div>
          </div> -->
        </div>
      </div>
    </div>
  </div>
</template>

<script>
    import {mapState} from "vuex";
		import VueUeditorWrap from "vue-ueditor-wrap";
    import pageInation from "@/components/pageInation.vue"
    import designInfo from "@/components/person/baseInfo2.vue"
    import excel from '@/assets/img/personal/excel.png'
    import ppt from '@/assets/img/personal/ppt.png'
    import word from '@/assets/img/personal/word.png'
    import pdf from '@/assets/img/personal/pdf.jpg'
    import del from '@/assets/img/personal/del.png'
    export default {
      layout:'person',
      name: "designFile",
      props:[],
      components: {
        pageInation,VueUeditorWrap,designInfo
      },
      data(){
          return{
            excel,
            ppt,
            word,
            pdf,
            del,
            id:"",  //档案id
            designId:"",  //设计流程id
            statusLV:{lable:'基本资料',value:'0'},
            options: [{
                value: '0',
                label: '基本资料'
            }],
            form:{
              docsId: "",
              designFlowCd: "",
              description: "",
              // imgUrl: "",
              // attachment: "",
              designerId: "",
              designerNm: "",
              // completeTm: '',
            },
            currentRole:{},
            imgList:[],
            fileList:[],
            imgBoxHeight:'100',
            flag:0,  //判断是否已有数据 0没有调新增接口 1有掉修改接口
						myConfig: {
						  // 编辑器不自动被内容撑高
						  autoHeightEnabled: false,
						  // 初始容器高度
						  initialFrameHeight: 440,
						  // 初始容器宽度
						  initialFrameWidth: "100%",
						  // 上传文件接口（这个地址是我为了方便各位体验文件上传功能搭建的临时接口，请勿在生产环境使用！！！）
						  serverUrl: "https://www.ship88.cn//general/ueditor/exec",
						  UEDITOR_HOME_URL: '/sinovat/UEditor/'
						  // UEditor 资源文件的存放路径，如果你使用的是 vue-cli 生成的项目，通常不需要设置该选项，vue-ueditor-wrap 会自动处理常见的情况，如果需要特殊配置，参考下方的常见问题2
						  // UEDITOR_HOME_URL:
						  //   process.env.NODE_ENV === "production"
						  //     ? "/static/ueditor/"
						  //     : "/static/ueditor/"

						},
          }
      },
      computed:{
        ...mapState([
          'bWidth',
          'width'
        ])
      },
      watch:{
        $route(){
          this.cd = this.until.getQueryString('cdType')
        },
        statusLV(){
          console.log('变了')
          // console.log(this.statusLV)

        },
        width(){
          this.getHeight()
        }
      },
      mounted() {
        this.id = this.until.getQueryString('id')
        this.currentRole=JSON.parse(this.until.seGet('currentRole'))
        this.getDic()
        this.getHeight()
		console.log(11134,this.statusLV.value);
        if(this.statusLV.value!='0'){
          this.getInfo()
        }
		console.log(1112,this.form);
      },
      methods: {
        async getInfo(){
          let qry = this.query.new()
          this.form.designerId=this.currentRole.id
          this.form.designerNm=this.currentRole.company
          this.query.toW(qry,'docsId',this.id,'EQ')
          this.query.toW(qry,'designFlowCd',this.statusLV.value,'EQ')
          this.list=await this.api.designDeptListAll(this.query.toEncode(qry))
		  console.log(34,this.list);
          if(this.list.length>0){
            this.flag=1
            this.designId=this.list[0].id
            this.form.description=this.list[0].description
            // this.form.completeTm=this.list[0].completeTm
            // this.imgList=this.list[0].imgUrl.split(',')
            // this.fileList=this.list[0].attachment.split(',')

            // let imgList2=[]
            // this.imgList.forEach(v=>{
            //   imgList2.push({url:v,imgUrl:v})
            // })
            // this.imgList=imgList2

          //   let fileList2=[]
          //   this.fileList.forEach(v=>{
          //     let type=v.split('.')[v.split('.').length - 1]
          //     let nmList=v.split('.com/')  //分割出url后的内容
          //     let nm=""
          //     nmList.forEach((j,z)=>{       //防止文件名中有 .com/ 所以循环加入
          //       if(z!=0){
          //         nm+=j
          //       }
          //     })
          //     nmList=nm.split('-')        //分割随机字符后的内容
          //     nm=""
          //     nmList.forEach((j,z)=>{   //防止文件名中有 - 所以循环
          //       if(z!=0){
          //         nm+=j
          //       }
          //     })
          //     nm=nm.split('.'+type)[0]
          //     if(type=='pdf'){
          //     	fileList2.push({url:v,imgUrl:this.pdf,'fileNm':nm})
          //     }else if(type=='doc'||type=='docx'){
          //     	fileList2.push({url:v,imgUrl:this.word,'fileNm':nm})
          //     }else if(type=='ppt'||type=='pptx'){
          //     	fileList2.push({url:v,imgUrl:this.ppt,'fileNm':nm})
          //     }else if(type=='xls'||type=='xlsx'){
          //     	fileList2.push({url:v,imgUrl:this.excel,'fileNm':nm})
          //     }else{
          //     	fileList2.push({url:v,imgUrl:v,'fileNm':nm})
          //     }
          //     this.fileList=fileList2
          //   })
          }
			  else{
				this.form={}
				// this.imgList=[]
				// this.fileList=[]
				this.form.designerId=this.currentRole.id
				this.form.designerNm=this.currentRole.company
				this.flag=0
			  }
          console.log('123312',this.form)
        },
        async getDic(){
          let data=await this.api.dataDictionary('DOCS_DESIGN_STATUS')
          // console.log('获取数据字典')
          // console.log(data)
          data.forEach((item,index)=>{
            if(index!=data.length-1){
              this.options.push({label:item.nm,value:item.cd})
            }
          })
          console.log(this.options)
        },
        toSubmit(){
          this.form.docsId = this.id
          this.form.designFlowCd=this.statusLV.value
          // let fileList=[]
          // let imgList=[]
          // this.fileList.forEach(item=>{
          //   fileList.push(item.url)
          // })
          // this.imgList.forEach(item=>{
          //   imgList.push(item.url)
          // })
          // this.form.attachment=fileList.join(",")
          // this.form.imgUrl=imgList.join(",")
          // if(!this.form.completeTm){
          //   this.$message({
          //     message: '请选择完成时间',
          //     type: 'warning',
          //     duration:'1500'
          //   });
          //   return
          // }
          // let timeC=this.until.formatDate(this.form.completeTm)
          // this.form.completeTm=timeC.year+'-'+timeC.month+'-'+timeC.day+' '+timeC.hour+':'+timeC.minite+':'+timeC.second
          // console.log(this.form.completeTm)
          console.log(this.form)
          if(!this.form.description){
            this.$message({
              message: '请输入内容及附件',
              type: 'warning',
              duration:'1500'
            });
            return
          }
          console.log(this.form)
          if(this.flag==0){
            this.api.designDeptAdd(this.form).then(()=>{
              this.$message({
                message: '新增成功',
                type: 'success',
                duration:'1500'
              });
              // setTimeout(()=>{
              //   this.$router.push('../shipFile')
              // },1500)
            })
          }else if(this.flag==1){
            this.form.id= this.designId,
            this.api.designDeptUpd(this.form).then(()=>{
              this.$message({
                message: '修改成功',
                type: 'success',
                duration:'1500'
              });
              // setTimeout(()=>{
              //   this.$router.push('../shipFile')
              // },1500)
            })
          }

        },
        choose(item){//选择状态
          console.log(777,item)
		  
          this.statusLV.lable=item.label
          this.statusLV.value=item.value
          if(this.statusLV.value!='0'){
            this.getInfo()
          }
        },
        back(){//返回
			this.$router.back()
        },
        //获取高度，达到1：1
        getHeight(){
          this.imgBoxHeight=((this.width*0.813)-60)*0.5*0.59*0.3;
          console.log('高度'+this.imgBoxHeight)
        },
        //删除图片
        deleteImg(index){
           this.imgList.splice(index,1)
         },
         //上传图片
        async upImg(e){
          console.log(e)
          if(e.target.files.length===0){
            return
          }
          let img = await this.api.uploadImg(e)
          let type=img.split('.')[img.split('.').length - 1]
          console.log(type)
          if(type=='pdf'){
          	this.imgList.push({url:img,imgUrl:this.pdf,type:'1'})
          }else if(type=='doc'||type=='docx'){
          	this.imgList.push({url:img,imgUrl:this.word,type:'1'})
          }else if(type=='ppt'||type=='pptx'){
          	this.imgList.push({url:img,imgUrl:this.ppt,type:'1'})
          }else if(type=='xls'||type=='xlsx'){
          	this.imgList.push({url:img,imgUrl:this.excel,type:'1'})
          }else{
          	this.imgList.push({url:img,imgUrl:img,type:'2'})
          }
          console.log('照片列表')
          console.log(this.imgList)
        },
        //删除文件
        deleteFile(index){
           this.fileList.splice(index,1)
         },
         //上传文件
        async upFile(e){
          console.log(e)
          if(e.target.files.length===0){
            return
          }
          let img = await this.api.uploadImg(e)
          let type=img.split('.')[img.split('.').length - 1]
          console.log(type)
          if(type=='pdf'){
          	this.fileList.push({url:img,imgUrl:this.pdf,type:'1'})
          }else if(type=='doc'||type=='docx'){
          	this.fileList.push({url:img,imgUrl:this.word,type:'1'})
          }else if(type=='ppt'||type=='pptx'){
          	this.fileList.push({url:img,imgUrl:this.ppt,type:'1'})
          }else if(type=='xls'||type=='xlsx'){
          	this.fileList.push({url:img,imgUrl:this.excel,type:'1'})
          }else{
          	this.fileList.push({url:img,imgUrl:img,type:'2'})
          }
          console.log('文件列表')
          console.log(this.fileList)
        },
        toLink(url){
          window.open(url)
        },
      },
    }
</script>
<style lang="less">
  .status .el-input__inner{
          height: 36px;
          line-height: 36px;
  }
  .tableOrder{
    .el-table__header thead{
      color: #666666;
      font-size:16px;
      font-weight:400;
    }
    .el-table__header th{
      background-color: #F7F7F7;

    }
    .el-table__body tr{
      color: #333333;
      font-size:15px;
    }
    .el-table__fixed-right{
      height: 100%!important;
    }
  }
  .main .content .right{
    .el-date-editor.el-input{
      width: 59.5%;

    }
    .el-input--prefix .el-input__inner{
      height: 52px;
    }
  }
</style>
<style lang="less" scoped>
  @import url("../../assets/css/init.less");
  .point{
    cursor:pointer;
  }
  .head{
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin: 30px 0;
    width: 100%;
    padding: 0 24px 0 30px;
    font-family:Microsoft YaHei;
    font-weight:400;
    // border-bottom: 1px solid rgba(0,0,0,0.1);;
    >p{
      display: flex;
      align-items: center;
      font-size:20px;
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
      color: #666666;
    }
  }
  .mainDetail{
    width: 100%;
    font-family:Microsoft YaHei;
    font-weight:400;
    .tab{
      margin: 0 36px 0 30px;
      border-bottom: 1px solid #2778BE;
      background: #F7F7F7;
      height: 40px;
      display: flex;
      >p{
        display: flex;
        justify-content: center;
        align-items: center;
        width:14.1%;
        height: 100%;
        color: #666666;
        font-size:14px;
      }
      .clickP{
        background-color: #2778BE;
        color: #FFFFFF;
      }
    }
    .nav{
      display: flex;
      margin: 17px 36px 0px 30px;
      height: 40px;
      border-bottom:  1px solid rgba(0,0,0,0.1);
      >p{
        display: flex;
        justify-content: center;
        align-items: center;
        width:14.3%;
        height: 100%;
        color: #666666;
        font-size:14px;
      }
      .clickP{
        color: #2778BE;
        border-bottom:  2px solid #2778BE;
      }
    }
    .main{
      width: 100%;
      >div{
         width: 100%;
         padding: 30px 30px 0;
        >p{
          // margin: 30px 30px 0;
          height:40px;
          background:rgba(247,247,247,1);
          border:1px solid rgba(238, 238, 238, 1);
          // padding:14px 0 14px 28px;
          padding-left: 28px;
          font-size:14px;
          font-weight:bold;
          color:#666666;
          line-height:40px;
        }
        .content{
          // display: flex;
          padding-bottom: 30px;
          width: 100%;
          >div{
            margin-top: 25px;
            display: flex;
          }
          >button{
            width: 200px;
            height:52px;
            background:rgba(39,120,190,1);
            border: none;
            margin: 59px auto 0;
            display: block;
            font-size:16px;
            color:rgba(255,255,255,1);
            line-height:52px;
          }
          // >div{
          //   width: 50%;
          //   padding-top: 30px;
          // }
          // .left{
          //   display: flex;
          //   .left-title{
          //     padding-top: 19px;
          //     font-size:15px;
          //     color:rgba(102,102,102,1);
          //   }
          //   .left-msg{
          //     width: 59.5%;
          //     >textarea{
          //       width: 100%;
          //       height:165px;
          //       padding: 20px 21px 0;
          //       background:rgba(255,255,255,1);
          //       border:1px solid rgba(221, 221, 221, 1);
          //       resize: none;
          //     }
          //     >button{
          //       width: 100%;
          //       height:52px;
          //       background:rgba(39,120,190,1);
          //       border: none;
          //       margin-top: 59px;
          //       font-size:16px;
          //       color:rgba(255,255,255,1);
          //       line-height:52px;
          //     }
          //   }
          // }
          // .right{
          //   display: flex;
          //   flex-direction: column;
          //   >div{
          //     display: flex;
          //     justify-content: center;
          //     // align-items: center;
          //     margin-bottom: 28px;
          //     >p{
          //       margin-top: 20px;
          //       margin-right: 28px;
          //     }
          //     .imgBox{
          //       width: 59.5%;
          //       .img{
          //         //overflow: hidden;
          //         width: 100%;
          //         >div{
          //           width:30%;
          //           // width: 85px;
          //           // height: 85px;
          //           margin-right: 3%;
          //           margin-bottom: 10px;
          //           float: left;
          //           position: relative;
          //           img{
          //             width: 100%;
          //             height: 100%;
          //           }
          //           .delImg{
          //             cursor: pointer;
          //             position: absolute;
          //             top: -7.5px;
          //             right: -7.5px;
          //             z-index: 2;
          //             width: 15px;
          //             height: 15px;
          //           }
          //         }
          //         .uploadImg{
          //           display: flex;
          //           display: -webkit-flex;
          //           align-items: center;
          //           // width: 85px;
          //           >p{
          //             color: #333333;
          //             font-size: 15px;
          //             float: left;
          //             line-height: 85px;
          //             margin-left: 3%;
          //           }
          //           >div{
          //             display: flex;
          //             display: -webkit-flex;
          //             color: #C6C6C6;
          //             // width: 85px;
          //             width: 100%;
          //             // height: 85px;
          //             height: 100%;
          //             p{
          //               // width: 85px;
          //               width: 100%;
          //               text-align: center;
          //               // height: 85px;
          //               // line-height: 70px;
          //               border: 1px dashed #C6C6C6;
          //               font-size: 50px;
          //             }
          //             input{
          //               width: 100%;
          //               height: 100%;
          //               opacity: 0;
          //               position: absolute;
          //               left: 0;
          //               top: 0;
          //             }
          //           }
          //         }
          //       }
          //       .img::after{
          //         content: "";
          //         display: block;
          //         clear: both;
          //       }
          //     }
          //   }
          // }
        }
      }
    }
  }
</style>
