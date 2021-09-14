<template>
  <div>
    <div class="top-img">
      <img :src="ts">
    </div>
    <div class="tableOrder">
      <div class="orderTitle">
        <p>订单详情</p>
        <p style="flex:2">规格型号</p>
        <p>数量</p>
        <p>发票类型</p>
        <p>状态</p>
        <p>操作</p>
      </div>
      <div class="orderBox" v-for="item in 3">
        <div class="boxHead">
          <span>2020-10-12  08:20:11</span><span>订单号：20201012082011</span><span>中创海洋旗舰店</span>
        </div>
        <div class="boxBodyer">
          <div class="infoBox">
            <div v-for="v in 3">
              <p v-show="true"><img :src="ts"></p>
              <p class="nmBox" :style="true?'flex: 3;':'flex: 4;'">
                <span style="color: #333333;">水下推进器 Lefeet S1r 浮潜工具潜水工具</span>
                <span style="color: #999999;">颜色：红色 规格：s</span>
              </p>
              <p>x 1</p>
            </div>
          </div>
          <div>增值税发票</div>
          <div style="color: #E4393C;">未开票</div>
          <div class="btnList">
            <el-button type="primary" size="small" plain v-if="false">申请发票</el-button>
            <el-button size="small" v-if="true">发票详情</el-button>
          </div>
        </div>
      </div>
    </div>
    <div class="footPage">
      <el-pagination background @current-change="handleCurrentChange" :current-page.sync="currentPage1"
        :page-size="pageSize" layout="prev, pager, next, jumper" :total="total">
      </el-pagination>
    </div>
  </div>
</template>

<script>
  import ts from "../../assets/img/personal/提示2.png";
  export default {
    name: "billApply",
    props: {

    },
    data() {
      return {
        ts,
        currentPage1: 1,      //前往哪页
        pageSize:4,       //每页数量
        total:0,         //总数
      }
    },
    methods: {

    }
  }
</script>

<style scoped lang="less">
  .top-img {
    width: 100%;
    padding: 20px 30px;
    box-sizing: border-box;
    img {
      width: 100%;
      height: 100%;
    }
  }
  .tableOrder{
    width: 100%;
    .orderTitle{
      background-color: #F7F7F7;
      font-size: 16px;
      color: #666666;
      padding: 22px 0;
      display: flex;
      justify-content: space-between;
      >p{
        text-align: center;
        flex: 1;
      }
    }
    .orderBox{
      margin: 30px;
      .boxHead{
        padding: 14px 0 14px 30px;
        background:rgba(247,247,247,1);
        font-size:14px;
        color: #666666;
        >span{
          margin-right: 6.4%;
        }
      }
      .boxBodyer{
        display: flex;
        border-left:  1px solid #E7E7E7;
        >div{
          flex: 1;
          border-right: 1px solid #E7E7E7;
          padding-top: 20px;
          text-align: center;
          border-bottom: 1px solid #E7E7E7;
        }
        .infoBox{
          flex: 4;
          display: flex;
          flex-direction: column;
          border-bottom: none;
          padding-top: 0;
          >div{
            display: flex;
            padding: 20px 0 20px 30px;
            height: 100%;
            border-bottom: 1px solid #E7E7E7;
            img{
              width: 90px;
              height: 90px;
              // cursor: pointer;
            }
            >p{
              flex: 1;
              margin-right: 10px;
              text-align: center;
            }
            .nmBox{
              flex: 3;
              text-align: left;
              display: flex;
              flex-direction: column;
              justify-content: space-between;
              >span{
                display: block;
              }
            }
          }
        }
        .btnList{
          display: flex;
          flex-direction: column;
          align-items: center;
          padding-top: 0;
          padding-bottom: 10px;
          >button{
            width: 60%;
            margin: 0;
            margin-top: 12px;
          }
          >p{
            width: 60%;
            text-align: center;
            height: 24px;
            line-height: 24px;
            border-radius: 3px;
            margin-top: 12px;
            cursor: pointer;
            font-size: 12px;
          }
        }
      }
    }
  }
  .footPage{
    display: flex;
    justify-content: center;
    margin: 44px 0 20px;
    .btn_confirm{
      margin-left: 43px;
    }
  }
</style>
