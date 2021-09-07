<template>
    <!--船东(代理商)-客户档案-->
    <div class="main">
      <div style="padding:0 30px">
        <tobbar :title="title" :show-all="false"></tobbar>

      </div>
      <p style="border-bottom: 1px solid rgba(0,0,0,0.1);"></p>
      <div class="line2">
        <input placeholder="客户名称" v-model="custNm" clearable>
        <input placeholder="联系人" v-model="linkman" clearable>
        <input placeholder="联系电话" v-model="mob" clearable>

        <p class="line2-s" @click="toSearch">查询</p>
        <p class="line2-n" @click="showAdd=true">新增客户</p>
      </div>
      <div class="line3">
        <p>
          序号
        </p>
        <p>
          客户名称
        </p>
        <p>
          联系人
        </p>
        <p>
          联系电话
        </p>
        <p>
          客户地址
        </p>
        <p>
          创建时间
        </p>
        <p>
          备注
        </p>

      </div>
      <div class="line4">
        <div v-for="(item,index) in list" :key="item.id" @click="toDetail(item)">
          <p>{{index+1}}</p>
          <p>{{item.custNm}}</p>
          <p>{{item.linkman}}</p>
          <p>{{item.mob}}</p>
          <p>{{item.addrNm}}-{{item.address}}</p>
          <p>{{item.crtTm | timeFormat }}</p>
          <p>{{item.rmks}}</p>

        </div>

      </div>


      <div v-show="showAdd" class="addPop">
        <div class="pop-cont">
          <div class="title">
            <img :src="deleteIc" class="del" @click="showAdd=false">
            <div class="p-line1">
              新增客户

            </div>
          </div>

          <div class="addPopmain">
            <div class="p-line6">
              <div class="p-line6-l">客户名称:</div>
              <div  class="p-line6-r">
                <input placeholder="请输入客户名称/公司名称" v-model="cusInfo.custNm" clearable>
              </div>

            </div>
            <div class="p-line6">
              <div class="p-line6-l">联系人:</div>
              <div  class="p-line6-r">
                <input placeholder="请输入联系人" v-model="cusInfo.linkman" clearable>
              </div>

            </div>


            <div class="p-line6">
              <div class="p-line6-l">联系电话:</div>
              <div  class="p-line6-r">
                <input placeholder="请输入联系电话" v-model="cusInfo.mob" clearable>
              </div>

            </div>

            <div class="p-line6">
              <div class="p-line6-l">联系邮箱:</div>
              <div  class="p-line6-r">
                <input placeholder="请输入联系邮箱" v-model="cusInfo.email" clearable>
              </div>

            </div>

            <div class="p-line6">
              <div class="p-line6-l">客户区域:</div>
              <div  class="p-line6-r">
                <addr @changeAddr="changeAddr"  ref="addrC"></addr>
              </div>

            </div>
            <div class="p-line6">
              <div class="p-line6-l">详细地址:</div>
              <div  class="p-line6-r">
                <input placeholder="请输入详细地址" v-model="cusInfo.address" clearable>
              </div>

            </div>

            <div class="p-line4">
              <div class="p-line4-l">备注:</div>
              <div  class="p-line4-r">

              <textarea placeholder="请输入备注" v-model="cusInfo.rmks" clearable>

              </textarea>

              </div>

            </div>




            <div class="p-line5">
              <div class="p-line5-l"></div>
              <div  class="p-line5-r">

                <p @click="showAdd=false">取消</p>
                <p @click="toSubmit">提交</p>
              </div>

            </div>
          </div>


        </div>


      </div>
      <div class="line5">
        <el-pagination
          @size-change="handleSizeChange"
          @current-change="handleCurrentChange"
          :current-page="pageNo"
          :page-sizes="[10, 20, 30, 40]"
          :page-size="pageSize"
          prev-text="上一页"
          next-text="下一页"
          layout="total, sizes, prev, pager, next, jumper"
          :total="total">
        </el-pagination>
      </div>

    </div>
</template>

