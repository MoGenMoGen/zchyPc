<template>
  <div>
    <div class="top-img" v-if="imgUrl">
      <img :src="imgUrl">
    </div>
    <div class="tableOrder">
      <div class="orderTitle">
        <p style="flex: 0.6;">订单详情</p>
        <p style="flex:2">规格型号</p>
        <p style="flex: 0.8;">单价</p>
        <p style="flex: 0.6;">数量</p>
        <p>实付款</p>
        <!-- <p>发票类型</p> -->
        <p>状态</p>
        <p>操作</p>
      </div>
      <div class="orderBox" v-for="item in list">
        <div class="boxHead">
          <span>{{item.crtTm}}</span><span>订单号：{{item.orderCd}}</span><span>{{item.shopNm}}</span>
        </div>
        <div class="boxBodyer">
          <div class="infoBox">
            <div v-for="v in item.itms">
              <p v-show="true"><img :src="v.goodsImgUrl"></p>
              <p class="nmBox" :style="true?'flex: 3;':'flex: 4;'">
                <span style="color: #333333;">{{v.goodsNm}}</span>
                <span style="color: #999999;">{{v.goodsSkuAttrNm}}</span>
              </p>
              <p style="color: #E4393C;">￥ {{fmoney(v.goodsPrice)}}</p><p>x {{v.qty}}</p>
            </div>
          </div>
          <div style="color: #E4393C;">￥ {{fmoney(item.orderPrice)}}</div>
          <!-- <div>增值税发票</div> -->
          <div style="color: #E4393C;" v-if="item.invoiceType==0">未开票</div>
          <div style="color: #E4393C;" v-if="item.invoiceType==1">已提交开票申请</div>
          <div style="color: #E4393C;" v-if="item.invoiceType==2">部分开票</div>
          <div v-if="item.invoiceType==3">已开票</div>
          <div class="btnList">
            <el-button type="primary" size="small" plain @click="toApply(item.id,item.orderCd)" v-if="item.invoiceType!=3&&item.payAmt!=0">申请发票</el-button>
            <el-button size="small" @click="toDetail(item.orderCd)" v-if="item.invoiceType!=0&&item.invoiceType!=1">发票详情</el-button>
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
  export default {
    name: "billApply",
    props: {

    },
    data() {
      return {
        imgUrl: '',
        currentPage1: 1,      //前往哪页
        pageSize:4,       //每页数量
        total:0,         //总数
        list: []
      }
    },
    mounted() {
      this.api.getAdert('billAd').then(res => {
        this.imgUrl=res[0].imgUrl
      })
      this.getList()
    },
    methods: {
      fmoney(s, n) {
          n = n > 0 && n <= 20 ? n : 2;
          s = parseFloat((s + "").replace(/[^\d\.-]/g, "")).toFixed(n) + "";
          var l = s.split(".")[0].split("").reverse(), r = s.split(".")[1];
          var t = "";
          for (let i = 0; i < l.length; i++) {
              t += l[i] + ((i + 1) % 3 == 0 && (i + 1) != l.length ? "," : "");
          }
          return t.split("").reverse().join("") + "." + r;
      },
      getList() {
        let qry = this.query.new()
        this.query.toP(qry,this.currentPage1,this.pageSize)
        this.query.toO(qry,'crtTm','desc')
        this.api.myOrderP(this.query.toEncode(qry)).then(res => {
          this.total=res.page.total
          this.list = res.data.list
        })
      },
      toApply(id,cd) {
        this.$router.push(`./myBillApply?id=${id}&orderCd=${cd}`)
      },
      toDetail(id){
        this.$router.push('./myBillDetail?id='+id)
      },
      handleCurrentChange(val){
        this.currentPage1=val
        this.list=[]
        this.getList()
      },
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
      padding: 22px 30px;
      box-sizing: border-box;
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
            padding: 20px 0 20px 10px;
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
              height: 90px;
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
