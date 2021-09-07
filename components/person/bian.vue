<template>
<!--  变更记录-->
  <div>
      <div class="bianBox">
        <div v-for="(item,index) in list" :key="index">
          <div class="title" @click="item.show=!item.show">
            <span>时间：{{item.chgTm}}</span>
            <p>
              <img :src="arrowDown" v-if="item.show">
              <img :src="arrowUp" v-else>
            </p>
          </div>
          <div class="rich" v-if="item.show">
            <p class="desc">
              变更内容：{{item.cont}}
            </p>
            <p style="margin-top: 20px" v-show="item.fileList">附件：</p>
            <div class="doc">
              <p v-for="j in item.fileList" @click="toLink(j.url)">
                <img :src="j.img">
                <span>{{j.fileNm}}</span>
              </p>
            </div>
          </div>
        </div>
        <div class="Footer">
          <el-pagination
            background
            @current-change="handleCurrentChange"
            :current-page.sync="currentPage3"
            :page-size="pageSize"
            layout="prev, pager, next, jumper"
            :total="total">
          </el-pagination>
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
                total:0,
                pageSize:7,
                currentPage3: 1,
                currentRole:{},
                list:[],
            }
        },
        mounted() {
          this.id= this.until.getQueryString('id')
          this.getInfo()

        },
        methods:{
          async getInfo(){
            this.list=[]
            this.currentRole=JSON.parse(this.until.seGet('currentRole'))
            let qry = this.query.new()
            this.query.toW(qry,'docsId',this.id,'EQ')
            this.query.toO(qry,'chgTm','desc')
            this.query.toP(qry,this.currentPage3,this.pageSize)
            let data=await this.api.bianRecords(this.query.toEncode(qry))
            this.total=data.page.total
            data.data.list.forEach(item=>{
              item.show=false
              item.chgTm=item.chgTm?item.chgTm.split(' ')[0]:''
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
              this.list.push(item)
            })

            console.log(this.list)
          },
          toLink(url){
            console.log(url)
            window.open(url)
          },
          handleCurrentChange(val) {
            console.log(`当前页: ${val}`);
            this.currentPage3=val
            this.getInfo()
          },
        }


    }
</script>

<style scoped lang="less">
  .bianBox{
    padding: 0 60px 0 30px;
    .Footer{
      margin-top: 40px;
      width: 100%;
      display: inline-flex;
      justify-content: center;
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
        flex-direction: column;
        // flex-wrap: wrap;
        width: 100%;
        p{
          margin-top: 5px;
          // width: 120px;
          display: flex;
          cursor: pointer;
          img{
            width: 14px;
            height: 14px;
            margin-right: 3px;
            display: block;
            &:hover{
            }
          }
          span{
            display: block;
            width: 100%;
            // text-align: center;
            color: #0066cc;
            font-size: 14px;
            // margin-top: 5px;
          }
        }

      }
  }
</style>
