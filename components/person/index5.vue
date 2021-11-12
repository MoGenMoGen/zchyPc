<template>
  <div class="left1">
    <offer  :applyInfo="applyInfo" v-if="offer" @close="close"></offer>
    <bail :applyInfo="applyInfo" :bail="bail" @close="close2"></bail>
    <download :download1="download1" @toClose="toClose" :applyInfo="applyInfo"></download>
    <!--投标管理-->
    <div class="message" >
      <div class="title">
        <p><img src="@/assets/img/personal/首页/投标管理.png" alt=""/>投标管理<span class="more" @click="toPage('./tender?cdType=1')">查看更多<i class="el-icon-arrow-right"></i></span></p>
      </div>
      <div class="content1">
        <el-table :data="bidList" style="width: 100%">
          <el-table-column type="index" align="center" min-width="150" label="序号">
          </el-table-column>
          <el-table-column align="center" min-width="200" label="项目信息">
            <template slot-scope="scope">
              <p>{{scope.row.nm}}</p>
              <p>项目编号：{{scope.row.cd}}</p>
            </template>
          </el-table-column>
          <el-table-column width="80" prop="budget" align="center" label="采购金额">
          </el-table-column>
          <el-table-column width="110" prop="bidOpenTm" align="center" label="开标时间">
          </el-table-column>
          <el-table-column width="110" align="center" label="状态">
            <template slot-scope="scope">
              <p v-if="scope.row.bidDecideTm" style="color: #E4393C;">已定标</p>
              <p v-else-if="returnDate(1,scope.row.bidOpenTm)" style="color: #2778BE;">评标中</p>
              <p v-else-if="returnDate(2,scope.row.bidEndTm)" style="color: #3FAB2E;">投标中</p>
              <p v-else style="color: #E4393C;">待开标</p>
            </template>
          </el-table-column>
          <el-table-column align="center" width="110" fixed="right" prop="operations" label="操作">
            <div class="btnList" slot-scope="scope">
              <button class="button3" v-if="scope.row.depositStatus==2" @click="openBail(scope.row)" style="font-size: 12px;">保证金上传</button>
              <button class="button3" v-if="(scope.row.depositStatus==1||scope.row.depositStatus==3)&&!scope.row.bidDecideTm&&returnDate(2,scope.row.bidEndTm)&&!scope.row.offer" @click="openOffer(scope.row)">资料上传</button>
              <button class="button3" v-if="(scope.row.depositStatus==1||scope.row.depositStatus==3)&&!scope.row.bidDecideTm&&returnDate(2,scope.row.bidEndTm)&&scope.row.offer" @click="openOffer(scope.row)">查看报价</button>
              <button class="button3" v-if="scope.row.signin.shipBidSigninVo.signinStatus==0&&!scope.row.bidDecideTm&&returnDate(1,scope.row.bidOpenTm)" @click="sign(scope.row)">签到</button>
              <p v-if="scope.row.signin.shipBidSigninVo.signinStatus==1&&!scope.row.bidDecideTm&&returnDate(1,scope.row.bidOpenTm)">已签到</p>
              <button class="button4" @click="toDetail(scope.row)">查看详情</button>
            </div>
          </el-table-column>
        </el-table>
      </div>
    </div>
    <!--管理船舶-->
    <div class="manage">
      <div class="title">
        <p><img src="@/assets/img/personal/首页/管控船舶.png" alt=""/>管控船舶<span class="more" @click="toPage('./shipControl')">查看更多<i class="el-icon-arrow-right"></i></span></p>
      </div>
      <div class="content1">
        <el-table
          :data="manageList"
          style="width: 100%">
          <el-table-column
            prop="cd"
            width="160"
            label="船舶编号">
          </el-table-column>
          <el-table-column
            prop="nm"
            width="200"
            label="船舶名称">
          </el-table-column>
          <el-table-column
            width="100"
            prop="productName"
            align="center"
            label="客户名称">
          </el-table-column>
          <el-table-column
            prop="statusNm"
            width = "85"
            align="center"
            label="船舶状态">
          </el-table-column>
          <el-table-column
            align="center"
            width="90"
            fixed="right"
            prop="operations"
            label="操作">
              <template slot-scope="scope">
                <div class="btnList">

                <button class="button3" @click="toDetail2(scope.row,'1')" v-if="scope.row.statusNm!='已交船'">新增</button>
                <button class="button2" @click="toDetail2(scope.row,'2')">查看详情</button>
                </div>

              </template>
          </el-table-column>
        </el-table>
      </div>
    </div>
  </div>
