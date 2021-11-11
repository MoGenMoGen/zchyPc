<template>
  <div class="left1">
    <offer :applyInfo="applyInfo" v-if="offer" @close="close"></offer>
    <bail :applyInfo="applyInfo" :bail="bail" @close="close2"></bail>
    <download
      :download1="download1"
      @toClose="toClose"
      :applyInfo="applyInfo"
    ></download>
    <!--整改单-->
    <div class="message">
      <div class="title">
        <p>
          <img
            src="@/assets/img/personal/首页/整改单图标.png"
            alt=""
          />整改单<span class="more" @click="toPage('./rectification')"
            >查看更多<i class="el-icon-arrow-right"></i
          ></span>
        </p>
      </div>
      <div class="content1">
        <el-table
          :data="rectifyList"
          :cell-style="status"
          style="width: 100%"
          @row-click="toRectifydetail"
        >
          <el-table-column
            type="index"
            :index="indexMethod"
            align="center"
            width="60"
            label="序号"
          >
          </el-table-column>
          <el-table-column prop="cd" width="150" label="整改单号">
          </el-table-column>
          <el-table-column
            width="200"
            prop="nm"
            align="center"
            label="整改内容"
          >
          </el-table-column>
          <el-table-column
            width="110"
            prop="issueTm"
            align="center"
            label="下发日期"
          >
          </el-table-column>
          <el-table-column
            prop="status"
            fixed="right"
            width="110"
            align="center"
            label="当前状态"
          >
            <template slot-scope="scope">
              <div
                v-if="scope.row.state == 2"
                style="color: #ff3c00; border: 1px solid #ff3c00"
              >
                待执行
              </div>
              <div
                v-else-if="scope.row.state == 3"
                style="color: #2778be; border: 1px solid #2778be"
              >
                待结案
              </div>
              <div v-else-if="scope.row.state == 4">已完成</div>
            </template>
          </el-table-column>
        </el-table>
      </div>
    </div>
    <!--云检验-->
    <div class="manage">
      <div class="title">
        <p>
          <img
            src="@/assets/img/personal/首页/云检验图标.png"
            alt=""
          />云检验<span class="more" @click="toPage('./allShipList')"
            >查看更多<i class="el-icon-arrow-right"></i
          ></span>
        </p>
      </div>
      <div class="content1">
        <el-table
          :data="yunCheckList"
          :cell-style="statusYun"
          style="width: 100%"
        >
          <el-table-column prop="cd" width="150" label="船舶编号">
          </el-table-column>
          <el-table-column
            width="200"
            prop="nm"
            align="center"
            label="船舶名称"
            show-overflow-tooltip
          >
          </el-table-column>
          <el-table-column
            width="90"
            prop="cusname"
            align="center"
            label="客户名称"
          >
          </el-table-column>
          <el-table-column
            width="90"
            align="center"
            prop="statusNm"
            label="船舶状态"
          >
            <!-- <template slot-scope="scope">
              <span v-if="scope.row.status == 1">建造中</span>
              <span v-else-if="scope.row.status == 2">已完成</span>
            </template> -->
          </el-table-column>
          <el-table-column
            fixed="right"
            width="110"
            align="center"
            label="操作"
          >
            <template slot-scope="scope">
              <div
                style="
                  display: flex;
                  flex-direction: column;
                  justify-content: center;
                  align-items: center;
                "
              >
                <div
                  style="
                    background: red;
                    color: #fff;
                    font-size: 15px;
                    margin-bottom: 15px;
                    width: 77px;
                    padding: 5px;
                  "
                  v-if="
                    scope.row.statusCd == 'DOCS_STATUS.01' ||
                    scope.row.statusCd == 'DOCS_STATUS.02'
                  "
                  class="btn_yunCheckDetail"
                  @click="tofpxpert"
                >
                  检验入口
                </div>
                <div
                  style="color: rgb(39, 120, 190)"
                  class="btn_yunCheckDetail"
                  @click="
                    toPage(
                      `/personal/archives?id=${scope.row.id}&shipCd=${scope.row.cd}&shipStatus=${scope.row.statusNm}&toYunCheck=true`
                    )
                  "
                >
                  查看详情
                </div>
              </div>
            </template>
          </el-table-column>
        </el-table>
      </div>
    </div>
    <!--投标管理-->
    <div class="manage">
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
          <el-table-column width="170" prop="bidOpenTm" align="center" label="开标时间">
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
              <button class="button3" v-if="(scope.row.depositStatus==1||scope.row.depositStatus==3)&&!scope.row.bidDecideTm&&returnDate(2,scope.row.bidEndTm)&&!scope.row.offer" @click="openOffer(scope.row)">投标报价</button>
              <button class="button3" v-if="(scope.row.depositStatus==1||scope.row.depositStatus==3)&&!scope.row.bidDecideTm&&returnDate(2,scope.row.bidEndTm)&&scope.row.offer" @click="openOffer(scope.row)">查看报价</button>
              <button class="button3" v-if="scope.row.signin.shipBidSigninVo.signinStatus==0&&!scope.row.bidDecideTm&&returnDate(1,scope.row.bidOpenTm)" @click="sign(scope.row)">签到</button>
              <p v-if="scope.row.signin.shipBidSigninVo.signinStatus==1&&!scope.row.bidDecideTm&&returnDate(1,scope.row.bidOpenTm)">已签到</p>
              <button class="button4" @click="toDetail(scope.row)">查看详情</button>
            </div>
          </el-table-column>
        </el-table>
      </div>
    </div>
    <!--检测船舶-->
    <div class="manage">
      <div class="title">
        <p>
          <img src="@/assets/img/personal/首页/留言.png" alt="" />检测船舶<span
            class="more"
            @click="toPage('./shipDetec')"
            >查看更多<i class="el-icon-arrow-right"></i
          ></span>
        </p>
      </div>
      <div class="content1">
        <el-table :data="manageList" style="width: 100%">
          <el-table-column prop="cd" width="160" label="船舶编号">
          </el-table-column>
          <el-table-column prop="nm" width="200" label="船舶名称">
          </el-table-column>
          <el-table-column
            width="100"
            prop="productName"
            align="center"
            label="客户名称"
          >
          </el-table-column>
          <el-table-column
            prop="statusNm"
            width="85"
            align="center"
            label="船舶状态"
          >
          </el-table-column>
          <el-table-column
            align="center"
            width="90"
            fixed="right"
            prop="operations"
            label="操作"
          >
            <template slot-scope="scope">
              <div class="btnList">
                <button
                  class="button3"
                  @click="toDetail2(scope.row, '1')"
                  v-if="scope.row.statusNm != '已交船'"
                >
                  新增
                </button>
                <button class="button2" @click="toDetail2(scope.row, '2')">
                  查看详情
                </button>
              </div>
            </template>
          </el-table-column>
        </el-table>
      </div>
    </div>
  </div>
