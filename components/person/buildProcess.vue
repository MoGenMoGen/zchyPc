<template>
<!--  建造流程-->
  <div style="margin-top: -30px;">
<!--    新增弹框-->
    <div class="bigBox" v-if="submitShow">
      <div class="submitBox">
        <div class="submitTitle">
          <p>{{changeIndex>-1?'修改':'新增'}}</p>
          <img :src="close" alt="" @click="submitShow=false">
        </div>
        <div class="content">
            <div>
              <p class="left-title">文件类型：</p>
              <p class="left-msg">
                <el-select v-model="form.catCd">
                  <el-option
                    v-for="item in options"
                    :key="item.cd"
                    :label="item.nm"
                    :value="item.cd">
                  </el-option>
                </el-select>
              </p>
            </div>
            <!-- <div>
              <p class="left-title">建造过程：</p>
              <p class="left-msg">
                <el-select v-model="form.cd"
                  @change="proChange">
                  <el-option
                    v-for="item in optionProcess"
                    :key="item.cd"
                    :label="item.nm"
                    :value="item.cd">
                  </el-option>
                </el-select>
              </p>
            </div> -->
            <div>
              <p class="left-title">标题：</p>
              <p class="left-msg">
                <el-input v-model="form.nm"></el-input>
              </p>
            </div>
            <div>
              <p class="left-title">完成时间：</p>
              <p class="left-msg">
                <el-date-picker
                  v-model="form.actDt"
                  type="date"
                  placeholder="选择日期时间">
                </el-date-picker>
              </p>

            </div>
            <div>
              <p class="left-title">上传图片：</p>
              <div class="imgBox">
                <div class="img">
                  <div class="uploadImg">
                    <div>
                    	<p >+</p>
                  <input ref="upload" type="file" name="file"   multiple="multiple" @change="upImg"/>
                    </div>
                  </div>
                  <div v-for="(item,index) in imgList" :key="index">
                    <img :src="del" class="delImg" @click="deleteImg(index)"/>
                    <img :src="item.imgUrl" v-viewer/>
                  </div>
                </div>
              </div>
            </div>
            <div>
              <p class="left-title">上传附件：</p>
              <div class="fileBox">
                <div class="file">
                  <div class="uploadFile">
                    <div>
                    	<button>选择文件</button>
                      <input ref="upload" type="file" name="file"   multiple="multiple" @change="upFile($event)"/>
                    </div>
                  </div>
                  <div v-for="(item,index) in fileList" :key="index" class="fileP">
                    <p @click="toLink(item.url)">{{item.url}}</p>
                    <img :src="del" class="delImg" @click="deleteFile(index)"/>
                    <!-- <img :src="item.imgUrl" v-viewer/> -->
                  </div>
                </div>
              </div>
            </div>
            <button @click="toSubmit">保存</button>
        </div>
      </div>

    </div>

    <div class="liuchengBox">
