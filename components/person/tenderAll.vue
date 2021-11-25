<template>
  <div id="app">
    <offer :applyInfo="applyInfo"  v-if="offer" @close="close"></offer>
    <bail :applyInfo="applyInfo" v-if="bail" @close="close2"></bail>
    <div class="body">
      <div class="table">
        <el-table :data="list" style="width: 100%">
          <el-table-column type="index" align="center" min-width="150" label="序号">
            <template slot-scope="scope">
            	<span>{{(pageNum - 1) * pageSize + scope.$index + 1}}</span>
            </template>
          </el-table-column>
          <el-table-column align="center" min-width="200" label="项目信息">
            <template slot-scope="scope">
              <p>{{scope.row.nm}}</p>
              <p>项目编号：{{scope.row.cd}}</p>
            </template>
          </el-table-column>
          <el-table-column width="150" prop="budget" align="center" label="采购金额(元)">
          </el-table-column>
            <el-table-column width="150" prop="depositMoney" align="center" label="保证金(元)">
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
              <!-- depositStatus：1不需要缴纳，2需缴未缴，3已缴纳，4已退还 -->
              <button class="button3" v-if="scope.row.depositStatus==2&&returnDate(2,scope.row.bidEndTm)" @click="openBail(scope.row)">保证金上传</button>
              <button class="button3" v-if="scope.row.depositStatus==3" @click="openBail(scope.row)">查看保证金</button>
              <!-- bidDecideTm：定标时间 ，returnDate(2,scope.row.bidEndTm)：当前时间是否小于截标时间，scope.row.offer：投标报价内容-->
              <button class="button3" v-if="!scope.row.bidDecideTm&&returnDate(2,scope.row.bidEndTm)&&!scope.row.offer" @click="openOffer(scope.row)">资料上传</button>
              <button class="button3" v-if="scope.row.offer" @click="openOffer(scope.row)">查看资料</button>
              <!-- signin.shipBidSigninVo.signinStatus：签到状态 ，returnDate(1,scope.row.bidOpenTm)：当前时间是否大于开标时间-->
              <button class="button3" v-if="(scope.row.depositStatus==1||scope.row.depositStatus==3)&&scope.row.signin.shipBidSigninVo.signinStatus==0&&!scope.row.bidDecideTm&&returnDate(1,scope.row.bidOpenTm)" @click="sign(scope.row)">签到</button>
              <p v-if="(scope.row.depositStatus==1||scope.row.depositStatus==3)&&scope.row.signin.shipBidSigninVo.signinStatus==1&&!scope.row.bidDecideTm&&returnDate(1,scope.row.bidOpenTm)">已签到</p>
              <button class="button4" @click="toDetail(scope.row)">查看详情</button>
            </div>
          </el-table-column>
        </el-table>
      </div>
      <div class="foot">
        <el-pagination background @current-change="handleCurrentChange" :current-page.sync="pageNum"
          :page-size="pageSize" layout="prev, pager, next, jumper" :total="total">
        </el-pagination>
      </div>
    </div>
  </div>
</template>

