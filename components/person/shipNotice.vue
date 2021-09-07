<template>
<!--  维保记录-->
    <div>
      <!--节点通知-->
      <div v-if="noticeShow" class="pop" >
        <div class="pop-cont">
          <p><span>节点通知</span>
            <img :src="deleteIc" @click="noticeShow=false">
          </p>

          <div class="popMain">
            <div><p>标题：</p><input v-model="noticeContent.title"/></div>
            <div><p>类型：</p><el-select v-model="noticeContent.typesCd" placeholder="请选择" clearable>
              <el-option
                v-for="item in noticeType"
                :key="item.id"
                :label="item.nm"
                :value="item.cd">
              </el-option>
            </el-select></div>
            <div><p>内容：</p><textarea v-model="noticeContent.cont"></textarea></div>
            <div>
              <p>通知人员：</p>
              <el-select v-model="noticeContent.idsList" multiple collapse-tags placeholder="请选择">
                  <el-option
                    v-for="item in userList"
                    :key="item.id"
                    :label="item.nickname"
                    :value="item.id">
                  </el-option>
                </el-select>
            </div>
            <div>
              <p>是否通知船东：</p>
              <el-radio v-model="noticeContent.isShipOwn" label="true">通知</el-radio>
              <el-radio v-model="noticeContent.isShipOwn" label="false">不通知</el-radio>
            </div>
            <!--<div><p>备注：</p><textarea v-model="noticeContent.rmks"></textarea></div>-->
          </div>
          <p>
            <span @click="noticeShow=false">取消</span>
            <span @click="noticeSumbit">确认</span>
          </p>
        </div>
      </div>
    </div>
</template>

<script>
  import deleteIc from '@/assets/img/personal/delete.png'
    export default {
        name: "weibao",

        data(){
            return{
              deleteIc,
              noticeType:[],
              noticeContent:{
                title:'',
                typesCd:'',
                cont:'',
                idsList:[],
                isShipOwn:'false',
                docsId:''
              },
              noticeShow:false,
              userList:[],
            }
        },
        watch:{
          isShipOwn(e){
            console.log(e)
          }
        },
        async mounted() {
          this.noticeType = await this.api.dataDictionary('DOCS_NOTICE_TYPE')
          this.getList()
        },
        methods:{
          async getList(){
            let qry = this.query.new()
            this.query.toW(qry,'type','1','EQ')
            this.userList=await this.api.adminUserList(this.query.toEncode(qry))
            console.log('管理员列表')
            console.log(this.userList)
          },
          //节点通知
          toNotice(item){
            this.noticeContent = {
              title:item.nm+' '+item.cd,
              cont: "",
              typesCd: '',
              idsList:[],
              isShipOwn:'false',
              docsId:item.id
            }
            this.noticeShow = true
          },
          //节点通知提交
          noticeSumbit(){
            if(!this.noticeContent.typesCd){
              this.$message({
                message: '请选择类型',
                type: 'warning'
              });
              return
            }
            if(this.noticeContent.idsList.length<1){
              this.$message({
                message: '请选择通知的人员',
                type: 'warning'
              });
              return
            }
            this.api.shipNotice(this.noticeContent).then(()=>{
              this.$message({
                message: '提交成功',
                type: 'success',
                duration:'1000'
              });
              this.noticeShow = false
            })
          },
        },
    }
</script>

<style scoped lang="less">
  .pop{
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    background: rgba(0,0,0,0.1);
    z-index: 99;
    .pop-cont{
      position: absolute;
      top: 50%;
      left: 50%;
      transform: translate(-50%,-50%);
      width:500px;
      height:500px;
      background:rgba(255,255,255,1);
      border-radius:8px;
      display: flex;
      display: -webkit-flex;
      flex-direction: column;
      padding-bottom: 20px;
      .popMain{
        flex: 1;
        overflow-y: scroll;
        height: 100%;
        padding: 10px 0;
        >div{
          display: flex;
          display: -webkit-flex;
          padding: 10px 20px;
          line-height: 40px;
          align-items: center;
          p{
            width: 100px;
            text-align: right;
          }

          input{
            height: 40px;
            text-indent: 10px;
          }
          input,textarea{
            flex: 1;
            border: 1px solid #e0e0e0;
            border-radius: 3px;
          }
          textarea{
            line-height: 30px;
            padding: 10px;
          }
        }
      }
      >p:nth-of-type(1){
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

      >p:nth-of-type(2){
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
</style>
