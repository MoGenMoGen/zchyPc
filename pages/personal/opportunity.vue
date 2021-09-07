<template>
    <!--船东(代理商)-我的商机-->
    <div class="main">
      <div style="padding:0 30px">
        <tobbar :title="title" :show-all="false"></tobbar>

      </div>
      <p style="border-bottom: 1px solid rgba(0,0,0,0.1);"></p>
      <div class="line2">
        <input placeholder="客户名称">
        <input placeholder="联系人">
        <input placeholder="联系电话">


        <div class="slt">
          <el-select v-model="status" placeholder="状态" clearable>
            <el-option
              v-for="item in statusList"
              :key="item.cd"
              :label="item.nm"
              :value="item.cd">
            </el-option>
          </el-select>

        </div>
        <p class="line2-s">查询</p>
        <p class="line2-n" @click="showAdd=true">新建商机</p>
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
          商机内容
        </p>
        <p>
          意向金额
        </p>
        <p>
          创建时间
        </p>
        <p>
          状态
        </p>
        <p>
          操作
        </p>

      </div>
      <div class="line4">
        <div v-for="(item,index) in list" :key="item.id">
          <p>{{index+1}}</p>
          <p>{{item.custNm}}</p>
          <p>{{item.linkman}}</p>
          <p>{{item.mob}}</p>
          <p>{{item.cont}}</p>
          <p>￥{{item.amt}}</p>
          <p>{{item.crtTm.slice(0,10)}}</p>
          <p>{{item.statusNm}}</p>
          <p @click="toUpd(item)"><img :src="editIc"></p>

        </div>
        <div class="footPage">
          <el-pagination
            background
            @current-change="handleCurrentChange"
            :current-page.sync="pageNo"
            :page-size="pageSize"
            layout="prev, pager, next, jumper"
            :total="total">
          </el-pagination>
        </div>
      </div>


      <div v-if="showAdd" class="addPop">
        <div class="pop-cont">
          <div class="title">
            <img :src="deleteIc" class="del" @click="showAdd=false"/>
            <div class="p-line1">
              新增商机

            </div>
          </div>
          <div class="addPopMain">
            <div class="p-line6">
              <div class="p-line6-l">标题:</div>
              <div  class="p-line6-r">
                <input placeholder="请输入商机名称" v-model="addForm.nm">
              </div>

            </div>
            <div class="p-line6">
              <div class="p-line6-l">客户名称:</div>
              <div  class="p-line6-r">
                <el-select v-model="addForm.custId" filterable
                           remote
                           @change="changeCustom"
                           :remote-method="remoteMethod"
                           :loading="loading"
                           placeholder="请选择客户">
                  <el-option
                    v-for="item in custumList"
                    :key="item.id"
                    :label="item.custNm"
                    :value="item.id">
                    {{item.custNm }}({{item.linkman}}{{item.mob}})
                  </el-option>
                </el-select>
              </div>

            </div>
            <div class="p-line6">
              <div class="p-line6-l">联系人:</div>
              <div  class="p-line6-r">
                <input placeholder="请输入联系人" v-model="addForm.linkman" :disabled="true">
              </div>

            </div>

            <div class="p-line6">
              <div class="p-line6-l">联系电话:</div>
              <div  class="p-line6-r">
                <input placeholder="请输入联系电话" v-model="addForm.mob" type="tel" :disabled="true">
              </div>

            </div>
            <div class="p-line6">
              <div class="p-line6-l">联系邮箱:</div>
              <div  class="p-line6-r">
                <input placeholder="请输入联系邮箱" v-model="addForm.email" :disabled="true">
              </div>

            </div>


            <div class="p-line4">
              <div class="p-line4-l">商机内容:</div>
              <div  class="p-line4-r">

              <textarea placeholder="商机内容" v-model="addForm.cont">

              </textarea>

              </div>

            </div>

            <div class="p-line3">
              <div class="p-line3-l">商机状态:</div>
              <div class="p-line3-r">
                <el-select v-model="addForm.statusCd" filterable placeholder="状态">
                  <el-option
                    v-for="item in statusList"
                    :key="item.cd"
                    :label="item.nm"
                    :value="item.cd">
                  </el-option>
                </el-select>

              </div>
            </div>
            <div class="p-line6">
              <div class="p-line6-l">意向金额:</div>
              <div  class="p-line6-r">
                <input placeholder="请输入意向金额" v-model="addForm.amt" type="number" >
              </div>

            </div>
            <div class="p-line5">
              <div class="p-line5-l"></div>
              <div  class="p-line5-r">

                <p @click="showAdd=false">取消</p>
                <p @click="submit">提交</p>
              </div>

            </div>
          </div>


        </div>


      </div>
      <div v-if="dialogVisible" class="pop" >
        <div class="pop-cont2">
          <p><span>商机状态</span>
            <img :src="deleteIc" @click="dialogVisible=false">
          </p>
          <p>
            <el-select v-model="changeForm.statusCd" placeholder="商机" clearable>
              <el-option
                v-for="item in statusList"
                :key="item.cd"
                :label="item.nm"
                :value="item.cd">
              </el-option>
            </el-select>
          </p>
          <p>
            <span @click="dialogVisible=false">取消</span>
            <span @click="toConfirm">确认</span>
          </p>
        </div>
      </div>

    </div>
