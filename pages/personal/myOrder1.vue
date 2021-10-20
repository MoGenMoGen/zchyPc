<template>
<!--我的订单(船东)-->
<div >
  <!--申请开票-->
  <to-invoice v-show="invoiceId" :invoiceProNm="invoiceProNm" :id="invoiceId" @close="invoiceId=''" @confirm="invoiceConfirm"></to-invoice>
  <!--分期-->
  <fenqi v-if="type==1 " :info="currentInfo" @close="type=''"></fenqi>
  <!--售后弹窗-->
  <after-sale v-show="currentInfo && type==2" :info="currentInfo" @close="currentInfo=null"></after-sale>
  <div class="head">
    <p><span class="lineC"></span>我的订单</p>
    <span @click="back" class="point">< 返回</span>
  </div>
  <div class="search">
    <div class="orderNo">
      <span>订单 :</span>
      <input type="text" placeholder="请输入订单编号" v-model="orderCd">
    </div>
    <div class="shipNm">
      <input type="text" placeholder="搜索船舶名称" v-model="shipNm">
      <span class="point" @click="toSearch"><i class="iconfont icon-ziyuan"></i></span>
    </div>
  </div>
  <div class="mainOrder">
     <div class="tab">
       <p @click="toMyOrder(1)" :class="{clickP:tabId==1}" class="point">产品订单</p>
       <p @click="toMyOrder(2)" :class="{clickP:tabId==2}" class="point">船舶订单</p>
     </div>
     <div class="nav">
       <p v-for="item in options" @click="choose(item)" :class="{clickP:statusCd==item.value}" class="point">
         {{item.label}}
       </p>
     </div>
     <div class="tableOrder">
       <div class="orderTitle">
         <p>订单详情</p><p style="flex: 2;">规格型号</p><p>单价</p><p style="flex: 0.8;">数量</p><p style="flex: 0.8;">单位</p>
         <p style="flex: 1.2;">实付款</p><p style="flex: 1.2;">交易状态</p><p>交易操作</p>
       </div>
       <div class="orderBox" v-for="item in tableData">
         <div class="boxHead">
           <span>{{item.crtTm}}</span>
           <span>订单号 :  <el-link style="margin-bottom: 2px" :underline="false" @click="toOrder(item)">{{item.orderCd}}</el-link> </span>
           <span>{{item.shopNm}}</span>
         </div>
         <div class="boxBodyer">
           <div class="infoBox">
             <div v-for="v in item.itms">
                <p v-show="v.goodsImgUrl"><img :src="v.goodsImgUrl"  @click="toProDetail(v.goodsId)"></p>
                <p class="nmBox" :style="v.goodsImgUrl?'flex: 3;':'flex: 4;'">
                  <span style="color: #333333;">{{v.goodsNm}}</span>
                  <span style="color: #999999;">{{v.goodsSkuAttrNm}}</span>
                </p>
                <p style="color: #E4393C;" v-if="v.goodsPrice!=price">￥ {{v.goodsPrice}}</p>
               <p style="color: #E4393C;" v-else>价格面议</p>
               <p>x{{v.qty}}</p>
               <p>{{v.goodsUnit}}</p>
             </div>
           </div>
           <div style="color: #E4393C;"  v-if="item.itms[0].goodsPrice!=price">￥ {{item.orderAmt}}</div>
           <div style="color: #E4393C;"  v-else>价格面议</div>
           <div>{{item.statusNm}}</div>
           <div class="btnList">
             <!--未付款状态下：分期状态为无申请，申请通过，申请拒绝-->
             <!--分期申请-->
             <p @click="toPay(item)" type="danger" size="small" plain v-if="item.stageStatus==2" class="pay">支付信息</p>
             <p @click="toPay(item)" type="danger" size="small" plain v-if="item.status==10 && !item.stageStatus" class="pay">去支付</p>
             <el-link v-if="item.stageStatus==1" :underline="false" style="color:#E4393C;font-size:12px"  >分期申请中</el-link>
             <el-link v-if="item.stageStatus==3" :underline="false" style="color:#E4393C;font-size:12px"  >分期申请未通过</el-link>
             <el-link v-if="item.offlineStatus==1" :underline="false" style="color:#E4393C;font-size:12px"  >线下付款申请中</el-link>
             <el-link v-if="item.offlineStatus==3" :underline="false" style="color:#E4393C;font-size:12px"  >线下付款申请未通过</el-link>
             <p class="blueBg" :underline="false" @click="toInvoice(item)" v-if="item.status>10 && item.status < 100  && !item.invoiceType && item.orderPrice===item.payAmt">申请开票</p>
             <el-link style="font-size:12px" :underline="false"  v-if="item.invoiceType==1">已申请开票</el-link>
             <p class="grayBorder" @click="cancelOrder(item.id)"  v-if="item.statusCd==1||item.statusCd==2" :underline="false" style="font-size:12px"  >取消订单</p>
             <p class="blueWord" v-if="item.statusCd==3||item.statusCd==4" :underline="false" style="font-size:12px" @click="trace(item)"  >查看物流</p>
             <p class="blueBg" v-if="item.statusCd==3" @click="confirmGoods(item.id)" :underline="false" style="font-size:12px">确认收货</p>
             <p class="grayBorder" v-if="item.statusCd==4||item.statusCd==5" :underline="false" style="font-size:12px" @click="delOrder(item)" >删除订单</p>
             <p style="font-size:12px" class="blueBorder" :underline="false" @click="toOrder(item)">订单详情</p>
             <p class="blueBorder" @click="toReturn(item)" :underline="false" v-if="item.status==30|| item.status==40 || item.status==50 || item.status==190" >退货</p>
             <!--170： 退货中，180：退货成功，190：退货失败-->
             <!--<p class="blueBorder"  :underline="false" v-if="item.status==170 ">退货中</p>-->
             <!--<p class="blueBorder"  :underline="false" v-if="item.status==180 ">退货中</p>-->
             <!--<p class="blueBorder"  :underline="false" v-if="item.status==190 ">退货中</p>-->
           </div>
         </div>
       </div>
     </div>
  </div>


  <el-dialog title="物流信息"  :visible.sync="dialogVisible"   width="40%"  >
    <el-timeline >
      <el-timeline-item
        v-for="(item, index) in expressList"
        :key="index"
        :timestamp="item.time">
        {{item.context}}
      </el-timeline-item>
    </el-timeline>
  </el-dialog>

  <div class="footPage">
       <!-- <page-ination :total="total" :pageSize="pageSize" @changeP='pageChange'></page-ination> -->
       <el-pagination
         background
         @current-change="handleCurrentChange"
         :current-page.sync="currentPage1"
         :page-size="pageSize"
         layout="prev, pager, next, jumper"
         :total="total">
       </el-pagination>
  </div>