</template>

<script>
import { mapState } from "vuex";

import Offer from "../../components/person/offer";
  import bail from "../../components/person/bail";
import Download from "@/components/person/download";
export default {
  name: "shipowner",
  components: {
    Download,
    Offer,bail
  },
  data() {
    return {
      offer: false,
      bail: false,
      download1: false,
      applyInfo: {},
      identityCd: '',
      currentRoleId: '',
      nowDate: '',
      // 整改列表
      rectifyList: [],

      // 云检验列表
      yunCheckList: [
        {
          no: "ZCHY20200515",
          shipname:
            "新型 4000KW 中型休闲新型 4000KW 中型休闲新型 4000KW 中型休闲新型 4000KW 中型休闲新型 4000KW 中型休闲",
          cusname: "中创海洋",
          status: 1,
          id: 1,
        },
      ],
      manageList: [
        {
          no: "ZCHY20200515",
          nm: "新型 4000KW 中型休闲...",
          cl: "中创海洋",
          state: "建造中",
        },
        {
          no: "ZCHY20200515",
          nm: "新型 4000KW 中型休闲...",
          cl: "中创海洋",
          state: "建造中",
        },
        {
          no: "ZCHY20200515",
          nm: "新型 4000KW 中型休闲...",
          cl: "中创海洋",
          state: "已完成",
        },
      ],
      bidApplyInfo: {
        orgId: "5024282848367616",
        orgNm: "中创海洋科技股份有限公司",
        bidId: "5031217130673152",
        bidNm: "测试一个招标项目",
        rmks: "",
        attachment:
          "https://sinovat.oss-cn-shanghai.aliyuncs.com/c6178ce400fc4c048a9b4328cc92a878.pdf",
      },
      bidList: [], //投标管理列表
      bidData: {
        orgEnterId: "",
        identityCd: "",
      },
    };
  },
  computed: {
    ...mapState(["currentRole"]),
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
  watch: {
    currentRole() {
      this.identityCd = JSON.parse(this.until.seGet('currentRole')).identityCd
      this.currentRoleId = JSON.parse(this.until.seGet('currentRole')).id
      this.getBidData();
    },
  },
  mounted() {
    this.nowDate = (new Date()).getTime()
    this.identityCd = JSON.parse(this.until.seGet('currentRole')).identityCd
    this.currentRoleId = JSON.parse(this.until.seGet('currentRole')).id
    this.getData();
    // 获取整改单列表
    this.getrectifyList();
    // 云检验列表
    this.getyunCheckList();
  },
  methods: {
    tofpxpert() {
      // let uid = "03939";
      // let ps = "zchy54321";
      let uid = "";
      let ps = "";
      let server = "www.fpxpert.cn";
      let url = encodeURI(
        `esfp://login?uid=${uid}&password=${ps}&server=${server}`
      );
      window.location.replace(url);
    },
    async getData() {
      this.getBidData();
      this.getList();
    },
    //检测船舶列表
    async getList() {
      let qry = this.query.new();
      this.query.toP(qry, 1, 3);
      let param = {
        orgEnterId: this.currentRole.id,
      };
      let data = await this.api.allShip(this.query.toEncode(qry), param);
      this.manageList = data.data.list;
    },
    async getrectifyList() {
      let qry = this.query.new();
      this.query.toP(qry, 1, 3);
      this.query.toW(qry, "status", "0", "EQ");
      this.query.toW(
        qry,
        "orgTestEnterId",
        this.currentRole.id.toString(),
        "EQ"
      );
      let query = this.query.toEncode(qry);
      if (!this.time) {
        this.time = "";
      }
      let data = await this.api.getrectifyList(query, "", "");
      this.rectifyList = data.data.list;
    },
    // 云检验列表
    async getyunCheckList() {
      let qry = this.query.new();
      this.query.toP(qry, 1, 3);
      let param = {
        keyWord: "",
        orgEnterId: this.currentRole.id,
      };
      this.query.toO(qry, "cd", "asc");
      let data = await this.api.allShip(this.query.toEncode(qry), param);
      this.yunCheckList = data.data.list;
    },
    toPage(url) {
      if (url) {
        this.$router.push(url);
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
    statusYun({ row, column, rowIndex, columnIndex }) {
      if (columnIndex == 0) {
        return "color:#2778BE;";
      }
      // else if(columnIndex == 1){
      //  return  `height:25px;overflow:hidden;text-overflow:ellipsis;white-space:nowrap;`
      // }
    },
    //检测船舶详情
    toDetail2(item, type) {
      if (type == "1") {
        this.$router.push(
          "./shipFileDetail?id=" +
            item.id +
            "&cdType=3&formW=jiance&cdType2=3&shipCd=" +
            item.cd +
            "&shipStatus=" +
            item.statusNm
        );
      } else if (type == "2") {
        this.$router.push(
          "./archives?id=" +
            item.id +
            "&shipCd=" +
            item.cd +
            "&shipStatus=" +
            item.statusNm
        );
      }
      console.log(item.id);
    },
    indexMethod(index) {
      return index + 1;
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
    toRectifydetail(row) {
      this.$router.push(`./rectificationDetail?id=${row.id}`);
    },
  },
};
</script>

<style lang="less">
.content1 {
  .btnList {
    display: flex;
    justify-content: space-around;
    flex-direction: column;
    .button3 {
      width: 74px;
      height: 31px;
      background-color: #2778be;
      color: #ffffff;
      border: none;
      border-radius: 3px;
      margin-bottom: 15px;
      cursor: pointer;
    }
    .disable {
      background-color: #999999;
    }
    .button4 {
      border: none;
      width: 74px;
      height: 31px;
      color: #2778be;
      font-size: 15px;
      background-color: #ffffff;
      margin-top: 5px;
      border-radius: 3px;
      cursor: pointer;
    }
    .button2 {
      width: 74px;
      height: 31px;
      background-color: #ffffff;
      color: #2778be;
      border-radius: 3px;
      border: 1px solid #2778be;
      cursor: pointer;
    }
  }
  .btn_yunCheckDetail:hover {
    cursor: pointer;
  }
  .el-table__header thead {
    color: #666666;
    font-size: 14px;
  }
  .el-table__header th {
    background-color: #f7f7f7;
  }
  .el-table__body tr {
    color: #333333;
    font-size: 15px;
    height: 117px;
  }
  .el-table__fixed-right {
    height: 100% !important;
  }
}

.message {
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
</style>
<style lang="less" scoped>
@import url("../../assets/css/init.less");
.left1 {
  width: 67.3%;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  padding-top: 31px;
  .title {
    width: 100%;
    height: 56px;
    border-bottom: 1px solid #f3f3f3;
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

  //整改单
  .message {
    width: 100%;
    background-color: #ffffff;
    min-height: 445px;
  }
  /*监理监造机构  管理船舶*/
  .manage {
    width: 100%;
    background-color: #ffffff;
    margin-top: 24px;
    min-height: 434px;
  }
}
</style>