</template>

<script>
  import {mapState} from "vuex";
    import tobbar from "../../components/person/tobbar";
    import searchIc from '../../assets/img/personal/search.png'
    import deleteIc from '../../assets/img/personal/delete.png'
    import successIc from '../../assets/img/success.png'
    import editIc from '../../assets/img/personal/编辑.png'
    export default {
        name: "allShip",
        components:{
            tobbar,
        },
      data(){
        return{
          searchIc,
          deleteIc,
          successIc,
          editIc,
          dialogVisible:false,
          toastVisible:false,
          title:'我的商机',
          showAdd:false,//新增弹框
          options: [],

          statusList:[],
          status:'',
          custNm:'',
          linkMan:'',
          mob:'',
          loading:false,
          addForm:{
            nm: "",//商机标题
            custId: "",
            custNm: "",
            linkman: "",
            mob: "",
            email: "",
            cont: "",
            statusCd: "",
            amt: "",
            orgEnterId: "",
            rmks: ""
          },
          changeForm:{
            id:'',
            nm: "",//商机标题
            custId: "",
            custNm: "",
            linkman: "",
            mob: "",
            email: "",
            cont: "",
            statusCd: "",
            amt: "",
            orgEnterId: "",
            rmks: ""
          },
          custumList:[],
          list:[],
          pageNo:1,
          pageSize:20,
          total:0,
        }
      },
      layout:'person',
      mounted(){
        this.addForm.orgEnterId = this.currentRole ? this.currentRole.id : ''
        this.getList()
        this.getStatusList()
        this.getCustum('')
      },
      computed:{
        ...mapState([
          "currentRole",
        ])
      },
      watch:{
        currentRole(){
          this.addForm.orgEnterId = this.currentRole.id
        },
      },
      methods:{
        remoteMethod(query) {
          if (query !== '') {
            this.loading = true;
            this.getCustum(query)
          }
        },
        //客户档案
        async getCustum(val){
          let qry = this.query.new()
          this.query.toW(qry,'custNm',val,'LK')
          this.custumList = await this.api.custInfoList2(this.query.toEncode(qry))
          this.loading = false;

        },
        //更换客户公司
        changeCustom(id){
          let val = this.custumList.find(item=>item.id==id)
          console.log(val)
          this.addForm.linkman = val.linkman ? val.linkman : ''
          this.addForm.mob = val.mob ? val.mob : ''
          this.addForm.email = val.email
          this.addForm.custId = id
          this.addForm.custNm = val.custNm ? val.custNm : ''
        },
        //商机状态
        async getStatusList(){
          this.statusList = await this.api.dataDictionary('OPPORTUNITY_STATUS')
        },
        async getList(){
           let qry = this.query.new()
          this.query.toW(qry,'custNm',this.custNm,'LK')
          this.query.toW(qry,"linkman",this.linkMan,'LK')
          this.query.toW(qry,"mob",this.mob,'LK')
          this.query.toW(qry,'statusCd',this.status,'EQ')
          this.query.toP(qry,this.pageNo,this.pageSize)
          let data = await this.api.custProjectPage(this.query.toEncode(qry))
          this.list = data.data.list
          this.total = data.page.total
        },
        //新增商机
        submit(){
          if(!this.addForm.nm){
            this.$message({
              message: '请输入商机名称',
              type: 'warning'
            });
            return
          }
          if(!this.addForm.custId){
            this.$message({
              message: '请选择跟进客户',
              type: 'warning'
            });
            return
          }
          if(!this.addForm.cont){
            this.$message({
              message: '请输入商机内容',
              type: 'warning'
            });
            return
          }
          if(!this.addForm.statusCd){
            this.$message({
              message: '请选择商机状态',
              type: 'warning'
            });
            return
          }
          if(!this.addForm.amt){
            this.$message({
              message: '请输入意向金额',
              type: 'warning'
            });
            return
          }
          this.store.commit('changeLoading',true)
          this.api.custProjectAdd(this.addForm).then(()=>{
            this.$message({
              message: '操作成功',
              type: 'success'
            });
            this.showAdd = false
            this.pageNo = 1
            this.getList()
          })
        },
        toUpd(item){
          this.changeForm = item
          this.dialogVisible=true
        },
        //商机修改
        toConfirm(){
          this.store.commit('changeLoading',true)
          this.api.custProjectUpd(this.changeForm).then(()=>{
            this.$message({
              message: '操作成功',
              type: 'success'
            });
            this.dialogVisible = false
            this.getList()
          })
        },
        toSearch(){
           this.pageNo = 1
           this.getList()
        },
        handleCurrentChange(val) {
          this.pageNo = val
          this.getList()
        },
      },
    }