<script>
    import {mapState} from "vuex";
    import tobbar from "../../components/person/tobbar";
    import searchIc from '../../assets/img/personal/search.png'
    import deleteIc from '../../assets/img/personal/delete.png'
    import successIc from '../../assets/img/success.png'
    import addr from "@/components/addr";
    export default {
        name: "allShip",
        components:{
            tobbar,
            addr,
        },
      computed:{
        ...mapState([
          "currentRole",
        ])
      },
      watch:{
        currentRole(){
          this.cusInfo.orgEnterId = this.currentRole.id
        },
      },
        methods:{
            toDetail(item){
              this.$router.push({
                  path:'/personal/profileDetail',
                  query:{
                      id:item.id
                  }
              })
            },
           async toSearch(){
                this.pageNo=1
                this.getList()

            },
           async  getList(){
               let qry=this.query.new();
               this.query.toP(qry,this.pageNo,this.pageSize)
               if(this.custNm){
                   this.query.toW(qry,"custNm",this.custNm,"LK")
               }
               if(this.linkman){
                   this.query.toW(qry,"linkman",this.linkman,"LK")
               }
               if(this.mob){
                   this.query.toW(qry,"mob",this.mob,"LK")
               }
               this.query.toO(qry,'crtTm',"desc")
               let res= await this.api.custInfoList(this.query.toEncode(qry))
               this.list=res.data.list
               this.total=res.page.total
               // this.total=20
           },
            //新增客户
            toSubmit(){
                if(!this.cusInfo.custNm){
                    this.$message({
                        message: '请输入客户名称',
                        type: 'warning'
                    });
                    return
                }
                if(!this.cusInfo.linkman){
                    this.$message({
                        message: '请输入联系人名称',
                        type: 'warning'
                    });
                    return
                }
                if(this.reg.checkPhone(this.cusInfo.mob)!='ok'){
                    this.$message({
                        message: this.reg.checkPhone(this.cusInfo.mob),
                        type: 'warning'
                    });
                    return
                }
                if(this.reg.checkMail(this.cusInfo.email)!='ok'){
                    this.$message({
                        message: this.reg.checkMail(this.cusInfo.email),
                        type: 'warning'
                    });
                    return
                }
                if(!this.cusInfo.address){
                    this.$message({
                        message: '请输入详情地址',
                        type: 'warning'
                    });
                    return
                }

              this.store.commit('changeLoading',true)

               this.api.addCustomer(this.cusInfo).then(res=>{
                   this.$message({
                     message: '提交成功',
                     type: 'success'
                   });
                   setTimeout(()=>{
                     this.showAdd=false
                       this.pageNo=1
                       this.getList()
                   },2000)
               })
            },
            //获取地区信息
            changeAddr(info){
                let data = JSON.parse(info)
                window.console.log(data)
                this.cusInfo.addrNm = data.province+"-"+data.city+"-"+data.region
                this.cusInfo.addrCd = data.provinceId+"-"+data.cityId+"-"+data.regionId
            },
            handleSizeChange(val) {
                this.pageSize=val
                this.pageNo=1
                this.getList()
                console.log(`每页 ${val} 条`);
            },
            handleCurrentChange(val) {
                this.pageNo=val
                this.getList()
                console.log(`当前页: ${val}`);
            },



        },
        mounted(){
          this.cusInfo.orgEnterId = this.currentRole.id
          this.getList()
          this.$nextTick(() => {
            this.$refs.addrC.getProvice()
          });

        },
        filters:{
          timeFormat(val){
              if(val){
                  return val.substring(0,10)
              }else{
                  return val
              }
          }

        },
        data(){
            return{
                searchIc,
                deleteIc,
                successIc,
                pageNo:1,
                pageSize:10,
                total:0,//
                custNm:'',//客户名称
                linkman:'',//联系人
                mob:'',//联系电话
                dialogVisible:false,
                toastVisible:false,
                title:'客户档案',
                showAdd:false,//新增弹框
                cusInfo:{
                  orgEnterId:'',
                    custNm:'',//客户姓名
                    linkman:'',//
                    mob:'',
                    email:'',
                    addrCd:'',
                    addrNm:'',
                    address:'',
                    rmks:'',
                },

                list:[]

            }
        },
        layout:'person',

    }
</script>

