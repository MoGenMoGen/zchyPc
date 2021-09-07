<template>
  <div class="left1">
    <dispatch :show="show" @toClose="toClose" :info="currentInfo" :manList="manList" @confirm="dispatchConfirm"></dispatch>
    <service-order :show="show2" @close="close" :id="currentInfo.id" @confirm="serviceOrderConfirm"></service-order>
    <service-order-detail  :show="show3" @close="show3=false" :id="currentInfo.id"></service-order-detail>
    <!--维保管理-->
    <div class="message" >
      <div class="title">
        <p><img src="@/assets/img/personal/首页/投标管理.png" alt=""/>维保管理<span class="more" @click="toPage('./repair')">查看更多<i class="el-icon-arrow-right"></i></span></p>
      </div>
      <div class="content1">
        <el-table
          :data="list"

          style="width: 100%">
          <el-table-column
            align="left"
            width="140"
            prop="cd"
            label="服务单号">
          </el-table-column>
          <el-table-column
            prop="linkman"
            align="left"
            width="120"
            label="联系人姓名">
          </el-table-column>
          <el-table-column
            prop="docsCd"
            align="left"
            width="140"
            label="船舶编号">
          </el-table-column>
          <el-table-column
            prop="serviceItemNm"
            align="center"
            label="服务内容">
          </el-table-column>
          <el-table-column
            prop="statusNm"
            align="left"
            label="状态">
          </el-table-column>
          <el-table-column
            align="center"
            fixed="right"
            prop="operations"
            label="操作">
            <div class="btnList" slot-scope="scope">
              <el-button type="text" @click="toDetail(scope.row)" size="small" class="toEva">查看详情</el-button>
              <button v-if="scope.row.statusCd=='TASK_NOTE_STATUS.40' || scope.row.statusCd=='TASK_NOTE_STATUS.50' " size="small" plain class="evalutaed" @click="toDispatch(scope.row)">派单</button>
              <button v-if="scope.row.statusCd=='TASK_NOTE_STATUS.60'" size="small" plain class="evalutaed" @click="toFinish(scope.row)">确认完成</button>
            </div>
          </el-table-column>
        </el-table>
      </div>
    </div>
    <!--在线留言-->
    <div class="message1" >
      <div class="title">
        <p><img src="@/assets/img/personal/首页/留言.png" alt=""/>在线留言</p>
      </div>
      <div class="content1">
        <el-form ref="ruleForm" :model="ruleForm" label-width="100px" :rules="rules" :label-position="labelPosition">
          <el-form-item label="留言内容：" prop="cont">
            <el-input placeholder="可在线提交您的宝贵意见，我们会尽力做到更好。" type="textarea" row="3" id="resize" v-model="ruleForm.cont"></el-input>
          </el-form-item>
          <el-form-item label="联系人：" prop="linkman">
            <el-input placeholder="请输入联系人" v-model="ruleForm.linkman"></el-input>
          </el-form-item>
          <el-form-item label="联系电话：" prop="phone">
            <el-input placeholder="请输入联系电话"  v-model="ruleForm.phone"></el-input>
          </el-form-item>
          <el-form-item label="联系邮箱：" prop="email">
            <el-input placeholder="请输入联系邮箱" v-model="ruleForm.email"></el-input>
          </el-form-item>
          <el-form-item id="btnBA">
            <el-button id="buttonB" @click="resetForm('ruleForm')">取消</el-button>
            <el-button id="buttonA" type="primary" @click="submitForm('ruleForm')">提交</el-button>
          </el-form-item>
        </el-form>
      </div>
    </div>
  </div>
</template>