</template>

<script>
  import {mapState} from "vuex";

  import Offer from "@/components/person/offer";
  import bail from "../../components/person/bail";
  import Download from "@/components/person/download";
  export default {
    name: "shipowner",
    components: {
      Download,
      Offer,bail },
    data(){
      return {
        bail: false,
        offer: false,
        download1: false,
        applyInfo:{},
        identityCd: '',
        currentRoleId: '',
        nowDate: '',
        manageList: [{
          no: 'ZCHY20200515',
          nm: '新型 4000KW 中型休闲...',
          cl: '中创海洋',
          state: '建造中',
        }, {
          no: 'ZCHY20200515',
          nm: '新型 4000KW 中型休闲...',
          cl: '中创海洋',
          state: '建造中',
        }, {
          no: 'ZCHY20200515',
          nm: '新型 4000KW 中型休闲...',
          cl: '中创海洋',
          state: '已完成',
        }],
        bidApplyInfo:{
          orgId: "5024282848367616",
          orgNm: "中创海洋科技股份有限公司",
          bidId: "5031217130673152",
          bidNm: "测试一个招标项目",
          rmks: "",
          attachment: "https://sinovat.oss-cn-shanghai.aliyuncs.com/c6178ce400fc4c048a9b4328cc92a878.pdf"
        },
        bidList:[],//投标管理列表
        adertList:[],
        bidData:{
          orgEnterId:'',
          identityCd:'',
        }
      }

    },
    computed:{
      ...mapState([
        "currentRole",
      ]),
      returnDate() {
        return (type,date) => {
          if(type==1) {
            if((new Date(date)).getTime()<this.nowDate){
              return true
            } else {
              return false
            }
          } else if(type==2) {
            if((new Date(date)).getTime()>this.nowDate){
              return true
            } else {
              return false
            }
          }

        }
      }
    },
    watch:{
      currentRole(){
        this.identityCd = JSON.parse(this.until.seGet('currentRole')).identityCd
        this.currentRoleId = JSON.parse(this.until.seGet('currentRole')).id
        this.getBidData()
      },
    },
    mounted(){
      this.nowDate = (new Date()).getTime()
      if(this.currentRole){
        this.bidData.orgEnterId = this.currentRole.id;
        this.bidData.identityCd = this.currentRole.identityCd
      }
      this.identityCd = JSON.parse(this.until.seGet('currentRole')).identityCd
      this.currentRoleId = JSON.parse(this.until.seGet('currentRole')).id
      this.getData()
    },
    methods:{
      async getData(){
        this.getBidData()
        this.getList()
      },
      //投标列表
      async getBidData(){
        this.bidData.orgEnterId = this.currentRole.id;
        this.bidData.identityCd = this.currentRole.identityCd
        let qry = this.query.new()
        this.query.toP(qry,'1','3')
        this.query.toO(qry,'publishTm','desc')
        let data = await this.api.bidManage(this.query.toEncode(qry),this.bidData)
        this.bidList = data.data.list
        this.bidList.forEach(item =>{
          item.publishTm = item.publishTm.split(' ')[0];
          item.completeTm = item.completeTm.split(' ')[0];
          if(!item.statusNm){
            item.statusNm = '待报名'
          }
        })

        //根据时间添加disable属性
        for(var i=0; i<this.bidList.length;i++){ // 添加disable属性并设置为false
          this.$set(this.bidList[i], 'disable', 'false')
        }
        this.bidList.forEach(item => {
          if(this.until.TimeStep(item.completeTm) >= 0){
            item.disable = true;
          }
        });
      },
      //管控船舶列表
      async getList(){
        let qry = this.query.new()
        this.query.toP(qry,1,3)

        let param={
          orgEnterId:this.currentRole.id
        }
        let data=await this.api.allShip(this.query.toEncode(qry),param)
        this.manageList=data.data.list
      },
      toPage(url){
        if(url){
          this.$router.push(url)
        }
      },
      //投标详情
      toDetail(row) {
        this.$router.push({
          path: './bidAfficheDetail',
          query: {
            id: row.bidId,
          }
        })
      },
      //管控船舶详情
      toDetail2(item,type){
        if(type=='1'){
          this.$router.push('./shipFileDetail?id='+item.id+"&cdType=3&formW=jianli&cdType2=2&shipCd="+item.cd+"&shipStatus="+item.statusNm)
        }else if(type=='2'){
          this.$router.push('./archives?id='+item.id+"&shipCd="+item.cd+"&shipStatus="+item.statusNm)
        }
      },
      indexMethod(index) {
        return index +1;
      },
      openBail(row) {
        this.bail = true
        this.applyInfo = row
      },
      openOffer(row) {
          this.offer = true
          this.applyInfo = row
      },
      close() {
        this.offer = false;
        this.getBidData()
      },
      close2() {
        this.bail = false;
        this.getBidData()
      },
      getBidData() {
        let qry = this.query.new()
        this.query.toO(qry, 'publishTm', 'desc')
        this.query.toP(qry, 1, 3)
        // this.query.toW(qry, 'viewRangeCd', this.identityCd+'', 'LK')
        this.api.getMyBidList(this.query.toEncode(qry),this.currentRoleId).then(res => {
          this.bidList = res.data.list
          this.total = res.page.total
        })
      },
      sign(row) {
        let id = row.signin.shipBidSigninVo.id
        this.api.bidSign(id).then(res => {
          this.$message.success('签到成功')
        })
      }

    },
  }