<style scoped lang="less">
  .main{
    width: 100%;
    .line2{
      width: 100%;
      display: flex;
      align-items: center;
      height: 78px;
      padding-left: 29px;
      padding-right: 16px;

      input{
        flex-shrink: 0;
        width:120px;
        height:38px;
        background:rgba(255,255,255,1);
        border:1px solid rgba(223, 223, 223, 1);
        border-radius:5px;
        padding: 0 10px;
        color: #333333;
        margin-right: 10px;
        &::placeholder{
          color: #999999;
        }
      }
      div{
        flex:1;
        display: flex;
        align-items: center;
      }
      .slt{
        margin-right: 10px;
      }
      p{
        border-radius:4px;
        display: flex;
        align-items: center;
        justify-content: center;
        color: white;
        &:hover{
          cursor: pointer;
        }
        &:active{
          opacity: 0.5;
        }
      }
      .line2-s{
        width:100px;
        height:38px;
        background:rgba(228,57,60,1);
        margin-right: 10px;
      }
      .line2-n{
        width:100px;
        height:38px;
        background:rgba(39,120,190,1);

      }
    }
    .line3{
      display: flex;
      align-items: center;
      height: 60px;
      padding: 0 31px;
      background:#F7F7F7;
      p:nth-of-type(1){
        flex:0.8;
        text-align: center;
      }
      p:nth-of-type(2){
        flex:1;
      }
      p:nth-of-type(3){
        flex:1;
      }
      p:nth-of-type(4){
        flex:1;
      }
      p:nth-of-type(5){
        flex:1;
      }
      p:nth-of-type(6){
        flex:1;
        text-align: center;
      }
      p:nth-of-type(7){
        flex:1;
        text-align: center;
      }



    }
    .line4{

      div{
        display: flex;
        height: 76px;
        align-items: center;
        border-bottom: 1px solid #DFDFDF;
        padding: 0 31px;
        color: #333333;
        font-size: 15px;
        p{
          white-space: nowrap;
          text-overflow: ellipsis;
          overflow: hidden;

        }
        p:nth-of-type(1){
          flex:0.8;
          text-align: center;
        }
        p:nth-of-type(2){
          flex:1;
        }
        p:nth-of-type(3){
          flex:1;
        }
        p:nth-of-type(4){
          flex:1;
        }
        p:nth-of-type(5){
          flex:1;
        }
        p:nth-of-type(6){
          flex:1;
          text-align: center;
        }
        p:nth-of-type(7){
          flex:1;
          text-align: center;
        }



      }


    }
    .line5{
      padding: 30px 40px;
      width: 100%;
      display: flex;
      justify-content: flex-end;
    }

    .addPop{
      position: fixed;
      top: 0;
      left: 0;
      right: 0;
      bottom: 0;
      background: rgba(0,0,0,0.1);
      z-index: 99;


      .pop-cont{
        display: flex;
        display: -webkit-flex;
        flex-direction: column;
        box-sizing: border-box;
        position: absolute;
        top: 50%;
        left: 50%;
        transform: translate(-50%,-50%);
        width:560px;
        height:79vh;
        background:rgba(255,255,255,1);
        border-radius:8px;
        padding-bottom: 2.7vh;
        .addPopmain{
          flex: 1;
          height: 100%;
          overflow-y: scroll;
        }
        .del{
          position: absolute;
          width: 1.85vh;
          height: 1.85vh;
          top:2.12vh;
          right: 23px;
          &:hover{
            cursor: pointer;
          }
        }
        .p-line1{
          height: 7.12vh;
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
        .p-line2{
          padding: 0 40px;
          width: 100%;
          display: flex;
          div:nth-of-type(1){
            margin-top: 2.68vh;
            width: 69px;
            margin-right: 30px;
            flex-shrink: 0;
          }
          div:nth-of-type(2){
            display: flex;
            flex-wrap: wrap;

            p{
              width: 25%;
              display: flex;
              align-items: center;
              margin-top: 2.68vh;
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

            }
          }
        }

        .p-line4 {
          margin-top: 2.7vh;
          padding: 0 40px;
          width: 100%;
          display: flex;

          .p-line4-l{
            width: 69px;
            margin-right: 30px;
            flex-shrink: 0;
          }

          .p-line4-r{
            width: 373px;
            textarea{
              width: 100%;
              padding: 20px;
              resize: none;
              height: 12.6vh;
              border:1px solid rgba(221, 221, 221, 1);
            }
            textarea::placeholder{
              color: #666666;
            }
          }
        }
        /*联系电话*/
        .p-line6 {
          margin-top: 2.7vh;
          padding: 0 40px;
          width: 100%;
          display: flex;
          align-items: center;
          .p-line6-l{
            width: 69px;
            margin-right: 30px;
            flex-shrink: 0;
          }

          .p-line6-r{
            width: 373px;
            input{
              width: 100%;
              padding: 20px;
              height: 4.8vh;
              border:1px solid rgba(221, 221, 221, 1);
            }
            input::placeholder{
              color: #666666;
            }
          }
        }


        .p-line5 {
          margin-top: 3.8vh;
          padding: 0 40px;
          width: 100%;
          display: flex;

          .p-line5-l{
            width: 69px;
            margin-right: 30px;
            flex-shrink: 0;
          }

          .p-line5-r{
            width: 373px;
            font-size: 16px;
            display: flex;
            align-items: center;
            justify-content: space-between;
            p{
              display: flex;
              align-items: center;
              justify-content: center;
              font-weight:400;
              height:4.8vh;
              &:hover{
                cursor: pointer;
              }
              &:active{
                opacity: 0.5;
              }
            }
            p:nth-of-type(1){
              width:159px;
              border:1px solid rgba(221, 221, 221, 1);
              color: #666666;

            }
            p:nth-of-type(2){
              width:194px;

              background:rgba(39,120,190,1);
              color:rgba(255,255,255,1);

            }
          }
        }


      }
    }
  }




</style>
