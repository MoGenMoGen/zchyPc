<template>
<!--  维保服务-->
    <div class="main">
      <div style="padding:0 30px">
        <!--派单-->
        <dispatch  :show="show" @toClose="toClose" ></dispatch>
        <!--详情-->
        <service-order  :show="show2" @close="close"></service-order>
        <!--新增维保服务-->
        <task-note-add v-if="showAdd" @close="showAdd=false" @confirm="taskNoteSumbit"></task-note-add>
        <tobbar :title="title" :show-all="false"></tobbar>
      </div>
      <p style="border-bottom: 1px solid rgba(0,0,0,0.1);"></p>
      <div class="line1">
        <p>
          维保服务单：<span>{{total}}</span>
        </p>
        <p @click="showAdd=true"> 新增维保单</p>
      </div>
      <div class="tableRepair">
        <el-table
         :data="list"
         style="width: 100%;"
         :header-cell-style="{
           'font-size': '14px'
         }">
           <el-table-column
             type="index"
             align="center"
             width="70"
             label="序号">
           </el-table-column>
           <el-table-column
             prop="cd"
             width="150"
             label="服务单号">
           </el-table-column>
           <el-table-column
             width="150"
             prop="deviceNms"
             label="设备名称">
           </el-table-column>
           <el-table-column
             width="160"
             prop="serviceCont"
             label="问题描述">
           </el-table-column>
           <el-table-column
             prop="rmks"
             width="140"
             align="center"
             label="备注信息">
           </el-table-column>
           <el-table-column
             prop="crtTm"
             width="110"
             label="提交时间">
           </el-table-column>
           <el-table-column
             prop="statusNm"
             width = "100"
             label="状态">
           </el-table-column>
           <el-table-column
             fixed="right"
             align="center"
             width="120"
             prop="operations"
             label="交易操作">
             <template slot-scope="scope">
               <!--7维修完成 8支付完成-->
                <el-button type="text" v-if="(scope.row.statusCd=='TASK_NOTE_STATUS.70' || scope.row.statusCd=='TASK_NOTE_STATUS.80' ) && scope.row.evelStatus=='0'" @click="toEvl(scope.row)" size="small" class="toEva">服务评价</el-button>
                <el-button type="text" v-if="scope.row.evelStatus=='1'" size="small" plain class="evalutaed">已评价</el-button>
             </template>
           </el-table-column>
        </el-table>
      </div>
      <div class="line5">
        <el-pagination
          @current-change="handleCurrentChange"
          :current-page="currentPage"
          :page-size="pageSize"
          prev-text="上一页"
          next-text="下一页"
          layout="total, prev, pager, next, jumper"
          :total="total">
        </el-pagination>
      </div>




<!--评价弹框-->

      <div v-if="showEvl" class="addPop">
        <div class="pop-cont">
          <div style="height: 77px;">
            <img :src="deleteIc" class="del" @click="showEvl=false">
            <div class="p-line1">
              服务评价
            </div>
          </div>
          <div style="flex: 1; overflow-y: scroll;">
            <div class="p-line6">
              <div>解决速度:</div>
              <el-radio-group v-model="repairFrom.speed">
                <el-radio :label="item" v-for="(item,index) in evaluateList" :key="index"></el-radio>
              </el-radio-group>
            </div>
            <div class="p-line6">
              <div>服务态度:</div>
              <el-radio-group v-model="repairFrom.attitude">
                <el-radio :label="item" v-for="(item,index) in evaluateList" :key="index"></el-radio>
              </el-radio-group>
            </div>
            <div class="p-line6">
              <div>技术能力:</div>
              <el-radio-group v-model="repairFrom.capacity">
                <el-radio :label="item" v-for="(item,index) in evaluateList" :key="index"></el-radio>
              </el-radio-group>
            </div>
            <div class="p-line6">
              <div>服务价格:</div>
              <el-radio-group v-model="repairFrom.price">
                <el-radio :label="item" v-for="(item,index) in evaluateList" :key="index"></el-radio>
              </el-radio-group>
            </div>
            <div class="p-line6">
              <div>联系人:</div>
              <el-input v-model="repairFrom.linkman" placeholder="请输入联系人"></el-input>

            </div>
            <div class="p-line6">
              <div>联系电话:</div>
              <el-input v-model="repairFrom.phone" placeholder="请输入联系电话"></el-input>
            </div>
            <div class="p-line6">
              <div>职务:</div>
              <el-input v-model="repairFrom.linkman" placeholder="请输入职务"></el-input>
            </div>
            <div class="p-line4">
              <div class="p-line4-l">服务评价:</div>
              <div  class="p-line4-r">
                <textarea placeholder="多行输入" v-model="repairFrom.moreEvel">
                </textarea>
              </div>
            </div>
            <div class="p-line4">
              <div class="p-line4-l">备注:</div>
              <div  class="p-line4-r">
                <textarea placeholder="多行输入" v-model="repairFrom.rmks">
                </textarea>
              </div>
            </div>

            <div class="p-line5">
              <div class="p-line5-l"></div>
              <div  class="p-line5-r">
                <p @click="showEvl=false">取消</p>
                <p @click="toSubmit">提交</p>
              </div>
            </div>
          </div>

        </div>


      </div>

    </div>