</script>

<style scoped lang="less">
  .footPage{
    display: flex;
    justify-content: center;
    margin: 44px 0 20px;
    .btn_confirm{
      margin-left: 43px;
    }
  }
  .main{
    width: 100%;
    .line2{
      width: 80%;
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
      p:nth-of-type(8){
        flex:1;
        text-align: center;
      }
      p:nth-of-type(9){
        flex:0.5;
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
        p:nth-of-type(8){
          flex:1;
          text-align: center;
        }
        p:nth-of-type(9){
          flex:0.5;
          text-align: center;
          img{
            width: 20px;
            &:hover{
              cursor: pointer;
            }
            &:active{
              opacity: 0.5;
            }
          }
        }


      }


    }
    .addPop{
      position: fixed;
      top: 0;
      left: 0;
      right: 0;
      bottom: 0;
      background: rgba(0,0,0,0.1);
      .pop-cont{
        position: absolute;
        top: 50%;
        left: 50%;
        transform: translate(-50%,-50%);
        width:560px;
        height:80%;
        background:rgba(255,255,255,1);
        border-radius:8px;
        display: flex;
        display: -webkit-flex;
        flex-direction: column;
        .addPopMain{
          flex: 1;
          height: 100%;
          overflow-y: scroll;
          padding-bottom: 20px;
        }
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
        .p-line2{
          padding: 0 40px;
          width: 100%;
          display: flex;
          div:nth-of-type(1){
            margin-top: 29px;
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

            }
          }
        }
        .p-line3 {
          margin-top: 30px;
          padding: 0 40px;
          width: 100%;
          display: flex;
          align-items: center;
          .p-line3-l{
            width: 69px;
            margin-right: 30px;
            flex-shrink: 0;
          }

          .p-line3-r{
            width: 100%;
            /deep/.el-input__inner{
              width: 373px;
              border-radius: 0;
              height: 52px;
            }
            /deep/ input{
              &::placeholder{
                color: #666666;
              }
            }

          }
        }
        .p-line4 {
          margin-top: 30px;
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
              height: 136px;
              border:1px solid rgba(221, 221, 221, 1);
            }
            textarea::placeholder{
              color: #666666;
            }
          }
        }
        /*联系电话*/
        .p-line6 {
          margin-top: 30px;
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
              height: 52px;
              border:1px solid rgba(221, 221, 221, 1);
            }
            input::placeholder{
              color: #666666;
            }
          }
        }
        /*日历*/
        .p-line8 {
          margin-top: 30px;
          padding: 0 40px;
          width: 100%;
          display: flex;
          align-items: center;
          .p-line8-l{
            width: 69px;
            margin-right: 30px;
            flex-shrink: 0;
            white-space: nowrap;
          }

          .p-line8-r{
            width: 373px;
            /deep/.el-input__inner{
              width: 373px;
              border-radius: 0;
            }
            /deep/ input{
              &::placeholder{
                color: #999999;
              }
            }
          }
        }

        .p-line5 {
          margin-top: 60px;
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
              height:52px;
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
    .pop{
      position: fixed;
      top: 0;
      left: 0;
      right: 0;
      bottom: 0;
      background: rgba(0,0,0,0.1);
      .pop-cont2{
        position: absolute;
        top: 50%;
        left: 50%;
        transform: translate(-50%,-50%);
        width:381px;
        height:299px;
        background:rgba(255,255,255,1);
        border-radius:8px;
        p:nth-of-type(1){
          width: 100%;
          height: 78px;
          display: flex;
          align-items: center;
          justify-content: center;
          position: relative;
          font-size:18px;
          font-family:Microsoft YaHei;
          font-weight:400;
          color:rgba(51,51,51,1);
          border-bottom: 1px solid #DFDFDF;
          img{
            position: absolute;
            width:20px;
            height:20px;
            top:20px;
            right: 20px;
            &:hover{
              cursor: pointer;
            }
          }
        }
        p:nth-of-type(2){
          font-size:20px;
          font-family:Microsoft YaHei;
          font-weight:400;
          color:rgba(51,51,51,1);
          line-height:18px;
          width: 100%;
          margin:50px auto;
          display: flex;
          align-items: center;
          justify-content: center;
          /deep/.el-input__inner{
            width: 253px;
            height: 38px;
          }
          /deep/ input{
            &::placeholder{
              color: #999999;
            }
          }
        }
        p:nth-of-type(3){
          display: flex;
          align-items: center;
          justify-content: center;
          span{
            width:128px;
            height:42px;
            border:1px solid #CECECE;
            border-radius:5px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size:18px;
          }
          span:hover{
            cursor: pointer;
          }
          span:nth-of-type(1){
            margin-right: 22px;
          }
          span:nth-of-type(2){
            margin-left: 22px;
            color: white;
            background: #E4393C;
          }
        }
      }
    }
  }




</style>
