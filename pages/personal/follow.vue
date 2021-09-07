<template>
    <!--船东(代理商)-跟进记录-->
    <div class="main">
      <div style="padding:0 30px">
        <tobbar :title="title" :show-all="false"></tobbar>

      </div>
      <p style="border-bottom: 1px solid rgba(0,0,0,0.1);"></p>
      <div class="line2">
        <input placeholder="客户名称" v-model="custNm">
        <input placeholder="联系人" v-model="linkman">
        <input placeholder="联系电话" v-model="mob" type="tel">
        <div class="slt">
          <el-select v-model="type" placeholder="跟进方式" clearable>
            <el-option

              v-for="item in typeList"
              :key="item.cd"
              :label="item.nm"
              :value="item.cd">
            </el-option>
          </el-select>

        </div>

        <div class="slt">
          <el-select v-model="status" placeholder="跟进状态" clearable>
            <el-option
              v-for="item in statusList"
              :key="item.cd"
              :label="item.nm"
              :value="item.cd">
            </el-option>
          </el-select>

        </div>
        <p class="line2-s" @click="toSearch">查询</p>
        <p class="line2-n" @click="showAdd=true">新增跟进</p>
      </div>
      <div class="line3">
        <p>
          序号
        </p>
        <p>
          跟进客户
        </p>
        <p>
          联系人
        </p>
        <p>
          联系电话
        </p>
        <p>
          跟进内容
        </p>
        <p>
          跟进方式
        </p>
        <p>
          下次跟进时间
        </p>
        <p>
          跟进状态
        </p>
        <p>
          备注
        </p>

      </div>
      <div class="line4">
        <div v-for="(item,index) in list" :key="item.id">
          <p>{{index+1}}</p>
          <p>{{item.custNm}}</p>
          <p>{{item.linkman}}</p>
          <p>{{item.mob}}</p>
          <p>{{item.cont}}</p>
          <p>{{item.typesNm}}</p>
          <p>{{item.nextFollowTm}}</p>
          <p>{{item.statusNm}}</p>
          <p>{{item.rmks}}</p>

        </div>

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
      <div v-if="showAdd" class="addPop">
        <div class="pop-cont">
          <div class="title">
            <img :src="deleteIc" class="del" @click="showAdd=false">
            <div class="p-line1">
              新增跟进记录
            </div>
          </div>
          <div class="addPopmain">
            <div class="p-line3">
              <div class="p-line3-l">跟进客户:</div>
              <div class="p-line3-r">
                <el-select v-model="form.custId" filterable
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

            <div class="p-line3">
              <div class="p-line3-l">联系人:</div>
              <div class="p-line3-r">
                <!--<el-select v-model="value" filterable placeholder="请选择联系人">-->
                  <!--<el-option-->
                    <!--v-for="item in options"-->
                    <!--:key="item.value"-->
                    <!--:label="item.label"-->
                    <!--:value="item.value">-->
                  <!--</el-option>-->
                <!--</el-select>-->
                <el-input
                  v-model="form.linkman"
                  :disabled="true">
                </el-input>
              </div>
            </div>

            <div class="p-line6">
              <div class="p-line6-l">联系电话:</div>
              <div  class="p-line6-r">
                <el-input
                  v-model="form.mob"
                  :disabled="true">
                </el-input>
              </div>

            </div>

            <div class="p-line3">
              <div class="p-line3-l">跟进方式:</div>
              <div class="p-line3-r">
                <el-select v-model="form.typesCd" filterable placeholder="请选择跟进方式">
                  <el-option
                    v-for="item in typeList"
                    :key="item.cd"
                    :label="item.nm"
                    :value="item.cd">
                  </el-option>
                </el-select>

              </div>
            </div>

            <div class="p-line3">
              <div class="p-line3-l">跟进状态:</div>
              <div class="p-line3-r">
                <el-select v-model="form.statusCd" filterable placeholder="请选择跟进状态">
                  <el-option
                    v-for="item in statusList"
                    :key="item.cd"
                    :label="item.nm"
                    :value="item.cd">
                  </el-option>
                </el-select>

              </div>
            </div>


            <div class="p-line4">
              <div class="p-line4-l">跟进内容:</div>
              <div  class="p-line4-r">

              <textarea placeholder="多行输入" v-model="form.cont">

              </textarea>

              </div>

            </div>
            <div class="p-line8">
              <div class="p-line8-l">跟进时间:</div>
              <div class="p-line8-r">
                <el-date-picker
                  v-model="form.followTm"
                  type="date"
                  placeholder="选择日期">
                </el-date-picker>

              </div>
            </div>
            <div class="p-line8">
              <div class="p-line8-l">下次跟进时间:</div>
              <div class="p-line8-r">
                <el-date-picker
                  v-model="form.nextFollowTm"
                  type="date"
                  placeholder="选择日期">
                </el-date-picker>

              </div>
            </div>
            <div class="p-line3">
              <div class="p-line3-l">跟进商机:</div>
              <div class="p-line3-r">
                <el-select v-model="form.projId"
                           filterable
                           placeholder="请选择跟进商机"
                           remote
                           :remote-method="remoteMethod2"
                           :loading="loading"
                >
                  <el-option
                    v-for="item in businessList"
                    :key="item.id"
                    :label="item.nm"
                    :value="item.id">
                  </el-option>
                </el-select>

              </div>
            </div>
            <div class="p-line4">
              <div class="p-line4-l">备注:</div>
              <div  class="p-line4-r">

              <textarea placeholder="多行输入" v-model="form.rmks">

              </textarea>

              </div>

            </div>
            <div class="p-line5">
              <div class="p-line5-l"></div>
              <div  class="p-line5-r">

                <p @click="showAdd=false">取消</p>
                <p @click="toSumbit">提交</p>
              </div>

            </div>
          </div>


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
    export default {
        name: "allShip",
        components:{
            tobbar,
        },
        data(){
            return{
              loading:false,
                searchIc,
                deleteIc,
                successIc,
                dialogVisible:false,
                toastVisible:false,
                title:'跟进记录',
                total:0,
                pageNo:1,
                pageSize:20,
                showAdd:false,//新增弹框
                statusList:[],
                typeList:[],
                type: '',
                status:'',
              custNm:'',
              linkman:'',
              mob:'',
                list:[ ],
                option:[],
              custum:{},
              custumList:[],
              businessList:[],
                form:{
                  orgEnterId: "",
                  custId: "",
                  custNm: "",
                  projId: "",
                  linkman: "",
                  mob: "",
                  typesCd: "",
                  statusCd: "",
                  cont: "",
                  followTm: "",
                  nextFollowTm: "",
                  rmks: ""
                }
            }
        },
        layout:'person',
      computed:{
        ...mapState([
          "currentRole",
        ])
      },
      watch:{
        currentRole(){
          this.form.orgEnterId = this.currentRole.id
        },
      },
        mounted(){
          this.form.orgEnterId = this.currentRole ?  this.currentRole.id : ''
          let date = this.until.formatDate()
          this.form.followTm = date.year+'-'+date.month+'-'+date.day
          this.getStatus()
          this.getList()
          this.getCustum('')
          this.getBussinessList('')
        },
        methods:{
          remoteMethod(query) {
            if (query !== '') {
              this.loading = true;
              this.getCustum(query)
            }
          },
          remoteMethod2(query) {
            if (query !== '') {
              this.loading = true;
              this.getBussinessList(query)
            }
          },
          //商机列表
          async getBussinessList(val){
            let qry = this.query.new()
            this.query.toW(qry,'nm',val,'LK')
            if(this.form.custId){
              this.query.toW(qry,'custId',this.form.custId,'EQ')
            }
            this.businessList = await this.api.custProjectList(this.query.toEncode(qry))
          },
          //更换客户公司
          changeCustom(id){
            let val = this.custumList.find(item=>item.id==id)
            this.form.linkman = val.linkman ? val.linkman : ''
            this.form.mob = val.mob ? val.mob : ''
            this.form.custId = id
            this.form.custNm = val.custNm ? val.custNm : ''
            this.form.projId = ''
            this.getBussinessList('')
          },
          //获取跟进状态及方式
          async getStatus(){
             this.statusList = await this.api.dataDictionary('FOLLOW_STATUS')
             this.typeList = await this.api.dataDictionary('FOLLOW_METHOD')
          },
          //客户档案
          async getCustum(val){
            let qry = this.query.new()
            this.query.toW(qry,'custNm',val,'LK')
            this.custumList = await this.api.custInfoList2(this.query.toEncode(qry))
            this.loading = false;

          },
          //查询
          toSearch(){
            this.pageNo = 1
            this.getList()
          },
          //获取列表
          async getList(){
            let qry = this.query.new()
            this.query.toW(qry,'custNm',this.custNm,'LK')
            this.query.toW(qry,'linkman',this.linkman,'LK')
            this.query.toW(qry,'mob',this.mob,'LK')
            this.query.toW(qry,'statusCd',this.status,'EQ')
            this.query.toW(qry,'typesCd',this.type,'EQ')
            this.query.toP(qry,this.pageNo,this.pageSize)
            let data = await this.api.myCustFollow(this.query.toEncode(qry))
            this.list = data.data.list
            this.total = data.page.total
          },
          handleCurrentChange(val) {
            this.pageNo = val
            this.getList()
          },
          //新增
          toSumbit(){
            if(!this.form.custId){
              this.$message({
                message: '请选择跟进客户',
                type: 'warning'
              });
              return
            }
            if(!this.form.typesCd){
              this.$message({
                message: '请选择跟进方式',
                type: 'warning'
              });
              return
            }
            if(!this.form.statusCd){
              this.$message({
                message: '请选择跟进状态',
                type: 'warning'
              });
              return
            }
            if(!this.form.followTm){
              this.$message({
                message: '请选择跟进日期',
                type: 'warning'
              });
              return
            }
            this.store.commit('changeLoading',true)
            this.api.custFollwAdd(this.form).then(()=>{
              this.$message({
                message: '操作成功',
                type: 'success'
              });
              this.pageNo = 1
              this.getList()
              this.showAdd = false
            })
          }
        }
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
      }
      p:nth-of-type(7){
        flex:1;
      }
      p:nth-of-type(8){
        flex:1;
      }
      p:nth-of-type(9){
        flex:1;
      }


    }
    .line4{

      div{
        display: flex;
        height: 76px;
        align-items: center;
        border-bottom: 1px solid #DFDFDF;
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
        }
        p:nth-of-type(7){
          flex:1;
        }
        p:nth-of-type(8){
          flex:1;
        }
        p:nth-of-type(9){
          flex:1;
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
        height:901*100/1080vh;
        background:rgba(255,255,255,1);
        border-radius:8px;
        display: flex;
        display: -webkit-flex;
        flex-direction: column;
        .addPopmain{
          flex: 1;
          height: 100%;
          overflow-y: scroll;
          padding-bottom: 20px;
        }
        .del{
          position: absolute;
          width: 20*100/1080vh;
          height: 20*100/1080vh;
          top:23px;
          right: 23px;
          &:hover{
            cursor: pointer;
          }
        }
        .p-line1{
          height: 77*100/1080vh;
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
            margin-top: 29*100/1080vh;
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
              margin-top: 29*100/1080vh;
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
          margin-top: 25*100/1080vh;
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
            }
            /deep/ input{
              &::placeholder{
                color: #666666;
              }
            }

          }
        }
        .p-line4 {
          margin-top: 25*100/1080vh;
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
              padding: 20*100/1080vh;
              resize: none;
              height: 136*100/1080vh;
              border:1px solid rgba(221, 221, 221, 1);
            }
            textarea::placeholder{
              color: #666666;
            }
          }
        }
        /*联系电话*/
        .p-line6 {
          margin-top: 25*100/1080vh;
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
              padding: 20*100/1080vh;
              height: 42*100/1080vh;
              border:1px solid rgba(221, 221, 221, 1);
            }
            input::placeholder{
              color: #666666;
            }
          }
        }
        /*日历*/
        .p-line8 {
          margin-top: 25*100/1080vh;
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
                color: #666666;
              }
            }
          }
        }

        .p-line5 {
          margin-top: 42*100/1080vh;
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
              height:52*100/1080vh;
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
