<template>
    <!--经销商-子账号维护-->
    <div class="main">
      <div style="padding:0 30px">
        <tobbar :title="title" :show-all="false"></tobbar>

      </div>
      <p style="border-bottom: 1px solid rgba(0,0,0,0.1);"></p>
      <div class="line2">
          <input v-model="searchName" placeholder="姓名">
          <input v-model="searchMob" placeholder="联系电话">
          <input v-model="search" placeholder="账户名称">
        <p @click="toSearch">查询</p>
        <p @click="showAdd=true">新增子账户</p>
      </div>
      <div class="table">
        <el-table
          :data="list"
          style="width: 100%">
          <el-table-column
            type="index"
            label="序号"
            :index="indexMethod"
            align="center"
            width="60">
          </el-table-column>
          <el-table-column
            prop="realNm"
            label="姓名"
            :show-overflow-tooltip="true"
            align="left"
            width="150">
          </el-table-column>
          <el-table-column
            prop="mob"
            align="left"
            label="联系电话">
          </el-table-column>
          <el-table-column
            prop="username"
            align="left"
            :show-overflow-tooltip="true"
            label="子账户名称">
          </el-table-column>
          <el-table-column
            prop="rscoNm"
            align="left"
            :show-overflow-tooltip="true"
            label="权限">
          </el-table-column>
          <el-table-column
            prop="crtTmn"
            align="center"
            label="创建时间">
          </el-table-column>
          <el-table-column
            prop="rmks"
            align="center"
            :show-overflow-tooltip="true"
            label="备注">
          </el-table-column>
          <el-table-column
            fixed="right"
            label="操作"
            align="center"
            width="100">
            <div class="btnList" slot-scope="scope">
              <p style="color: #DFDFDF;">
                <span @click="openUpd(scope.row)" style="color: #2778BE;margin-right: 9px;cursor:pointer;">修改</span>|<span @click="delData(scope.row)" style="margin-left:9px;color: #2778BE;cursor:pointer;">删除</span>
              </p>
            </div>
          </el-table-column>
        </el-table>
      </div>
      <el-pagination
        background
        layout="prev, pager, next"
        @current-change="handleCurrentChange"
        :current-page.sync="pageNo"
        :page-size="pageSize"
        :total="total">
      </el-pagination>
      <!--新增子账户-->
      <div v-if="showAdd" class="addPop">
        <div class="pop-cont">
          <img :src="deleteIc" class="del" @click="showAdd=false">
          <div class="p-line1">
            新增子账户
          </div>
          <div class="addPopMain">
            <div class="p-line3">
              <div class="p-line3-l"><span class="red">*</span>账户名称:</div>
              <div  class="p-line3-r">
                <input v-model="addInfo.username" placeholder="请输入与账户名称">
              </div>
            </div>
            <div class="p-line3">
              <div class="p-line3-l"><span class="red">*</span>姓名:</div>
              <div  class="p-line3-r">
                <input v-model="addInfo.realNm" placeholder="请输入姓名">
              </div>
            </div>
            <div class="p-line3">
              <div class="p-line3-l"><span class="red">*</span>联系电话:</div>
              <div  class="p-line3-r">
                <input v-model="addInfo.mob" placeholder="请输入联系电话">
              </div>
            </div>
            <div class="p-line3">
              <div class="p-line3-l"><span class="red"></span>联系邮箱:</div>
              <div  class="p-line3-r">
                <input v-model="addInfo.email" placeholder="请输入联系邮箱">
              </div>
            </div>
            <div class="p-line4">
              <div class="p-line4-l"><span class="red"></span>备注:</div>
              <div  class="p-line4-r">
              <textarea v-model="addInfo.rmks" placeholder="请输入备注">
              </textarea>
              </div>
            </div>
            <div class="p-line8">
              <div><span class="red">*</span>选择角色:</div>
              <div>
                <el-checkbox-group v-model="addInfo.orgEnterIds">
                  <el-checkbox :label="item" v-for="(item,index) in roleList" :key="index">{{item.company}}[{{item.identityNm}}]</el-checkbox>
                </el-checkbox-group>
              </div>
            </div>
            <div class="p-line2" v-show="addInfo.orgEnterIds.length>0">
              <div><span class="red"></span>权限设置:</div>
              <div>
                <el-checkbox-group v-model="addInfo.permList">
                  <el-checkbox :disabled="addInfo.orgEnterIds.length>0?false:true" v-show="item.show" :label="item.id" v-for="(item,index) in levelList" :key="index">{{item.nm}}</el-checkbox>
                </el-checkbox-group>
              </div>
            </div>
            <div class="p-line5">
              <div class="p-line5-l"></div>
              <div  class="p-line5-r">
                <p @click="showAdd=false">取消</p>
                <p @click="newData">提交</p>
              </div>
            </div>
          </div>
        </div>
      </div>

      <!--修改子账户-->
      <div v-if="showUpd" class="addPop">
        <div class="pop-cont">
          <img :src="deleteIc" class="del" @click="showUpd=false">
          <div class="p-line1">
            修改子账户
          </div>
          <div class="addPopMain">
            <div class="p-line3">
              <div class="p-line3-l">账户名称:</div>
              <div  class="p-line3-r">
                <input v-model="updInfo.username" placeholder="请输入与账户名称">
              </div>
            </div>
            <div class="p-line3">
              <div class="p-line3-l">姓名:</div>
              <div  class="p-line3-r">
                <input v-model="updInfo.realNm" placeholder="请输入姓名">
              </div>
            </div>
            <div class="p-line3">
              <div class="p-line3-l">联系电话:</div>
              <div  class="p-line3-r">
                <input v-model="updInfo.mob" placeholder="请输入联系电话">
              </div>
            </div>
            <div class="p-line3">
              <div class="p-line3-l">联系邮箱:</div>
              <div  class="p-line3-r">
                <input v-model="updInfo.email" placeholder="请输入联系邮箱">
              </div>
            </div>
            <div class="p-line4">
              <div class="p-line4-l">备注:</div>
              <div  class="p-line4-r">
              <textarea v-model="updInfo.rmks" placeholder="请输入备注">
              </textarea>
              </div>
            </div>
            <div class="p-line8">
              <div>选择角色:</div>
              <div>
                <el-checkbox-group v-model="updInfo.orgEnterIds">
                  <el-checkbox :label="item" v-for="(item,index) in roleList" :key="index">{{item.company}}[{{item.identityNm}}]</el-checkbox>
                </el-checkbox-group>
              </div>
            </div>
            <div class="p-line2" v-show="updInfo.orgEnterIds.length>0">
              <div>权限设置:</div>
              <div>
                <el-checkbox-group v-model="updInfo.permList">
                  <el-checkbox :disabled="updInfo.orgEnterIds.length>0?false:true" v-show="item.show" :label="item.id" v-for="(item,index) in levelList" :key="index">{{item.nm}}</el-checkbox>
                </el-checkbox-group>
              </div>
            </div>
            <div class="p-line5">
              <div class="p-line5-l"></div>
              <div  class="p-line5-r">
                <p @click="showUpd=false">取消</p>
                <p @click="updData">提交</p>
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
    import successIc from '../../assets/img/success.png'
    import checked from '../../assets/img/personal/checked.png'
    import unchecked from '../../assets/img/personal/unchecked.png'
    export default {
        name: "allShip",
      data(){
        return{
          searchIc,
          deleteIc,
          successIc,
          checked,
          unchecked,
          pageNo:1,
          pageSize:10,
          total:0,
          dialogVisible:false,
          toastVisible:false,
          title:'子账户维护',
          showAdd:false,
          showUpd:false,
          search:'',
          searchName:'',
          searchMob:'',
          addInfo:{
            id: 0,
            userId: 0,
            username: "",
            mob: "",
            realNm: "",
            email: "",
            rmks: "",
            orgEnterIds: [],
            permList: []
          },
          updInfo:{
            id: 0,
            userId:0 ,
            username: "",
            mob: "",
            realNm: "",
            email: "",
            rmks: "",
            orgEnterIds: [],
            permList: []
          },
          delInfo:'',
          userId:'',//修改时查询权限
          list:[],
          levelList:[],
          roleList:[],
        }
      },
      layout:'person',
        components:{
            tobbar,
        },
      mounted(){
          console.log(this.updInfo)
        this.getData();
        this.getAuthz();
        this.getRsco();
        this.getRole();
      },
      watch:{
        'addInfo.orgEnterIds'(e) {
          console.log('1111111111111')
            console.log(e)
            if(e.length>0){
              this.getRsco(e,'add')
            }else{
              this.getRsco()
            }

        },
        'updInfo.orgEnterIds'(e) {
            console.log('更新吧吧吧')
            if(e.length>0){
              this.getRsco(e,'upd')
            }else{
              this.getRsco()
            }
        },
        'addInfo.permList'(e) {
          this.levelList.forEach((item,index)=>{
            item.checked=false
            e.forEach(i=>{    //将选中的加入
              if(item.id==i){
                item.checked=true
              }
            })

            this.addInfo.orgEnterIds.forEach(j=>{
              if(item.identityCds.indexOf(j.identityCd)==-1&&item.checked){//将不属于该角色的去除
                item.checked=false
              }
            })
            this.$set(this.levelList,index,this.levelList[index])
          })
        },
        'updInfo.permList'(e) {
          this.levelList.forEach((item,index)=>{
            item.checked=false
            e.forEach(i=>{    //将选中的加入
              if(item.id==i){
                item.checked=true
              }
            })

            this.updInfo.orgEnterIds.forEach(j=>{
              if(item.identityCds.indexOf(j.identityCd)==-1&&item.checked){//将不属于该角色的去除
                item.checked=false
              }
            })
            this.$set(this.levelList,index,this.levelList[index])
          })
        },
      },
        methods:{
          async getData(){
            let qry = this.query.new()
            this.query.toW(qry,'realNm',this.searchName,'LK')
            this.query.toW(qry,'mob',this.searchMob,'LK')
            this.query.toW(qry,'username',this.search,'LK')
            this.query.toP(qry,this.pageNo,this.pageSize)
            this.query.toO(qry,'crtTm','desc')
            let data = await this.api.getAccountList(this.query.toEncode(qry))
            this.list = data.data.list
            this.list.forEach((item,index) =>{
              let timeS = this.until.formatDate(item.crtTm)
              item.crtTmn = timeS.year+'-'+timeS.month+'-'+timeS.day
              this.$set(this.list,'crtTmn',item.crtTmn)
            })
          },
          //序号
          indexMethod(index) {
            return index +1;
          },
          handleCurrentChange(val){
            this.pageNo = val
            this.getData()
          },
          //新增子账户
          newData(){
            let orgIds=[]
            this.addInfo.orgEnterIds.forEach(item=>{
                orgIds.push(item.id)
            })
            let param={}
            param=JSON.parse(JSON.stringify(this.addInfo))
            param.orgEnterIds=orgIds
            // console.log('this.addinfo',param)
            this.api.addSubAccount(param).then(res =>{
                this.$message({
                  message: '新增成功',
                  type: 'success',
                  duration: '1500',
                  offset: '50'
                });
            })
            this.addInfo={
              id: 0,
              userId: 0,
              username: "",
              mob: "",
              realNm: "",
              email: "",
              rmks: "",
              orgEnterIds: [],
              permList: []
            },
            this.levelList=[]
            this.showAdd = false
            setTimeout(()=>{
              this.getData();
            },1000)

          },
          //修改子账户
          updData(){
            let orgIds=[]
            this.updInfo.orgEnterIds.forEach(item=>{
                orgIds.push(item.id)
            })
            let param={}
            param=JSON.parse(JSON.stringify(this.updInfo))
            param.orgEnterIds=orgIds
            // console.log('this.updInfo',param)
            this.api.updSubAccount(param).then(res =>{
              this.$message({
                message: '修改成功',
                type: 'success',
                duration: '1500',
                offset: '50'
              });
            })
            this.levelList=[]
            this.showUpd = false
            setTimeout(()=>{
              this.getData();
            },1000)
          },
          //删除子账户
          delData(e){
            this.api.delSubAccount(e.id)
            this.$message({
              message: '删除成功',
              type: 'success',
              duration: '1500',
              offset: '50'
            });
            setTimeout(()=>{
              this.getData();
            },1000)

          },
          //搜索
          toSearch(){
            this.pageNo = 1
            this.getData();
          },
          //查询账户权限回显
          getAuthz(){
            let data = this.api.getAuthz()
          },
          openUpd(e){
            this.showUpd = true;
            // console.log(e)
            this.updInfo.userId = e.userId
            this.updInfo.username = e.username
            this.updInfo.mob = e.mob
            this.updInfo.realNm = e.realNm
            this.updInfo.email = e.email
            this.updInfo.rmks = e.rmks
            this.updInfo.id = e.id
            this.userId = e.userId
            this.getRole(this.updInfo.id)
            this.getRsco('','upd')
          },
          //查询账户权限
          async getRsco(e,ways){
            let data
            if(!e&&!ways){  //第一次什么都不传
              console.log('11111111第一次')
              this.levelList = await this.api.getAccRsco()
            }else if(!e && ways=='upd'){  //修改数据第一次接口获取
              console.log('11111111修改数据第一次接口获取')
              this.levelList = await this.api.getAccRsco({userId:this.userId})
              this.levelList.forEach((item,index)=>{
                item.show=false
                this.updInfo.orgEnterIds.forEach(i=>{
                  if(item.identityCds.indexOf(i.identityCd)>-1){
                    item.show=true
                  }
                })
                this.$set(this.levelList,index,this.levelList[index])
              })
              this.updInfo.permList = []
              // console.log(this.levelList)
              this.levelList.forEach(item=>{
                if(item.checked){
                  this.updInfo.permList.push(item.id)
                }
              })
            }else if(e && ways=='upd'){
              console.log('11111111upd')
              this.levelList.forEach((item,index)=>{
                item.show=false
                e.forEach(i=>{
                  if(item.identityCds.indexOf(i.identityCd)>-1){
                    item.show=true
                  }
                })
                this.$set(this.levelList,index,this.levelList[index])
              })
              this.updInfo.permList = []
              // console.log(this.levelList)
              this.levelList.forEach(item=>{
                if(item.checked){
                  this.updInfo.permList.push(item.id)
                }
              })
            }
            else if(e && ways=='add'){//新增数据
              console.log('新增数据')
              this.levelList.forEach((item,index)=>{
                item.show=false
                e.forEach(i=>{
                  if(item.identityCds.indexOf(i.identityCd)>-1){
                    item.show=true
                  }
                })

                this.$set(this.levelList,index,this.levelList[index])
              })
              // console.log(this.levelList)
              this.addInfo.permList = []
              this.levelList.forEach(item=>{
                if(item.checked){
                  this.addInfo.permList.push(item.id)
                }
              })
            }
          },
          async getRole(id){
            if(id){
              let data =await this.api.listIdent(id)
              this.updInfo.orgEnterIds=[]
              this.roleList = data
              this.roleList.forEach(item=>{
                if(item.checked){
                  this.updInfo.orgEnterIds.push(item)
                }
              })
            }else{
              this.roleList = await this.api.roleList()
            }

            // console.log(this.roleList)
          },
        },


    }