</template>

<script>
    import tobbar from "../../components/person/tobbar";
    import searchIc from '../../assets/img/personal/search.png'
    import deleteIc from '../../assets/img/personal/delete.png'
    import jifen from '../../assets/img/personal/jifen.png'
    import successIc from '../../assets/img/success.png'
    import checked from '../../assets/img/personal/checked.png'
    import unchecked from '../../assets/img/personal/unchecked.png'
    import upIc from '../../assets/img/personal/上传图片.png'
    import del from '@/assets/img/personal/del.png'
   import Dispatch from "../../components/person/Dispatch";
   import ServiceOrder from "../../components/person/serviceOrder";
    import taskNoteAdd from "@/components/person/taskNoteAdd";
    export default {
        name: "allShip",
        components:{
          ServiceOrder,
          Dispatch,
            tobbar,
          taskNoteAdd
        },

        layout:'person',
        data(){
            return{
                searchIc,
                deleteIc,
                successIc,
                jifen,
                checked,
                unchecked,
                upIc,
                del,
                show:false,//派单
                title:'维保服务',
                showEvl:false,//评价弹框
                showAdd:false,//新增弹框
                total:0,
                currentPage:1,
                pageSize:6,
                list:[],
                addForm:{},
                repairFrom:{
                  taskId:"",
                  speed: "非常满意",
                  attitude: "非常满意",
                  capacity: "非常满意",
                  price: "非常满意",
                  job: "",
                  moreEvel: "",
                },
                evaluateList:['非常满意','比较满意','一般','不满意'],
                currentRole:{},
                show2:false, //详情
            }
        },
        watch:{
        },
        mounted(){
          this.currentRole=JSON.parse(this.until.seGet('currentRole'))

          this.getList()
        },
        methods:{

          async getList(){
            let qry = this.query.new()
            console.log(this.currentRole)
            let param={
              orgId:this.currentRole.id
            }
            this.query.toO(qry,'crtTm','desc')
            this.query.toP(qry,this.currentPage3,this.pageSize)
            let data=await this.api.maintenR(this.query.toEncode(qry),param)
            this.total=data.page.total
            data.data.list.forEach((item,index)=>{
              item.no=index+1
              item.crtTm = item.crtTm.split(' ')[0]

            })
            this.list = data.data.list
          },

          handleSizeChange(val) {
              console.log(`每页 ${val} 条`);
          },
          handleCurrentChange(val) {
              console.log(`当前页: ${val}`);
          },
          //去评价
          toEvl(item){
            console.log(item.id)
            this.repairFrom.linkman = item.linkman
            this.repairFrom.phone = item.mob
            this.repairFrom.taskId=item.id,
            this.showEvl=true
          },
          toClose(){
            this.show = false
          },
          close(){
            this.show2 = false
          },
          //新增维保单提交确定
          taskNoteSumbit(){
            this.showAdd = false
            this.getList()
          },
          //提交评价
          toSubmit(){
            this.store.commit('changeLoading',true)
            this.api.toRepair(this.repairFrom).then(res=>{
              console.log(res)
              this.$message({
                message: '评价成功',
                type: 'success',
                duration:'1500'
              });
              setTimeout(() => {
                this.showEvl=false
                this.getList()
              }, 1500);
            })
          },

        },

    }
</script>
<style lang="less">
  .tableRepair{
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
    .toEva{
      border:1px solid rgba(39, 120, 190, 1);
      border-radius:3px;
      color: #2778BE;
      padding: 9px 15px;
    }
    .evalutaed{
      color: #999999;
    }
  }
</style>