<!--      标签-->
      <div class="topBox">
        <p v-for="(item,index) in options" :key="index" @click="checkIndex=index" :class="{clickP:checkIndex==index}">
          {{item.nm}}<span>{{index==options.length-1?`(${listNo.length})`:''}}</span>
        </p>
        <p class="addBtn" @click="addNew">新增</p>
      </div>
      <!--最后一个 审核未通过-->
      <div class="bottomBox" v-if="checkIndex==options.length-1">
        <div v-for="(item,index) in listNo" :key="index">
          <div class="title" @click="item.show=!item.show">
            <span>{{item.nm}}(时间：{{item.actDt}})</span>
            <p>
              <span style="color: #2778BE;font-weight: bold">点击查看内容</span>
              <img :src="arrowDown" v-if="item.show">
              <img :src="arrowUp" v-else>
            </p>
            <p class="set">
              <span @click.stop="change(index)"><i class="el-icon-edit"></i>修改</span>
              <span @click.stop="delet(index)"><i class="el-icon-delete"></i>删除</span>
            </p>
          </div>

          <div class="rich" v-if="item.show">
            <p v-if="item.options">审核意见：{{item.options}}</p>
            <!-- <p class="desc" v-show="item.imgList">
              时间 : {{item.actDt}}
            </p> -->
            <p class="desc" v-show="item.imgList">
              报告：{{item.nm}}
            </p>
            <div class="report" v-viewer>
              <p v-for="j in item.imgList">
                <img :src="j.img">
              </p>
            </div>

            <p style="margin-top: 20px" v-show="item.fileList">附件：</p>
            <div class="doc">
              <p v-for="j in item.fileList" @click="toLink(j.url)">
                <img :src="j.img">
                <span>{{j.fileNm}}</span>
              </p>
            </div>
          </div>

        </div>
      </div>
      <!--不是最后一个(未审核通过) 并且 不是照片-->
      <div class="bottomBox" v-if="checkIndex<options.length-1 && options[checkIndex].cd!='DOCS_BUILD_SHIP_CAT.30'">
        <div v-for="(item,index) in list" :key="index">
          <div class="title">

            <span>{{item.nm}}(时间：{{item.actDt}})</span>

            <p class="cont1">

              <sapn style="margin-right: 10px" v-if="item.audit==2">审核已通过</sapn>
              <sapn style="margin-right: 10px" v-else-if="item.audit==1">待审核</sapn>
              <sapn style="margin-right: 10px" v-else>未通过</sapn>
              <span @click.stop="change2(index)" style="margin-right: 10px" v-if="item.audit!=2"><i class="el-icon-edit"></i>修改</span>
              <span @click.stop="delet2(index)" style="margin-right: 10px"  v-if="item.audit!=2"><i class="el-icon-delete"></i>删除</span>
