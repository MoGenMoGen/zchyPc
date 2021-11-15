<template>
  <div class="left1">
    <!--报价-->
    <offer  :applyInfo="applyInfo" v-if="offer" @close="close"></offer>
    <bail  :applyInfo="applyInfo" v-if="bail" @close="close"></bail>
    <!--附件下载-->
    <download :download1="download1" @toClose="toClose" :applyInfo="applyInfo"></download>
    <!--采购订单-->
    <div class="productOrder">
      <div class="title">
        <p><img src="@/assets/img/personal/首页/产品订单.png" alt=""/>采购订单</p>
      </div>
      <div class="content">
        <div class="section" @click="toPage('./myOrder2?cdType=1&statusCd=1')">
          <div class="partA">
            <img src="@/assets/img/personal/首页/待付款.png" alt=""/>
            <span v-if="this.number != 0" class="span">{{number}}</span>
          </div>
          <p>待付款</p>
        </div>
        <div class="section"  @click="toPage('./myOrder2?cdType=1&statusCd=2')">
          <div class="partA">
            <img src="@/assets/img/personal/首页/待发货.png" alt=""/>
            <span v-if="this.number != 0" class="span">{{number}}</span>
          </div>
          <p>待发货</p>
        </div>
        <div class="section" @click="toPage('./myOrder2?cdType=1&statusCd=3')">
          <div class="partA">
            <img src="@/assets/img/personal/首页/待收货.png" alt=""/>
            <span v-if="this.number != 0"  class="span">{{number}}</span>
          </div>
          <p>待收货</p>
        </div>
        <div class="section" @click="toPage('./myOrder2?cdType=1&statusCd=4')">
          <div class="partA">
            <img src="@/assets/img/personal/首页/已完成.png" alt=""/>
            <span v-if="this.number != 0"  class="span">{{number}}</span>
          </div>
          <p>已完成</p>
        </div>
        <div class="section"  @click="toPage('./myOrder2?cdType=1')">
          <div class="partA">
            <img src="@/assets/img/personal/首页/全部订单.png" alt=""/>
            <span v-if="this.number != 0"  class="span">{{number}}</span>
          </div>
          <p>全部订单</p>
        </div>
      </div>
    </div>
    <!--设计销售订单-->
    <div class="shipOrder">
      <div class="title">
        <p><img src="@/assets/img/personal/首页/产品订单.png" alt=""/>设计销售订单</p>
      </div>
      <div class="content">
        <div class="section"  @click="toPage('./myOrder2?cdType=21&statusCd=1')">
          <div class="partA">
            <img src="@/assets/img/personal/首页/待付款.png" alt=""/>
            <span v-if="this.number != 0"  class="span">{{number}}</span>
          </div>
          <p>待付款</p>
        </div>
        <div class="section" @click="toPage('./myOrder2?cdType=21&statusCd=2')">
          <div class="partA">
            <!--图片需更换-->
            <img src="@/assets/img/personal/首页/设计中.png" alt=""/>
            <span v-if="this.number != 0" class="span">{{number}}</span>
          </div>
          <p>进行中</p>
        </div>
        <div class="section"  @click="toPage('./myOrder2?cdType=21&statusCd=4')">
          <div class="partA">
            <img src="@/assets/img/personal/首页/已完成.png" alt=""/>
            <span v-if="this.number != 0" class="span">{{number}}</span>
          </div>
          <p>已完成</p>
        </div>
        <div class="section"  @click="toPage('./myOrder2?cdType=21')">
          <div class="partA">
            <img src="@/assets/img/personal/首页/全部订单.png" alt=""/>
            <span v-if="this.number != 0"  class="span">{{number}}</span>
          </div>
          <p>全部订单</p>
        </div>
      </div>
    </div>
    <!--设计采购订单-->
    <div class="shipOrder">
      <div class="title">
        <p><img src="@/assets/img/personal/首页/产品订单.png" alt=""/>设计采购订单</p>
      </div>
      <div class="content">
        <div class="section"  @click="toPage('./myOrder2?cdType=22&statusCd=1')">
          <div class="partA">
            <img src="@/assets/img/personal/首页/待付款.png" alt=""/>
            <span v-if="this.number != 0"  class="span">{{number}}</span>
          </div>
          <p>待付款</p>
        </div>
        <div class="section" @click="toPage('./myOrder2?cdType=22&statusCd=2')">
          <div class="partA">
            <!--图片需更换-->
            <img src="@/assets/img/personal/首页/设计中.png" alt=""/>
            <span v-if="this.number != 0" class="span">{{number}}</span>
          </div>
          <p>进行中</p>
        </div>
        <div class="section"  @click="toPage('./myOrder2?cdType=22&statusCd=4')">
          <div class="partA">
            <img src="@/assets/img/personal/首页/已完成.png" alt=""/>
            <span v-if="this.number != 0" class="span">{{number}}</span>
          </div>
          <p>已完成</p>
        </div>
        <div class="section"  @click="toPage('./myOrder2?cdType=22')">
          <div class="partA">
            <img src="@/assets/img/personal/首页/全部订单.png" alt=""/>
            <span v-if="this.number != 0"  class="span">{{number}}</span>
          </div>
          <p>全部订单</p>
        </div>
      </div>
    </div>
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
              <button class="button3" v-if="scope.row.depositStatus==3" @click="openBail(scope.row)" style="font-size: 12px;">查看保证金</button>
              <button class="button3" v-if="(scope.row.depositStatus==1||scope.row.depositStatus==3)&&!scope.row.bidDecideTm&&returnDate(2,scope.row.bidEndTm)&&!scope.row.offer" @click="openOffer(scope.row)">资料上传</button>
              <button class="button3" v-if="(scope.row.depositStatus==1||scope.row.depositStatus==3)&&!scope.row.bidDecideTm&&returnDate(2,scope.row.bidEndTm)&&scope.row.offer" @click="openOffer(scope.row)">查看资料</button>
              <button class="button3" v-if="scope.row.signin.shipBidSigninVo.signinStatus==0&&!scope.row.bidDecideTm&&returnDate(1,scope.row.bidOpenTm)" @click="sign(scope.row)">签到</button>
              <p v-if="scope.row.signin.shipBidSigninVo.signinStatus==1&&!scope.row.bidDecideTm&&returnDate(1,scope.row.bidOpenTm)">已签到</p>
              <button class="button4" @click="toDetail(scope.row)">查看详情</button>
            </div>
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
        number: 0,
        offer: false,
		bail: false,
        download1: false,
        applyInfo:{},
		identityCd: '',
		currentRoleId: '',
		nowDate: '',
        bidApplyInfo:{  //报名参数
          orgId: "5024282848367616",
          orgNm: "中创海洋科技股份有限公司",
          bidId: "5031217130673152",
          bidNm: "测试一个招标项目",
          rmks: "",
          attachment: "https://sinovat.oss-cn-shanghai.aliyuncs.com/c6178ce400fc4c048a9b4328cc92a878.pdf"
        },
        bidList:[],//投标管理列表
        bidData:{  //投标管理列表请求参数
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
		this.identityCd = JSON.parse(this.until.seGet('currentRole')).identityCd
		this.currentRoleId = JSON.parse(this.until.seGet('currentRole')).id
      this.getBidData()
      this.nowDate = (new Date()).getTime()
    },
    methods:{
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
      indexMethod(index) {
        return index +1;
      },
      download(row){
        console.log(row)

        let url = `/general/oss/aliDownload?urls=${row.attachment}&zipName=${row.nm}`
        window.open(url)
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
    //产品订单
    .productOrder{
      width: 100%;
      /*height: 200px;*/
      background-color: #FFFFFF;
      margin-bottom: 24px;
      .title{
        width: 100%;
        height: 56px;
        border-bottom: 1px solid #F3F3F3;
        >p{
          font-size: 16px;
          color: #333333;
          line-height: 56px;
          margin-left: 20px;

        }
        img{
          padding-right: 10px;
          height: 19px;
          vertical-align: middle;
        }
      }
      //内容
      .content{
        width: 100%;
        display: inline-flex;
        .section{
          width: 20%;
          display: flex;
          flex-direction: column;
          justify-content: center;
          align-items: center;
          height: 162px;
          .partA{
            position: relative;
            .span{
              position: absolute;
              width: 14px;
              height: 14px;
              background-color: #E4393C;
              border-radius: 50%;
              left: 26px;
              color: #FFFFFF;
              font-size: 12px;
              text-align: center;
              line-height: 14px;
            }
            >img{
              cursor: pointer;
              width: 35px;
              padding-bottom: 10px;
            }
          }
          >p{
            font-size: 12px;
            color: #333333;
            cursor: pointer;
          }
        }
      }
    }
    //船舶订单
    .shipOrder{
      width: 100%;
      /*height: 200px;*/
      background-color: #FFFFFF;
      margin-bottom: 24px;
      .title{
        width: 100%;
        height: 56px;
        border-bottom: 1px solid #F3F3F3;
        >p{
          font-size: 16px;
          color: #333333;
          line-height: 56px;
          margin-left: 20px;
        }
        img{
          padding-right: 10px;
          height: 21px;
          vertical-align: middle;
        }
      }
      .content{
        width: 100%;
        display: inline-flex;
        justify-content: space-between;
        .section{
          width: 20%;
          display: flex;
          flex-direction: column;
          justify-content: center;
          align-items: center;
          height: 162px;
          .partA{
            position: relative;
            .span{
              position: absolute;
              width: 14px;
              height: 14px;
              background-color: #E4393C;
              border-radius: 50%;
              color: #FFFFFF;
              left: 26px;
              font-size: 12px;
              text-align: center;
              line-height: 14px;
            }
            >img{
              cursor: pointer;
              width: 35px;
              padding-bottom: 10px;
            }
          }
          >p{
            font-size: 12px;
            color: #333333;
            cursor: pointer;
          }
        }
      }
    }
    /*我的产品*/
    .myProduct{
      width: 100%;
      background-color: #FFFFFF;
      .title{
        width: 100%;
        height: 56px;
        border-bottom: 1px solid #F3F3F3;
        >p{
          font-size: 16px;
          color: #333333;
          line-height: 56px;
          margin-left: 20px;

        }
        img{
          padding-right: 10px;
          height: 19px;
          vertical-align: middle;
        }
      }
      //内容
      .content{
        width: 100%;
        display: inline-flex;
        .sectionA{
          width: 25%;
          display: flex;
          flex-direction: column;
          justify-content: center;
          align-items: center;
          height: 162px;
          >img{
            width: 84px;
          }
          >p{
            margin-top: 16px;
            font-size: 16px;
            color: #666666;
            cursor: pointer;
          }
        }
      }
    }
    /*经销商 客户档案*/
    .document{
      width: 100%;
      background-color: #FFFFFF;
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

      }
    }
    /*经销商 收款记录 */
    .money{
      width: 100%;
      background-color: #FFFFFF;
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

      }
    }
    /*跟进记录*/
    .record{
      width: 100%;
      background-color: #FFFFFF;
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
      .content2{
      }
    }
    //在线留言板块
    .message1{
      width: 100%;
      background-color: #FFFFFF;
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
        padding: 20px 110px 30px 80px;
      }
    }
    //在线留言板块
    .message{
      width: 100%;
      background-color: #FFFFFF;
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
        min-height: 180px;
        .format{
          width: 100%;
          display: inline-flex;
          justify-content: center;
          flex-direction: row;
          align-items: baseline;
          text-align: center;
          cursor: pointer;
          >p{
            font-size: 15px;
            color: #666666;
            width: 93px;
          }
          input{
            width: 348px;
            height: 42px;
            border: 1px solid #DDDDDD;
            margin-top: 20px;
            padding-left: 21px;
            color: #999999;
          }
          textarea{
            width: 348px;
            height: 92px;
            border: 1px solid #DDDDDD;
            margin-top: 20px;
            resize: none;
            padding: 21px;
            color: #999999;
          }
        }
        .button1{
          margin-top: 30px;
          width: 100%;
          padding-left: 92px;
          display: inline-flex;
          justify-content: center;
          padding-bottom: 30px;
          button:nth-child(1){
            width: 159px;
            height: 42px;
            border: 1px solid #DDDDDD;
            color: #666666;
            font-size: 16px;
            background-color: #FFFFFF;
            cursor: pointer;
          }
          button:nth-child(2){
            width: 159px;
            height: 42px;
            border: 1px solid #2778BE;
            color: #FFFFFF;
            font-size: 16px;
            background-color: #2778BE;
            margin-left: 31px;
            cursor: pointer;
          }
        }
        .disable{
          background-color: #999999;
        }
      }
    }
    /*监理监造机构  管理船舶*/
    .manage{
      width: 100%;
      background-color: #FFFFFF;
      margin-top: 24px;
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

      }
    }
  }


</style>