</script>

<style scoped lang="less">
  .red{
    width: 9px;
    display: inline-block;
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
        width:140px;
        height:38px;
        background:rgba(255,255,255,1);
        border:1px solid rgba(223, 223, 223, 1);
        border-radius:5px;
        padding: 0 11px;
        margin-right: 10px;
        &::placeholder{
          color: #999999;
        }
      }
      p{
        display: flex;
        align-items: center;
        justify-content: center;
        font-size:16px;
        font-family:Microsoft YaHei;
        font-weight:400;
        color:rgba(255,255,255,1);
        &:hover{
          cursor: pointer;
        }
        &:active{
          opacity: 0.5;
        }
      }
      p:nth-of-type(1){
        width:100px;
        height:38px;
        background:rgba(228,57,60,1);
        border-radius:4px;
        margin-right: 10px;
        line-height:24px;

      }
      p:nth-of-type(2){
        width:100px;
        height:38px;
        background:rgba(39,120,190,1);
        border-radius:4px;

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
        flex:0.8;
      }
      p:nth-of-type(3){
        flex:1;
      }
      p:nth-of-type(4){
        flex:1;
      }
      p:nth-of-type(5){
        flex:1.5;

      }
      p:nth-of-type(6){
        flex:1;
        text-align: center;
      }

      p:nth-of-type(7){
        flex:1;

      }
      p:nth-of-type(8){
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
        p:nth-of-type(1){
          flex:0.8;
          text-align: center;
        }
        p:nth-of-type(2){
          flex:0.8;
        }
        p:nth-of-type(3){
          flex:1;
        }
        p:nth-of-type(4){
          flex:1;
        }
        p:nth-of-type(5){
          flex:1.5;
        }
        p:nth-of-type(6){
          flex:1;
        }
        p:nth-of-type(7){
          flex:1;
        }
        p:nth-of-type(8){
          flex:1;
          text-align: center;
          display: flex;
          align-items: center;
          justify-content: center;

          span:nth-of-type(1),span:nth-of-type(3){
            color: #2778BE;
            &:hover{
              cursor: pointer;
            }
          }
          span:nth-of-type(2){
            color: rgba(0,0,0,0.2);
            font-size: 12px;
            margin: 0 5px;

          }
        }


      }


    }

    /*增加子账号弹框*/
    .addPop{
      position: fixed;
      top: 0;
      left: 0;
      right: 0;
      bottom: 0;
      background: rgba(0,0,0,0.1);
      z-index: 9999;

      .pop-cont{
        position: absolute;
        top: 50%;
        left: 50%;
        transform: translate(-50%,-50%);
        width:691px;
        height:80vh;
        background:rgba(255,255,255,1);
        border-radius:8px;
        display: flex;
        display: -webkit-flex;
        flex-direction: column;
        .addPopMain{
          flex: 1;
          overflow: auto;
          padding-bottom: 20px;
        }
        .del{
          position: absolute;
          width: 40*100/1080vh;
          height: 40*100/1080vh;
          top:23*100/1080vh;
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
          padding: 0px 29px;
          width: 100%;
          display: flex;
          div:nth-of-type(1){
            margin-top: 29*100/1080vh;
            /*width: 69px;*/
            margin-right: 40px;
            /*flex-shrink: 0;*/
          }
          div:nth-of-type(2){
            display: flex;
            flex-wrap: wrap;
            flex: 1;
            .el-checkbox{
              width: 85px;
            }
          }
        }
        .p-line3 {
          margin-top: 30*100/1080vh;
          padding: 0 29px;
          width: 100%;
          display: flex;
          align-items: center;
          .p-line3-l{
            width: 69px;
            margin-right: 30px;
            flex-shrink: 0;
          }

          .p-line3-r{
            width: 500px;
            input{
              width: 100%;
              padding: 20px;
              resize: none;
              height: 52*100/1080vh;
              border:1px solid rgba(221, 221, 221, 1);
              color: #333333;
            }
            input::placeholder{
              color: #666666;
            }
          }
        }

        .p-line4 {
          margin-top: 42*100/1080vh;
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
          margin-top: 42*100/1080vh;
          padding: 0 29px;
          width: 100%;
          display: flex;

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
              height:52px;
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
          padding: 0px 29px;
          width: 100%;
          display: flex;
          div:nth-of-type(1){
            margin-top: 29*100/1080vh;
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
          padding: 0px 29px;
          width: 100%;
          display: flex;
          margin-top: 30*100/1080vh;
          div:nth-of-type(1){
            margin-top: 29*100/1080vh;
            width: 69px;
            margin-right: 30px;
            flex-shrink: 0;
          }
          div:nth-of-type(2){
            width: 100%;
            img{
              width:205px;
              height:156px;
              border-radius:8px;
            }
          }
        }
        .p-line8{
          padding: 0px 29px;
          width: 100%;
          display: flex;
          div:nth-of-type(1){
            margin-top: 29*100/1080vh;
            width: 69px;
            margin-right: 40px;
            flex-shrink: 0;
          }
          div:nth-of-type(2){
            display: flex;
            flex-wrap: wrap;
            /*flex: 1;*/
            .el-checkbox{
              width: 85px;
            }
          }
        }

      }
    }
  }

  .table{
    .el-table__header thead{
      color: #666666;
      font-size:14px;
    }
    .el-table__header th{
      background-color: #F7F7F7;

    }
  }



</style>