<!--              <sapn style="margin-right: 10px" v-if="item.audit==1">待审核</sapn>-->

              <span style="color: #2778BE;font-weight: bold"  @click="item.show=!item.show">点击查看内容</span>
              <img :src="arrowDown" v-if="item.show">
              <img :src="arrowUp" v-else>
            </p>

          </div>

          <div class="rich" v-if="item.show">
            <!-- <p class="desc" v-show="item.imgList">
              时间 : {{item.actDt}}
            </p> -->
            <p class="desc" v-show="item.imgList">
              报告：{{item.nm}}
            </p>
            <div class="report" v-viewer>
              <p v-for="j in item.imgList">
                <img :src="j.img">
              </p>
            </div>

            <p style="margin-top: 20px" v-show="item.fileList">附件：</p>
            <div class="doc">
              <p v-for="j in item.fileList" @click="toLink(j.url)">
                <img :src="j.img">
                <span>{{j.fileNm}}</span>
              </p>
            </div>
          </div>

        </div>
      </div>
      <!-- 照片 -->
      <div class="bottomBox" v-if="options[checkIndex] && options[checkIndex].cd=='DOCS_BUILD_SHIP_CAT.30'">
        <div v-for="(item,index) in list" :key="index">
          <div class="title" @click="item.show=!item.show">
            <span>{{item.nm}}(时间：{{item.actDt}})</span>
            <p>
              <sapn style="margin-right: 10px" v-if="item.audit==2">审核已通过</sapn>
              <sapn style="margin-right: 10px" v-else-if="item.audit==1">待审核</sapn>
              <sapn style="margin-right: 10px" v-else>未通过</sapn>
              <span @click.stop="change3(index)" style="margin-right: 10px" v-if="item.audit!=2"><i class="el-icon-edit"></i>修改</span>
              <span @click.stop="delet2(index)" style="margin-right: 10px"  v-if="item.audit!=2"><i class="el-icon-delete"></i>删除</span>


              <span style="color: #2778BE;font-weight: bold">点击查看内容</span>
              <img :src="arrowDown" v-if="item.show">
              <img :src="arrowUp" v-else>
            </p>
          </div>

          <div class="rich" v-if="item.show">
            <!-- <p class="desc" v-show="item.imgList1">
              时间 : {{item.actDt}}
            </p> -->
            <p class="desc" v-show="item.imgList1">
              报告：{{item.nm}}
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
    import del from '@/assets/img/personal/del.png'
    import close from "@/assets/img/close.png"
    export default {
        name: "buildProcess",
        components: {
        },
        data(){
            return{
                test1,
                excel,
                ppt,
                word,
                pdf,
                del,
                arrowDown,
                arrowUp,
                close,
                currentRole:{},
                imgList:[],
                fileList:[],
                list:[],//已审核列表
                listNo:[], //未通过
                submitShow:false,
                changeIndex:-1,
                options: [],//类别
                optionProcess:[],//建造流程
                checkIndex:0,
                //修改表单
                form:{
                  nm:"",
                  docsId: "",
                  catCd:"",
                  cd:"",
                  actDt:"",
                  imgUrl: "",
                  fileUrl: "",
                  shipyardId: "",
                  shipyardNm: "",
                  seq:"",
                },
            }
        },
        watch:{
          //  切换tab
          checkIndex(){
            if(this.checkIndex<=this.options.length-1){
              console.log(111);
              this.getInfo()
            }
          },
          'form.catCd'(e){
            if(e=='DOCS_BUILD_SHIP_CAT.30'){
              this.fileList=[]
            }
          }
        },
        mounted() {
          this.id= this.until.getQueryString('id')
          this.currentRole=JSON.parse(this.until.seGet('currentRole'))
          this.getDic()
        },
        methods:{


          //新增
          addNew(){
            this.changeIndex = -1
            this.form = {
              nm:"",
              docsId: "",
              catCd:"",
              cd:"",
              actDt:"",
              imgUrl: "",
              fileUrl: "",
              shipyardId: "",
              shipyardNm: "",
              seq:"",
            }
            this.imgList = []
            this.fileList = []
            this.submitShow=true
          },

            //修改
            change2(index){
                console.log("修改表单。。")
              console.log(this.list[index])
                this.changeIndex = index
                this.form = JSON.parse(JSON.stringify(this.list[index]))
                this.imgList = []
                if(this.form.imgList){
                    this.form.imgList.forEach(item=>{
                        this.imgList.push({
                            url:item.img,imgUrl:item.img,type:'2'
                        })
                    })
                }

                // console.log(this.form)
                this.fileList = []
                if(this.form.fileList){
                    this.form.fileList.forEach(item=>{
                        this.fileList.push({
                            url:item.url,imgUrl:item.img
                        })
                    })
                }

                this.submitShow = true
            },

            //修改
            change3(index){
                console.log("修改表单。。")
                console.log(this.list[index])
                this.changeIndex = index
                this.form = JSON.parse(JSON.stringify(this.list[index]))
                this.form.imgList=this.form.imgList1
                this.imgList = []
                if(this.form.imgList){
                    this.form.imgList.forEach(item=>{
                        this.imgList.push({
                            url:item.img,imgUrl:item,type:'2'
                        })
                    })
                }

                // console.log(this.form)
                this.fileList = []
                if(this.form.fileList){
                    this.form.fileList.forEach(item=>{
                        this.fileList.push({
                            url:item.url,imgUrl:item.img
                        })
                    })
                }

                this.submitShow = true
            },


            //修改
          change(index){
            this.changeIndex = index
            this.form = JSON.parse(JSON.stringify(this.listNo[index]))
            this.imgList = []
            if(this.form.imgList){
              this.form.imgList.forEach(item=>{
                this.imgList.push({
                  url:item.img,imgUrl:item.img,type:'2'
                })
              })
            }

            // console.log(this.form)
            this.fileList = []
            if(this.form.fileList){
              this.form.fileList.forEach(item=>{
                this.fileList.push({
                  url:item.url,imgUrl:item.img
                })
              })
            }

            this.submitShow = true
          },

            //删除
            delet2(index){
                this.$confirm('确定删除?', '提示', {
                    confirmButtonText: '确定',
                    cancelButtonText: '取消',
                    type: 'warning'
                }).then(() => {
                    this.api.buildDeptDel(this.list[index].id).then(res=>{
                        this.list.splice(index,1)
                        this.$message({
                            type: 'success',
                            message: '删除成功!'
                        });

                    })

                }).catch(() => {

                });
            },
          //删除
          delet(index){
            this.$confirm('确定删除?', '提示', {
              confirmButtonText: '确定',
              cancelButtonText: '取消',
              type: 'warning'
            }).then(() => {
              this.api.buildDeptDel(this.listNo[index].id).then(res=>{
                this.listNo.splice(index,1)
                this.$message({
                  type: 'success',
                  message: '删除成功!'
                });

              })

            }).catch(() => {

            });
          },
          proChange(){
            console.log('改变',this.form.cd)
            // let flag=0
            this.optionProcess.forEach((item,index)=>{
              if(this.form.cd==item.cd){
                // flag++
                this.form.seq=index+1
                this.form.nm=item.nm
              }
            })
            // if(flag===0){
              // this.form.seq=1
              // this.form.nm=item.label
            // }
          },
          toSubmit(){
            this.form.docsId=this.id
            this.form.shipyardId=this.currentRole.id
            this.form.shipyardNm=this.currentRole.company

            let fileList=[]
            let imgList=[]
            this.fileList.forEach(item=>{
              fileList.push(item.url)
            })
            this.imgList.forEach(item=>{
              imgList.push(item.url)
            })
            this.form.fileUrl=fileList.join(",")
            this.form.imgUrl=imgList.join(",")
            if(!this.form.actDt){
              this.$message({
                message: '请选择完成时间',
                type: 'warning',
                duration:'1500'
              });
              return
            }
            let timeC=this.until.formatDate(this.form.actDt)
            this.form.actDt=timeC.year+'-'+timeC.month+'-'+timeC.day
            console.log(this.form)
            if(!this.form.catCd){
              this.$message({
                message: '请选择文件类型',
                type: 'warning',
                duration:'1500'
              });
              return
            }
            // if(!this.form.cd){
            //   this.$message({
            //     message: '请选择建造过程',
            //     type: 'warning',
            //     duration:'1500'
            //   });
            //   return
            // }
            // if(!this.form.attachment){
            //   this.$message({
            //     message: '请输入详细说明',
            //     type: 'warning',
            //     duration:'1500'
            //   });
            //   return
            // }
            if(this.form.id){
              this.api.buildDeptUpd(this.form).then((res)=>{
                if(res.code==0){
                  this.submitShow=false
                  this.$message({
                    message: '修改成功',
                    type: 'success',
                    duration:'1000'
                  });
                  setTimeout(()=>{
                    this.form={}
                    this.imgList=[]
                    this.fileList=[]
                    this.getInfoNo()
                      this.getInfo()
                  },1000)
                }else{
                  this.$message({
                    message: res.msg,
                    type: 'warning',
                    duration:'1000'
                  });
                }

              })
            }else {
              this.api.buildDeptAdd(this.form).then((res)=>{
                if(res.code==0){
                  this.submitShow=false
                  this.$message({
                    message: '新增成功',
                    type: 'success',
                    duration:'1000'
                  });
                  setTimeout(()=>{
                    this.form={}
                    this.imgList=[]
                    this.fileList=[]
                    this.getInfo()
                  },1000)
                }else{
                  this.$message({
                    message: res.msg,
                    type: 'warning',
                    duration:'1000'
                  });
                }

              })
            }

          },
          async getDic(){
            this.options=await this.api.dataDictionary('DOCS_BUILD_SHIP_CAT')
            this.optionProcess=await this.api.dataDictionary('DOCS_BUILD_SHIP_PROCESS')

            console.log('获取数据字典')
            console.log(this.optionProcess)

            this.getInfo()
            this.getInfoNo()
          },

          async getInfo(){
            this.list=[]
            this.currentRole=JSON.parse(this.until.seGet('currentRole'))
            let data = []
            let qry = this.query.new()
            this.query.toW(qry,'docsId',this.id,'EQ')
            if(this.options[this.checkIndex].cd){
              this.query.toW(qry,'catCd',this.options[this.checkIndex].cd,'EQ')
              data=await this.api.buildStatusAll(this.query.toEncode(qry))
                console.log("99898999999")
                console.log(data)
            }
            console.log(this.options[this.checkIndex].cd)
            let data1=[]
            if(data.length>0){
              data.forEach((item,index)=>{
                if(index==0){
                  item.show=true
                }else{
                  item.show=false
                }
                if(this.options[this.checkIndex].cd!='DOCS_BUILD_SHIP_CAT.30'){
                  let imgList1=item.imgUrl ? item.imgUrl.split(',') : []
                  let imgList2=[]
                  imgList1.forEach(v=>{
                    imgList2.push({url:v,img:v})
                  })
                  item.imgList=imgList2

                  let fileList1=item.fileUrl?item.fileUrl.split(','):[]
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
                }else{
                  item.imgList1=item.imgUrl ? item.imgUrl.split(',') : []
                  if(data1.length==0){
                    data1[0]=item
                  }else{
                    // if(item.actDt==data1[data1.length-1].actDt){
                    //   data1[data1.length-1].imgList1.push(...item.imgList1)
                    // }else{
                      data1.push(item)
                    // }
                  }
                }
              })
            }
            if(this.options[this.checkIndex].cd!='DOCS_BUILD_SHIP_CAT.30'){
              this.list=data
            }else{
              this.list=data1
            }
          },

          //  审核未通过
          async getInfoNo(){
            this.listNo=[]
            this.currentRole=JSON.parse(this.until.seGet('currentRole'))
            let qry = this.query.new()
            this.query.toW(qry,'docsId',this.id,'EQ')
            this.query.toP(qry,1,1000)
            let data=await this.api.buildStepNo(this.query.toEncode(qry))
            console.log("建造列表审核未通过：")
            console.log(data)
            let data1=[]
            if(data.length>0){
              data.forEach((item,index)=>{
                if(index==0){
                  item.show=true
                }else{
                  item.show=false
                }
                if(this.options[this.checkIndex].cd!='DOCS_BUILD_SHIP_CAT.30'){
                  let imgList1=item.imgUrl ? item.imgUrl.split(',') : []
                  let imgList2=[]
                  imgList1.forEach(v=>{
                    imgList2.push({url:v,img:v})
                  })
                  item.imgList=imgList2

                  let fileList1=item.fileUrl?item.fileUrl.split(','):[]
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
                }else{
                  item.imgList1=item.imgUrl ? item.imgUrl.split(',') : []
                  if(data1.length==0){
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
            if(this.options[this.checkIndex].cd!='DOCS_BUILD_SHIP_CAT.30'){
              this.listNo=data
            }else{
              this.listNo=data1
            }
          },
          back(){//返回
            this.$router.back()
          },
          //删除图片
          deleteImg(index){
             this.imgList.splice(index,1)
           },
           //上传图片
          async upImg(e){
              console.log("打开图片回调。。")
            console.log(e)
            if(e.target.files.length===0){
              return
            }
            for( let i=0;i<e.target.files.length;i++){
                let img = await this.api.uploadImgNew(e.target.files[i])
                this.imgList.push({url:img,imgUrl:img,type:'2'})
                console.log('照片列表')
                console.log(this.imgList)
            }


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
            console.log(url)
            window.open(url)
          },
        }


    }
</script>
<style lang="less">
  .left .el-select{
    width: 100%;
  }
  .left-msg .el-input__inner{     //下拉框
    height: 52px;
    line-height: 52px;
    border-radius: 0;
  }
</style>
<style scoped lang="less">
  @import url("../../assets/css/init.less");
  .bigBox{
    width: 100%;
    height: 100%;
    background-color: rgba(0,0,0,0.5);
    position: fixed;
    top: 0;
    left: 0;
    display: flex;
    align-items: center;
    justify-content: center;
    z-index: 99;
    .submitBox{
      width: 600px;
      height: 80%;
      background-color: #ffffff;
      border-radius: 8px;
      display: flex;
      flex-direction: column;
      .submitTitle{
        position: relative;
        >p{
          padding: 30px 0;
          border-bottom: 1px solid #dfdfdf;
          text-align: center;
          font-size: 18px;
          color: #333333;

        }
        >img{
          position: absolute;
          width: 20px;
          height: 20px;
          top: 19px;
          right: 21px;
          cursor: pointer;
        }
      }
      .content{
        display: flex;
        flex-direction: column;
        width: 100%;
        padding: 30px 30px;
        overflow-y: auto;
          >div{
            display: flex;
            margin-bottom: 30px;
            >p{
              margin-right: 28px;
            }
            .left-title{
              width: 95px;
              margin-right: 0;
              padding-top: 19px;
              font-size:15px;
              color:rgba(102,102,102,1);
            }
            .left-msg{
              flex: 1;
              .el-select{
                width: 100%;
              }
              >input{
                width: 100%;
                height: 52px;
                border:1px solid rgba(221, 221, 221, 1);
                padding-left: 13px;

              }
              >textarea{
                width: 100%;
                height:165px;
                padding: 20px 13px;
                background:rgba(255,255,255,1);
                border:1px solid rgba(221, 221, 221, 1);
                resize: none;
              }
            }
            .imgBox{
              width: 59.5%;
              .img{
                //overflow: hidden;
                width: 100%;
                >div{
                  width:30%;
                  margin-right: 3%;
                  margin-bottom: 10px;
                  float: left;
                  position: relative;
                  height: 75px;
                  img{
                    width: 100%;
                    height: 100%;
                  }
                  .delImg{
                    position: absolute;
                    top: -7.5px;
                    right: -7.5px;
                    z-index: 2;
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
                    // width: 85px;
                    width: 100%;
                    // height: 85px;
                    height: 100%;
                    p{
                      // width: 85px;
                      width: 100%;
                      text-align: center;
                      // height: 85px;
                      // line-height: 70px;
                      border: 1px dashed #C6C6C6;
                      font-size: 50px;
                      line-height: 60px;
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
            .fileBox{
              flex: 1;
              .file{
                .uploadFile{
                  position: relative;
                  >div{
                    display: flex;
                    display: -webkit-flex;
                    color: #C6C6C6;
                    width: 100%;
                    height: 100%;
                    input{
                      // width: 100%;
                      // height: 100%;
                      opacity: 0;
                      position: absolute;
                      left: 0;
                      top: 0;
                    }
                  }
                }
                .fileP{
                  position: relative;
                  >p{
                    cursor: pointer;
                    width: 100%;
                    max-width: 400px;
                    margin: 10px 0;
                    overflow: hidden;
                    white-space: nowrap;
                    text-overflow: ellipsis;
                  }
                  .delImg{
                    cursor: pointer;
                    position: absolute;
                    top: -7.5px;
                    right: -7.5px;
                    z-index: 2;
                    width: 15px;
                    height: 15px;
                  }
                }
              }
            }
          }

          >button{
            width: 100%;
            height:52px;
            background:rgba(39,120,190,1);
            border: none;
            margin-top: 30px;
            font-size:16px;
            color:rgba(255,255,255,1);
            line-height:52px;
          }
      }
    }
  }
  .liuchengBox{
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
      .addBtn{
        background-color: #2778BE;
        color: #ffffff;
        padding: 14px 43px;
        text-align: center;
        font-size: 16px;
        display: block;
        margin-left:  50px;
      }
      >p{
        font-size: 14px;
        color: #666666;
        padding: 14px 20px;
        cursor: pointer;
        white-space: nowrap;
        span{
          color: @redColor;
        }
      }
      .clickP{
        font-size: 16px;
        color: #303030;
        font-weight: 600;
        border-bottom: 2px solid #2778BE;

      }
    }
    .bottomBox{

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

    &:hover{
      cursor: pointer;
    }
    >span:first-of-type{
      flex: 1;
    }
    p:nth-of-type(1){
      display: flex;
      align-items: center;
      padding-right: 20px;
      img{
        width: 12px;
        margin-left: 20px;
      }
    }
    p.set{
      span{
        margin-left: 10px;
        i{
          margin-right: 5px;
        }
        cursor: pointer;
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