</script>

<style lang="less">
  .content1{
    .btnList{
      display: flex;
      justify-content: space-around;
      flex-direction: column;
      .button3{
        width: 74px;
        height: 31px;
        background-color: #2778BE;
        color: #FFFFFF;
        border: none;
        border-radius: 3px;
        margin-bottom: 15px;
        cursor: pointer;
      }
      .disable{
        background-color: #999999;
      }
      .button4{
        border: none;
        width: 74px;
        height: 31px;
        color: #2778BE;
        font-size: 15px;
        background-color: #FFFFFF;
        margin-top: 5px;
        border-radius: 3px;
        cursor: pointer;
      }
      .button2{
        width: 74px;
        height: 31px;
        background-color: #FFFFFF;
        color: #2778BE;
        border-radius: 3px;
        border: 1px solid #2778BE;
        cursor: pointer;

      }
    }
    .el-table__header thead{
      color: #666666;
      font-size:14px;
    }
    .el-table__header th{
      background-color: #F7F7F7;

    }
    .el-table__body tr{
      color: #333333;
      font-size:15px;
      height: 117px;
    }
    .el-table__fixed-right{
      height: 100%!important;
    }
  }

  .message{
    input::-webkit-input-placeholder {
      /* Chrome/Opera/Safari */
      color: #999;
    }

    input::-moz-placeholder {
      /* Firefox 19+ */
      color: #999;
    }

    input:-ms-input-placeholder {
      /* IE 10+ */
      color: #999;
    }

    input:-moz-placeholder {
      /* Firefox 18- */
      color: #999;
    }
    textarea::-webkit-input-placeholder {
      /* Chrome/Opera/Safari */
      color: #999;
    }

    textarea::-moz-placeholder {
      /* Firefox 19+ */
      color: #999;
    }

    textarea:-ms-input-placeholder {
      /* IE 10+ */
      color: #999;
    }

    textarea:-moz-placeholder {
      /* Firefox 18- */
      color: #999;
    }
  }
    #resize{
      resize: none !important;
      height: 92px;
    }
    #btnBA{
      display: inline-flex;
      width: 100%;
      .el-form-item__content{
        display: inline-flex;
        justify-content: center;
        width: 100%;
      }
      #buttonA{
        width: 46%;
        height: 42px;
        border: 1px solid #2778BE;
        color: #FFFFFF;
        font-size: 16px;
        background-color: #2778BE;
        cursor: pointer;
        margin-left: 30px;
      }
      #buttonB{
        width: 46%;
        height: 42px;
        border: 1px solid #DDDDDD;
        color: #666666;
        font-size: 16px;
        background-color: #FFFFFF;
        cursor: pointer;
      }
    }

</style>
<style lang="less" scoped>
  @import url("../../assets/css/init.less");
  .left1{
    width: 67.3%;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    padding-top: 31px;

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
    //投标管理
    .message{
      width: 100%;
      background-color: #FFFFFF;
      min-height: 445px;
    }
    /*监理监造机构  管理船舶*/
    .manage{
      width: 100%;
      background-color: #FFFFFF;
      margin-top: 24px;
      min-height: 434px;
    }
  }
</style>