</div>
</template>
<script>
    import {mapState} from "vuex";
    import pageInation from "@/components/pageInation.vue"
    import afterSale from '@/components/person/afterSale.vue'
    import fenqi from '@/components/person/fenqi.vue'
    import toInvoice from '@/components/person/toInvoice.vue'
    export default {
      layout:'person',
      name: "shipowner",
      components: {
        pageInation,
        afterSale,
        fenqi,
        toInvoice
      },
      data(){
          return{
            invoiceId:'',
            invoiceProNm:'',//发票明细
            currentInfo:null,
            dialogVisible: false,
            expressList:[],
            orderCd:'',
            statusCd:'0',
            cd:'',
            shipNm:'',
            tabId:1,
            currentPage1: 1,      //前往哪页
            pageSize:4,       //每页数量
            total:0,         //总数
            type:'0',//1分期 2退货
            currentRole:{},
            options: [{
                value: '0',
                label: '全部'
              }, {
                value: '1',
                label: '待付款'
              }, {
                value: '2',
                label: '待发货'
              }, {
                value: '3',
                label: '待收货'
              }, {
                value: '4',
                label: '已完成'
              }],
            value: '',
            tableData: [],
            list:[{nm:11},{nm:11},{nm:11},{nm:11}],

          }
      },
      computed:{
        ...mapState([
          'bWidth',
          'width',
          'price',
          'userInfo'
        ])
      },
      watch:{
        $route(){
          this.tabId = this.until.getQueryString('cdType')
          this.statusCd = this.until.getQueryString('statusCd') ? this.until.getQueryString('statusCd') : 0
          this.getInfo()
        },

      },
      mounted() {
         this.currentRole=JSON.parse(this.until.seGet('currentRole'))
         this.tabId = this.until.getQueryString('cdType')
         this.statusCd = this.until.getQueryString('statusCd') ? this.until.getQueryString('statusCd') : 0
         this.getInfo()
      },
      methods: {
        async toMyOrder(id){
          if(this.userInfo.pid){
            let flag=0
            let data1=await this.api.getAuthz()//获取当前子账号有的权限
            this.authz=data1.stringPermissions

            if(id==1){    //产品订单
              this.authz.forEach(i=>{
               if(i=='product:order'){
                  flag++
                }
              })
            }else if(id==2 && this.currentRole.identityCd!="identity10"){//船舶订单
              this.authz.forEach(i=>{
               if(i=='ship:order'){
                  flag++
                }
              })
            }


            if(flag==1){
            	this.$router.push('../personal/myOrder1?cdType='+id)
            }else if(flag==0){
            	this.$message({
            	  message: '当前子账号没有权限',
            	  duration:'1500'
            	});
            }
          }else{
            this.$router.push('../personal/myOrder1?cdType='+id)
          }


        },
        //跳转商城商品详情
        toProDetail(id){
          window.open('./Trade/productDetail?id='+id)
        },
        //查询物流
        trace(item){
          if (item.logiEntpCd != null && item.logiCd != null){
            this.api.trace(item.logiEntpCd,item.logiCd).then((res) => {
              let parse = JSON.parse(res.msg);
              if (parse.status === '200'){
                this.expressList = parse.data;
                this.dialogVisible = true;
              }else {
                this.$message.error("未找到物流信息")
              }
            })
          }else {
            this.$message.error("未找到物流信息")
          }
        },

        //申请开票
        toInvoice(item){
          // this.invoiceId = item.id
          // let arr = []
          // item.itms.forEach(v=>{
          //   arr.push(v.goodsNm)
          // })
          // this.invoiceProNm = arr.join('/')
          this.$router.push('./myBillApply?id='+item.id)
        },
        //开票成功
        invoiceConfirm(){
           this.getInfo()
        },
        //跳转订单详情
        toOrder(item){
          window.open('./order/orderDetail?id='+item.id)
        },
        //删除订单
         delOrder(item){

          this.$confirm('是否确认删除订单?', '提示', {
            confirmButtonText: '确认',
            cancelButtonText: '取消',
            type: 'warning'
          }).then(() => {
            this.api.delOrder(item.id).then((res) => {
                if (res.code === 0) {
                  this.tableData = []
                  this.getInfo()
                }
              }
            )

          })
        },
        //去支付
        toPay(item){
          if(item.itms[0].goodsPrice==this.price){
            this.$alert('等待卖家修改价格', '标题名称', {
              confirmButtonText: '确定',
            });
            return
          }
          console.log(item.id)
          //this.$router.push('../order/toPay?id='+item.id)
          if(item.stageStatus==2){
            this.currentInfo = item
            console.log(this.currentInfo)
            this.type = 1
          }else {
            window.open('../sinovat2/order/toPay?id='+item.id)
          }
        },
        //申请售后
        toReturn(item){
          this.currentInfo = item
          this.type=2
        },
        //获取订单列表
        async getInfo(){
          this.tableData = [];
          let qry = this.query.new()
          if(this.statusCd){
            if(this.statusCd=='0'){
            }
            else if(this.statusCd=='1'){
              this.query.toW(qry,'status','10','EQ')
            }
            else if(this.statusCd=='2'){
              this.query.toW(qry,'status','20','EQ')
            }
            else if(this.statusCd=='3'){
              this.query.toW(qry,'status','30,40','BT')
            }
            else if(this.statusCd=='4'){
              this.query.toW(qry,'status','50,90','BT')
            }
          }

          this.query.toO(qry,'crtTm','desc')
          this.query.toP(qry,this.currentPage1,this.pageSize)
          let data
          if(this.currentRole.identityCd=="identity10"&&this.tabId=='3'){//船东的船舶订单是线下的订单
            data= await this.api.shipOderPage(this.query.toEncode(qry),{orgEnterId:this.currentRole.id})
            console.log('船东船舶')
            console.log(data)
          }else{
            if(this.orderCd){
              this.query.toW(qry,'orderCd',this.orderCd,'LK')
            }
            if(this.shipNm){
              this.query.toW(qry,'orderCd',this.shipNm,'LK')
            }
            //产品订单
            if (this.tabId === '1'){
              this.query.toW(qry,'ship',0,'EQ')
            }
            //船舶订单
            if (this.tabId === '2'){
              this.query.toW(qry,'ship',1,'EQ')
            }
            data= await this.api.myOrderP(this.query.toEncode(qry))
          }

          this.total = data.page.total
          // this.$set(this)
          if(this.total>0){
            data.data.list.forEach((item,index)=>{
              if(item.status=='10'){
                item.statusCd=1
                item.statusNm='待支付'
                if(item.stageStatus==1){
                  item.statusNm = '分期支付申请中'
                }
                if(item.stageStatus==2){
                  item.statusNm = '待分期支付'
                }
                if(item.stageStatus==3){
                  item.statusNm = '分期支付未通过'
                }
              }else if(item.status=='20'){
                item.statusCd=2
                item.statusNm='待发货'
              }else if(item.status=='30'){
                item.statusCd=3
                item.statusNm='已发货'
              }else if(item.status=='40'){
                item.statusCd=3
                item.statusNm='未签收'
              }else if(item.status=='50'){
                item.statusCd=4
                item.statusNm='自动签收'
              }else if(item.status=='60'){
                item.statusCd=4
                item.statusNm='用户确认收货'
              }else if(item.status=='70'){
                item.statusCd=4
                item.statusNm='自动确认收货'
              }else if(item.status=='80'){
                item.statusCd=4
                item.statusNm='待评价'
              }else if(item.status=='90'){
                item.statusCd=4
                item.statusNm='已完成'
              }else if(item.status=='100'){
                item.statusCd=4
                item.statusNm='取消处理中'
              }else if(item.status=='110'){
                item.statusCd=4
                item.statusNm='取消中'
              }else if(item.status=='120'){
                item.statusCd=5
                item.statusNm='已取消'
              }else if(item.status=='160'){
                item.statusCd=6
                item.statusNm='删除订单'
              }else if(item.status=='170'){

                item.statusNm='退货中'
              }else if(item.status=='180'){
                item.statusNm='退货成功'
              }else if(item.status=='190'){
                item.statusNm='退货失败'
              }
              item.index=index+1
              this.tableData.push(item)
            })
          }
        },
        // toDetail(id){
        //   this.$router.push('../order/orderDetail?id='+id)
        // },
        confirmGoods(id){
          this.$confirm('是否确认收货?', '提示', {
            confirmButtonText: '确认',
            cancelButtonText: '取消',
            type: 'warning'
          }).then(() => {
            this.api.confirmGoods(id).then((res) => {
                if (res.code === 0) {
                  this.tableData = []
                  this.getInfo()
                }
              }
            )
          })
        },
        cancelOrder(id){
          this.$confirm('是否确认取消订单?', '提示', {
            confirmButtonText: '确认',
            cancelButtonText: '取消',
            type: 'warning'
          }).then(() => {
            this.api.cancelPay(id).then((res) => {
                if (res.code === 0) {
                  this.tableData = []
                  this.getInfo()
                }
              }
            )
          })
        },
        toSearch(){
          this.tableData=[]
          this.getInfo()
        },
        handleSizeChange(val) {

        },
        // pageChange(pageNo) {

        //   this.currentPage1=JSON.parse(pageNo)
        //   this.tableData=[]
        //   this.getInfo()
        // },
        handleCurrentChange(val){
          this.currentPage1=val
          this.tableData=[]
          this.getInfo()
        },
        choose(item){//选择状态
          this.statusCd=item.value
          this.tableData=[]
          this.getInfo()
        },
        back(){//返回
          this.$router.push('../personal/')
        },
        toPage(url){
          this.$router.push(url)
        }
      },
    }
