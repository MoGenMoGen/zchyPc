<template>
  <!-- 订单详情 -->
  <div id="home">
    <el-dialog title="收款记录" :visible.sync="dialogTableVisible" width="600px">
      <el-table :data="list">
        <el-table-column property="payment" label="款项金额（元）" width="280"></el-table-column>
        <el-table-column property="payDt" label="付款时间" width="280"></el-table-column>

      </el-table>
    </el-dialog>
      <div class="cont">
        <div class="line1">
          <span class="point" @click="toPage()">首页&nbsp;&nbsp;>&nbsp;&nbsp;</span>
          <span class="point" @click="toPage('../personal')">个人中心&nbsp;&nbsp;>&nbsp;&nbsp;</span>
          <span class="point" @click="toPage('../personal/myOrder2?cdType=3')" v-if="info.typesCd=='ORDER_TYPE.02'">我的订单&nbsp;&nbsp;>&nbsp;&nbsp;</span>
          <span class="point" @click="toPage('../personal/myOrder2?cdType=2')" v-if="info.typesCd=='ORDER_TYPE.01'">我的订单&nbsp;&nbsp;>&nbsp;&nbsp;</span>
          <span class="red">订单详情</span>
        </div>

        <div class="line2">
          <div class="line2-l">
            <p >订单号：{{info.cd}}</p>
            <p class="red">{{info.statusNm}}</p>
          </div>
          <div class="line2-r">
            <p>收款记录<span v-if="list.length>4" @click="dialogTableVisible=true">查看更多 ></span></p>
            <ul>
              <li v-for="item in listCurrent" :key="item.id">
                <p>款项金额：￥{{item.payment}}</p>
                <p>付款时间：{{item.payDt}}</p>
              </li>
            </ul>

          </div>
        </div>

<!--        产品信息-->

        <div class="line5">
          <p class="line5-t">
            <span>产品信息</span>
            <span>单价</span>
            <span>数量</span>
            <span>小计</span>
          </p>
          <div class="item">
             <div  >
               <p @click="toProDetail()">
                 <span>{{info.goodsNms}}</span>
               </p>
                <p>{{info.amt}}</p>
               <p>{{info.qty}}</p>
               <p>{{Number(info.amt).toFixed(2)}}</p>
             </div>
          </div>

        </div>

<!--        底部边距-->
        <div class="line7">

        </div>
      </div>
  </div>
</template>

<script>

import {mapState} from "vuex";
import steps from "../../components/steps/steps";
export default {
  head () {
    return {
      title: '订单详情',
    }
  },
  data(){
    return{
      dialogTableVisible:false,
      id:"",
      info:{},
      list:[],
      listCurrent:[],
    }
  },
  computed: {
    ...mapState([
      'bWidth',
      'width',
      'currentRole',
      'price'
    ])
  },
  components: {
      steps,

  },
  mounted(){
    this.id = this.until.getQueryString('id')
    this.getInfo()
  },
  methods:{

   async getInfo(){
      this.api.shipOrderDetail(this.id).then((res) => {
        this.info = res;
      })
     let qry = this.query.new()
     this.query.toP(qry,1,100)
     this.query.toW(qry,'orderId',this.id,'EQ')
     let param={
       orgEnterId:this.currentRole.id
       // orgEnterId: '4982862803145728'//测试
     }
     let data=await this.api.payment(this.query.toEncode(qry),param)
     this.list = data.data.list
     this.listCurrent = this.list.slice(0,4)
    },

    //页面跳转
    toPage(url){
      this.$router.push(url)
    },

    //跳转产品详情
    toProDetail(item) {
      console.log(item)
      // let url = '../sinovat2/Trade/productDetail?id='+item.id + "&type=product"
      // window.open(url)
    },
  }
}
</script>
<style lang="less">


</style>
<style lang="less" scoped>
  @import url("../../assets/css/init.less");
  #home{
    background: #F3F3F3;
    .point{
      cursor: pointer;
    }
    .cont{
      width: 1200px;
      margin: 0 auto;
      .line1{
        height: 52px;
        display: flex;
        display: -webkit-flex;
        align-items: center;
      }
      .line2{
        width: 100%;
        display: flex;
        height: 198px;
        background: white;
        .line2-l{
          width: 325px;
          height: 100%;
          border-right: 1px solid #F1F1F1;
          p{
            margin: 0 auto;
            text-align: center;
            width: fit-content;

            &:nth-child(1){
              color: #999999;
              margin-top: 30px;

            }
            &:nth-child(2){
              font-size: 28px;
              margin-top: 37px;

            }
            &:nth-child(3){
              font-size: 14px;
              color: #666666;
              margin-top: 30px;
              border-radius: 3px;
              border: 1px solid #DDDDDD;
              padding:9px 22px;
              background: #F5F5F5;

            }
          }
        }
        .line2-r{
          flex: 1;
          height: 100%;
          padding:14px 22px 40px 54px;
          box-sizing: border-box;
          >p{
            font-size: 16px;
            font-weight: bold;
            padding-bottom: 10px;
            span{
              font-weight: normal;
              font-size: 14px;
              float: right;
              cursor: pointer;
            }
          }
          ul li{
            display: flex;
            display: -webkit-flex;
            padding: 8px 0;
            border-bottom: 1px solid #efefef;
            p{
              flex: 1;
            }
          }
        }
      }



      .line5{
        width: 100%;
        background: white;
        padding: 30px;
        margin-top: 20px;
        .line5-t{
          width: 100%;
          display: flex;
          height: 50px;
          background: #F3F3F3;
          color: #666666;
          padding-left: 24px;
          padding-right: 24px;
          align-items: center;
          span:nth-of-type(1){
            flex: 3;
          }
          span:nth-of-type(2){
            flex: 1;
          }
          span:nth-of-type(3){
            flex: 1;
            text-align: right;
          }
          span:nth-of-type(4){
            flex: 1;
            text-align: right;
          }
          span:nth-of-type(5){
            flex: 1;
            text-align: right;
          }
        }
        .item{
          div{
            display: flex;
           padding: 19px 24px;
            border-bottom: 1px solid #E8E8E8;
            color: #333333;
            p:nth-of-type(1){
              flex: 3;
              display: flex;
              /*商品图标*/
              img{
                width: 110px;
                height: 110px;
              }
              /*商品名称*/
              span{
                margin-left: 10px;
                margin-right: 20px;
              }
            }
            p:nth-of-type(2){
              flex: 1;
              color: #999999;
            }
            p:nth-of-type(3){
              flex: 1;
              text-align: right;
            }
            p:nth-of-type(4){
              flex: 1;
              text-align: right;
            }
            p:nth-of-type(5){
              flex: 1;
              text-align: right;
            }

          }
        }
      }

      .line7{
        height: 129px;
      }

    }

  }

</style>