<script>
  import {
    mapState
  } from "vuex";
  import Offer from "../../components/person/offer";
  import bail from "../../components/person/bail";
  export default {
    components: {
      Offer,bail
    },
    layout: 'person',
    name: "bid",
    data() {
      return {
        offer: false,
        bail: false,
        list: [],
        pageNum: 1,
        pageSize: 10,
        total: 0, //当前页码
        applyInfo: {},
        value1: '',
        value: '',
        identityCd: '',
        currentRoleId: '',
        nowDate: ''
      }
    },
    computed: {
      ...mapState([
        'bWidth',
        'width',
        "currentRole",
        'loading',
        'tel',
        'msgNum'
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
    watch: {
      currentRole() {
        this.identityCd = JSON.parse(this.until.seGet('currentRole')).identityCd
        this.currentRoleId = JSON.parse(this.until.seGet('currentRole')).id
      },
      offer() {
        this.getBidData()
      },

    },
    mounted() {
      this.identityCd = JSON.parse(this.until.seGet('currentRole')).identityCd
      this.currentRoleId = JSON.parse(this.until.seGet('currentRole')).id
      this.getBidData()
      this.nowDate = (new Date()).getTime()


    },
    methods: {
      back() {
        this.$router.go(-1)
      },
      toDetail(row) {
        this.$router.push({
          path: './bidAfficheDetail',
          query: {
            id: row.bidId,
          }
        })
      },
      handleCurrentChange(val) {
        this.pageNum = val
        this.getBidData()
      },
      openBail(row) {
        this.bail = true
        this.applyInfo = row
      },
      openOffer(row) {
          this.offer = true
          this.applyInfo = row
          console.log(32356346456,row);
      },
      close() {
        this.offer = false;
        this.getBidData()
        console.log('父组件close');
      },
      close2() {
        this.bail = false;
        this.getBidData()
      },
      getBidData() {
        let qry = this.query.new()
        this.query.toO(qry, 'bidOpenTm', 'desc')
        this.query.toP(qry, this.pageNum, this.pageSize)
        // this.query.toW(qry, 'viewRangeCd', this.identityCd+'', 'LK')
        this.api.getMyBidList(this.query.toEncode(qry),this.currentRoleId).then(res => {
          this.list = res.data.list
          this.total = res.page.total
        })
      },
      sign(row) {
        let id = row.signin.shipBidSigninVo.id
        this.api.bidSign(id).then(res => {
          this.$message.success('签到成功')
          this.getBidData()
        })
      }
    },
  }
</script>
<style lang="less">
  .search {
    .el-input__inner {
      height: 38px;
      line-height: 38px;
    }
  }

  .table {
    .btnList {
      display: flex;
      justify-content: space-around;
      flex-direction: column;

      .button3 {
        width: 84px;
        height: 31px;
        background-color: #2778BE;
        color: #FFFFFF;
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
        width: 84px;
        height: 31px;
        color: #2778BE;
        font-size: 15px;
        background-color: #FFFFFF;
        margin-top: 5px;
        border-radius: 3px;
        cursor: pointer;
      }

      .button2 {
        width: 84px;
        height: 31px;
        background-color: #FFFFFF;
        color: #2778BE;
        border-radius: 3px;
        border: 1px solid #2778BE;
        cursor: pointer;

      }
    }

    .el-table__header thead {
      color: #666666;
      font-size: 14px;
    }

    .el-table__header th {
      background-color: #F7F7F7;

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
</style>
<style lang="less" scoped>
  @import url("../../assets/css/init.less");

  #app {
    .header {
      display: flex;
      justify-content: space-between;
      align-items: center;
      height: 59px;
      width: 100%;
      padding: 0 24px 0 30px;
      border-bottom: 1px solid rgba(0, 0, 0, 0.1);
      ;

      >p {
        display: flex;
        align-items: center;
        font-size: 20px;
        /*font-family:Microsoft YaHei;*/
        font-weight: 400;
        color: #333333;
        height: 18px;

        .lineC {
          display: block;
          height: 18px;
          width: 3px;
          background-color: #2778BE;
          margin-right: 8px;
        }
      }

      >span {
        display: block;
        height: 12px;
        font-size: 14px;
        /*font-family:Microsoft YaHei;*/
        font-weight: 400;
        color: #666666;
        cursor: pointer;
      }
    }

    .body {
      margin-top: 10px;
      .search {
        height: 78px;
        display: flex;
        align-items: center;
        border-bottom: 1px solid #DFDFDF;
        padding: 0 19px 0 26px;

        >span {
          font-size: 14px;
          color: #333;
          width: 60px;

          &:nth-of-type(2) {
            margin-left: 30px;
          }
        }

        .select {
          margin-right: 30px;
        }

        .shipNm {
          display: flex;
          position: relative;
          height: 38px;
          width: 30.6%;
          background: rgba(255, 255, 255, 1);
          border-radius: 5px;
          border: 1px solid rgba(223, 223, 223, 1);
          border-right: none;
          padding-left: 12px;
          cursor: pointer;

          >input {
            width: 77.6%;
            height: 100%;
            border: none;
          }

          >span {
            position: absolute;
            display: flex;
            justify-content: center;
            align-items: center;
            margin: 0;
            right: 0;
            width: 18.3%;
            height: 100%;
            background: rgba(39, 120, 190, 1);
            border-bottom-right-radius: 5px;
            border-top-right-radius: 5px;
            cursor: pointer;

            .icon-ziyuan {
              color: #ffffff;
            }
          }
        }
      }

      .table {}
    }
  }

  .foot {
    display: flex;
    width: 100%;
    justify-content: center;
  }
</style>