</script>
<style lang="less">
  .status .el-input__inner{
          height: 36px;
          line-height: 36px;
  }
  // .tableOrder{
  //   .el-table__header thead{
  //     color: #666666;
  //     font-size:16px;
  //     font-weight:400;
  //   }
  //   .el-table__header th{
  //     background-color: #F7F7F7;

  //   }
  //   .el-table__body tr{
  //     color: #333333;
  //     font-size:15px;
  //   }
  //   .el-table__fixed-right{
  //     height: 100%!important;
  //   }
  // }
</style>
<style lang="less" scoped>
  @import url("../../assets/css/init.less");
  .point{
    cursor:pointer;
  }
  .head{
    display: flex;
    justify-content: space-between;
    align-items: center;
    height: 59px;
    width: 100%;
    padding: 0 24px 0 30px;
    border-bottom: 1px solid rgba(0,0,0,0.1);;
    >p{
      display: flex;
      align-items: center;
      font-size:20px;
      font-family:Microsoft YaHei;
      font-weight:400;
      color:#333333;
      height: 18px;
      .lineC{
        display: block;
        height: 18px;
        width: 3px;
        background-color: #2778BE;
        margin-right: 8px;
      }
    }
    >span{
      display: block;
      height:12px;
      font-size:14px;
      font-family:Microsoft YaHei;
      font-weight:400;
      color: #666666;
    }
  }
  .search{
    width: 100%;
    height: 78px;
    padding: 20px 22px;
    display: flex;
    justify-content: space-between;
    align-items: center;
    >div{
      display: flex;
      align-items: center;
      height: 38px;
      box-sizing: content-box;
      >span{
        white-space: nowrap;
        display: inline-block;
        height: 12px;
        font-size: 14px;
        margin-right: 20px;
        font-family: Microsoft YaHei;
        font-weight: 400;
        line-height: 12px;
        color: #333333;
      }
    }
    .orderNo{
      width: 27.1%;
      >input{
        width: 78.9%;
        background: #FFFFFF;
        border:1px solid #DFDFDF;
        border-radius:5px;
        height: 100%;
        font-size: 14px;
        padding-left: 10px;
      }
    }
    .status{
      width: 31.5%;
      .el-select{
        width: 81.9%;
        // height: 100%;
        background:rgba(255,255,255,1);
        border:1px solid rgba(223, 223, 223, 1);
        border-radius:5px;
        height: 100%;

      }
    }
    .shipNm{
      position: relative;
      width: 30.6%;
      background:rgba(255,255,255,1);
      border:1px solid rgba(223, 223, 223, 1);
      border-radius:5px;
      padding-left: 12px;
      >input{
        width: 77.6%;
        height: 100%;
        border: none;
      }
      >span{
        position: absolute;
        display: flex;
        justify-content: center;
        align-items: center;
        margin: 0;
        right: 0;
        width: 18.3%;
        height:100%;
        background:rgba(39,120,190,1);
        border-bottom-right-radius: 5px;
        border-top-right-radius: 5px;
        .icon-ziyuan{
          color: #ffffff;
        }
      }
    }
  }
  .mainOrder{
    width: 100%;
    .tab{
      margin: 0 36px 0 30px;
      border-bottom: 1px solid #2778BE;
      height: 40px;
      display: flex;
      >p{
        display: flex;
        justify-content: center;
        align-items: center;
        width:14.1%;
        height: 100%;
        color: #666666;
        font-size:14px;
        font-family:Microsoft YaHei;
        font-weight:400;
      }
      .clickP{
        background-color: #2778BE;
        color: #FFFFFF;
      }
    }
    .nav{
      display: flex;
      margin: 17px 36px 30px 30px;
      height: 40px;
      border-bottom:  1px solid rgba(0,0,0,0.1);
      >p{
        display: flex;
        justify-content: center;
        align-items: center;
        width:14.3%;
        height: 100%;
        color: #666666;
        font-size:14px;
        font-family:Microsoft YaHei;
        font-weight:400;
      }
      .clickP{
        color: #2778BE;
        border-bottom:  2px solid #2778BE;
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
            flex: 5;
            display: flex;
            flex-direction: column;
            border-bottom: none;
            padding-top: 0;
            >div{
              display: flex;
              padding: 20px 0 20px 30px;
              height: 100%;
              border-bottom: 1px solid #E7E7E7;
              >p{
                flex: 2;
                margin-right: 10px;
                text-align: center;
                &:last-of-type{
                  flex: 1;
                }
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
            .pay{background: #E4393C;border: none;color: #FFF;}
            .blueBg{background: #2778BE;color:#fff}
            .grayBorder{color: #999;border:1px solid #999;}
            .blueWord{color: #2778BE;}
            .blueBorder{color: #2778BE;border:1px solid #2778BE;}
            .confirm{background-color: #2778BE;color: #FFF;}
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