<style scoped lang="less">
  .main{
    width: 100%;


    .line1{
      padding:0 18px 0 30px;
      width: 100%;
      height: 81px;
      position: relative;
      display: flex;
      align-items: center;
      justify-content: space-between;
      p:nth-of-type(1){
        display: flex;
        align-items: center;
        color: #333333;
        font-size: 18px;
        span{
          color: #E4393C;
        }

      }
      p:nth-of-type(2){
        width:126px;
        height:41px;
        background:rgba(39,120,190,1);
        border-radius:4px;
        color: white;
        display: flex;
        align-items: center;
        justify-content: center;
        font-size:16px;
        &:hover{
          cursor: pointer;
        }
        &:active{
          opacity: 0.5;
        }
      }
    }
    .line5{
      padding: 30px 40px;
      width: 100%;
      display: flex;
      justify-content: flex-end;
    }
  }

  .active2{
    color: #2572B5;
    /*border-bottom: 2px solid #2572B5;*/
  }


  /*新增维保弹框*/
  .addPop{
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    background: rgba(0,0,0,0.1);
    z-index: 100;
    .pop-cont{
      position: absolute;
      display: flex;
      flex-direction: column;
      top: 50%;
      left: 50%;
      transform: translate(-50%,-50%);
      width:691px;
      height: 75%;
      background:rgba(255,255,255,1);
      border-radius:8px;
      .del{
        position: absolute;
        width: 20px;
        height: 20px;
        top:23px;
        right: 23px;
        &:hover{
          cursor: pointer;
        }
      }
      .p-line1{
        height: 77px;
        width: 100%;
        display: flex;
        align-items: center;
        justify-content: center;

        border-bottom: 1px solid #DFDFDF;
        font-family:Microsoft YaHei;
        font-weight:400;
        color:rgba(51,51,51,1);
        font-size: 18px;

      }


      .p-line4 {
        margin-top: 42px;
        padding: 0 29px;
        width: 100%;
        display: flex;

        .p-line4-l{
          width: 69px;
          margin-right: 30px;
          flex-shrink: 0;
        }

        .p-line4-r{
          width: 500px;
          textarea{
            width: 100%;
            padding: 20px;
            resize: none;
            height: 136px;
            border:1px solid rgba(221, 221, 221, 1);
          }
          textarea::placeholder{
            color: #666666;
          }
        }
      }
      .p-line5 {
        margin-top: 42px;
        padding: 0 29px;
        width: 100%;
        display: flex;
        margin-bottom: 30px;
        .p-line5-l{
          width: 69px;
          margin-right: 30px;
          flex-shrink: 0;
        }

        .p-line5-r{
          width: 500px;
          font-size: 16px;
          display: flex;
          align-items: center;
          justify-content: space-between;
          p{
            display: flex;
            align-items: center;
            justify-content: center;
            font-weight:400;
            height: 52px;
            &:hover{
              cursor: pointer;
            }
            &:active{
              opacity: 0.5;
            }
          }
          p:nth-of-type(1){
            width:179px;


            border:1px solid rgba(221, 221, 221, 1);
            color: #666666;

          }
          p:nth-of-type(2){
            width:308px;

            background:rgba(39,120,190,1);
            color:rgba(255,255,255,1);

          }
        }
      }
      /*服务满意度单选框列表*/

      .p-line6{
        padding: 20px 29px;
        width: 100%;
        display: flex;
        div:nth-of-type(1){
          width: 69px;
          margin-right: 30px;
          flex-shrink: 0;
        }
        div:nth-of-type(2){
          width: 100%;
          display: flex;
          flex-wrap: nowrap;
          justify-content: space-between;
          padding-right: 40px;
          p{
            width: 33%;
            display: flex;
            align-items: center;
            justify-content: center;
            margin-top: 29px;
            img{
              width: 24px;
              margin-right: 10px;
              &:hover{
                cursor: pointer;
              }
            }

            span{
              color: #333333;
              font-size: 16px;
              &:hover{
                cursor: pointer;
              }
            }
            &:first-child{
              justify-content: flex-start;
            }
            &:last-child{
              justify-content: flex-end;
              text-align: right;

            }
          }
        }
      }

      .p-line7{
        display: flex;
        // justify-content: center;
        // align-items: center;
        padding: 0 29px;
        margin:42px 0 28px;
        >p{
          width: 69px;
          margin-right: 30px;
        }
        .imgBox{
          width: 500px;
          .img{
            //overflow: hidden;
            width: 100%;
            >div{
              width:30%;
              // width: 85px;
              // height: 85px;
              margin-right: 3%;
              margin-bottom: 10px;
              float: left;
              position: relative;
              height: 150px;
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
                  display: flex;
                  flex-direction: column;
                  justify-content: center;
                  width: 100%;
                  text-align: center;
                  // height: 85px;
                  // line-height: 135px;
                  border: 1px dashed #C6C6C6;
                  font-size: 50px;
                  span{
                    font-size: 16px;
                    margin-top: 20px;
                  }
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


</style>
