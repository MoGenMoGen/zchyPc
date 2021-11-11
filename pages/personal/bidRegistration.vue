<template>
  <div id="app">
    <div class="header">
      <p><span class="lineC"></span>投标报名</p>
      <span @click="back">
        < 返回</span>
    </div>
    <div class="body">
      <div class="table" style="margin-top: 10px;">
        <el-table :data="list" style="width: 100%;cursor: pointer;" @row-click="toDetail">
          <el-table-column type="index" align="center" min-width="150" label="序号">
            <template slot-scope="scope">
            	<span>{{(pageNum - 1) * pageSize + scope.$index + 1}}</span>
            </template>
          </el-table-column>
          <el-table-column align="center" min-width="200" label="项目信息">
            <template slot-scope="scope">
              <p>{{scope.row.bidNm}}</p>
              <p>项目编号：{{scope.row.bidCd}}</p>
            </template>
          </el-table-column>
          <el-table-column min-width="150" prop="crtTm" align="center" label="报名申请时间">
          </el-table-column>
          <el-table-column min-width="150" align="center" label="状态">
            <template slot-scope="scope">
              <p v-if="scope.row.audit==1" style="color: #E4393C;">待审核</p>
              <p v-if="scope.row.audit==2" style="color: #2778BE;">通过</p>
              <p v-if="scope.row.audit==3" style="color: #E4393C;">已驳回</p>
            </template>
          </el-table-column>
          <el-table-column align="center" min-width="150" fixed="right" prop="options" label="备注">
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
  import Download from "../../components/person/download";
  export default {
    components: {
      Download,
      Offer
    },
    layout: 'person',
    name: "bid",
    data() {
      return {
        list: [],
        pageNum: 1,
        pageSize: 10,
        total: 0, //当前页码
        orgEnterId: '',
        identityCd: ''
      }
    },
    computed: {
      ...mapState([
        'bWidth',
        'width',
        "currentRole",
      ])
    },
    watch: {
      currentRole() {
        this.orgEnterId = JSON.parse(this.until.seGet('currentRole')).id;
      },
      offer() {
        this.getBidData()
      },

    },
    mounted() {
      this.orgEnterId = JSON.parse(this.until.seGet('currentRole')).id;
      this.getList()
    },
    methods: {
      back() {
        this.$router.go(-1)
      },
      toDetail(row) {
        this.$router.push({
          path: './bidAfficheDetail',
          query:{
            id:row.bidId
          }
        })
      },
      handleCurrentChange(val) {
        this.pageNum = val
        this.getList()
      },
      getList() {
        let qry = this.query.new()
        this.query.toW(qry,'orgId',this.orgEnterId+'','EQ')
        this.query.toP(qry,this.pageNum,this.pageSize)
        this.query.toO(qry,'crtTm','desc')
        this.api.getMyBidApplyList(this.query.toEncode(qry)).then(res => {
          this.list = res.data.list
          this.total=res.page.total;
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
        width: 74px;
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
        width: 74px;
        height: 31px;
        color: #2778BE;
        font-size: 15px;
        background-color: #FFFFFF;
        margin-top: 5px;
        border-radius: 3px;
        cursor: pointer;
      }

      .button2 {
        width: 74px;
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
    margin-top: 40px;
  }
</style>