<script>
  import {mapState} from "vuex";
  import img1 from '@/assets/img/alter.png'
  import Dispatch from "./Dispatch";
  import ServiceOrder from "./serviceOrder";
  import serviceOrderDetail from "../../components/person/serviceOrderDetail";
  export default {
    name: "shipowner",
    components: {
      ServiceOrder,
      Dispatch,
      serviceOrderDetail
    },
    data(){
      var checkTel = (rule, value, callback) => {
        let regPhone = /^(((13[0-9]{1})|(14[0-9]{1})||(15[0-9]{1})|17[0-9]{1}|(18[0-9]{1}))+\d{8})$/;
        if (!value) {
          return callback(new Error('手机号码不能为空！'));
        }else {
          setTimeout(() => {
            if (!regPhone.test(value)) {
              return callback(new Error('请填写正确的手机号！'));
            }else {
              return callback()
            }
          }, 1000);
        }
      };
      return {
        manList:[],//派单人员
        currentInfo:{},
        number: 0,
        imgList: [{
          nm: '锚泊设备',
          img: img1,
        }, {
          nm: '系泊设备',
          img: img1,
        }, {
          nm: '锚泊设备',
          img: img1,
        }, {
          nm: '系泊设备',
          img: img1,
        }],
        list:[{
          cd:'WBFW0000001',
          nm:'王先生',
          cid:'202001ABC00',
          content:'安装调试',
          statusNm:'待办工单',
        }],
        ruleForm: {
          cont: '',
          linkman: '',
          phone: '',
          email: '',
        },
        rules: {
          linkman: [
            {required: true, message: '请输入联系人', trigger: 'blur'},
          ],
          phone: [
            {required: true, validator: checkTel, trigger: 'blur'},
          ],
          email: [
            {required: true, message: '请输入联系邮箱', trigger: 'blur'},
          ],
          cont: [
            {required: true, message: '请填写留言内容', trigger: 'blur'}
          ]
        },
        labelPosition: 'left',
        show:false,//派单弹窗
        show2:false,//确认完成
        show3:false,//详情窗口
      }

    },
    computed:{
      ...mapState([
        'bWidth',
        'width',
        "currentRole",
        'loading',
        'tel',
        'msgNum'
      ])
    },
    watch:{

    },
    mounted(){

      this.getData()
    },
    methods:{
      async getData(){
        this.manList = await this.api.toRepairMan(this.currentRole.id)
        this.getList()
      },
      async getList(){
        let qry = this.query.new()
        console.log(this.currentRole)
        let param={
          orgId:this.currentRole.id
        }
        this.query.toO(qry,'crtTm','desc')
        this.query.toP(qry,1,3)
        let data=await this.api.maintenR(this.query.toEncode(qry),param)
        this.total=data.page.total
        data.data.list.forEach((item,index)=>{
          item.no=index+1
          item.crtTm = item.crtTm.split(' ')[0]

        })
        this.list = data.data.list
      },
      //派单
      toDispatch(e){
        this.currentInfo = e
        this.show = true;
      },
      //完成确认
      toFinish(item){
        this.currentInfo = item
        this.show2=true
      },
      //派单确认
      dispatchConfirm(){
        this.show = false
        this.getList()
      },
      //派单确认完成
      serviceOrderConfirm(){
        this.show2=false
        this.getList()
      },
      //查看详情
      toDetail(item){
        this.currentInfo = item
        this.show3=true
      },
      toPage(url){
        if(url){
          this.$router.push(url)
        }
      },
      toClose(){
        this.show = false
      },
      close(){
        this.show2 = false
      },
      resetForm(formName) {
        this.$refs[formName].resetFields();
      },
      submitForm(formName){
        this.$refs[formName].validate((valid) => {
          console.log(valid)
          if (valid) {
            this.store.commit('changeLoading',true)

            this.api.msgAdd(this.ruleForm).then(res=>{
              console.log(res)
              this.$message({
                message: '留言成功',
                type: 'success',
                duration:'1500'
              });
              setTimeout(()=>{
                this.toPage('./')
              },1500)
            })
          } else {
            console.log('error submit!!');
            return false;
          }
        });
      },

    },
  }
</script>

<style lang="less" scoped>
  @import url("../../assets/css/init.less");
  .left1{
    width: 67.3%;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    padding-top: 31px;

    //在线留言板块
    .message1 {
      width: 100%;
      background-color: #FFFFFF;
      .title {
        width: 100%;
        height: 56px;
        border-bottom: 1px solid #F3F3F3;
        .more {
          float: right;
          font-size: 12px;
          color: #666666;
          margin-right: 30px;
          cursor: pointer;

        }
        > p {
          font-size: 16px;
          color: #333333;
          line-height: 56px;
          margin-left: 20px;
        }
        img {
          padding-right: 10px;
          height: 19px;
          vertical-align: sub;
        }
      }
      .content1 {
        height: 100%;
        padding: 20px 110px 30px 80px;
      }
    }
    //维保服务
    .message{
      width: 100%;
      background-color: #FFFFFF;
      margin-bottom: 24px;
      .title{
        width: 100%;
        height: 56px;
        border-bottom: 1px solid #F3F3F3;
        .more{
          float: right;
          font-size: 12px;
          color: #666666;
          margin-right: 30px;
          cursor: pointer;

        }
        >p{
          font-size: 16px;
          color: #333333;
          line-height: 56px;
          margin-left: 20px;
        }
        img{
          padding-right: 10px;
          height: 19px;
          vertical-align: sub;
        }
      }
      .content1{
        height: 100%;
        min-height: 413px;
        .btnList{
          >button{
            background-color: #2878BE;
            color: #FFFFFF;
            width: 82px;
            margin-bottom: 5px;
            height: 35px;
            border: none;
            border-radius: 2px;
            cursor: pointer;
          }
        }
      }
    }

  }



</style>
