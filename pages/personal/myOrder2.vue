<template>
<!--我的订单(船东)-->
<div >
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
  <!--申请开票-->
  <to-invoice v-show="invoiceId" :invoiceProNm="invoiceProNm" :id="invoiceId" @close="invoiceId=''" @confirm="invoiceConfirm"></to-invoice>
  <!--分期-->
  <fenqi v-if="type==1 " :info="currentInfo" @close="type=''"></fenqi>
  <!--售后弹窗-->
  <after-sale v-show="currentInfo && type==2" :info="currentInfo" @close="currentInfo=null"></after-sale>

  <el-dialog title="收款记录" :visible.sync="dialogTableVisible" width="700px">
    <el-table :data="payList">
      <el-table-column property="payerNm" label="付款单位" width="180"></el-table-column>
      <el-table-column  label="款项金额" width="120">
        <template slot-scope="scope">
          ￥{{scope.row.payment}}
        </template>
      </el-table-column>
      <el-table-column property="payDt" label="付款时间" width="120"></el-table-column>
      <el-table-column label="订单总额" width="120">
        <template slot-scope="scope">
          ￥{{scope.row.amt}}
        </template>
      </el-table-column>
      <el-table-column  label="已收" width="120">
        <template slot-scope="scope">
          ￥{{scope.row.paid}}
        </template>
      </el-table-column>
      <el-table-column label="未收" width="120">
        <template slot-scope="scope">
          ￥{{scope.row.amt-scope.row.paid}}
        </template>
      </el-table-column>
      <el-table-column label="操作" width="150" fixed="right">
        <template slot-scope="scope">
          <span class="sail" @click="showPop(scope.row.id)" v-if="scope.row.audit==1">确认收款</span>
          <!--<span class="sail2" v-if="item.audit==1">等待确认</span>-->
          <span class="sail2" v-if="scope.row.audit==2">已确认收款</span>
          <span class="sail2" v-if="scope.row.audit==3">拒绝确认收款</span>
        </template>
      </el-table-column>
    </el-table>
  </el-dialog>
  <div class="head">
    <p><span class="lineC"></span>我的订单</p>
    <span @click="back" class="point">< 返回</span>
  </div>
  <div class="search">
    <div class="orderNo">
      <span>订单 :</span>
      <input type="text" placeholder="请输入订单编号" v-model="orderCd">
    </div>
    <!-- <div class="status">
      <span>状态 :</span>
      <el-select v-model="statusCd">
          <el-option
            v-for="item in options"
            :key="item.value"
            :label="item.label"
            :value="item.value">
          </el-option>
        </el-select>
    </div> -->
    <div class="shipNm">
      <input type="text" placeholder="搜索船舶名称" v-model="shipNm">
      <span class="point" @click="toSearch"><i class="iconfont icon-ziyuan"></i></span>
    </div>
  </div>
  <div class="mainOrder">
     <div class="tab">
       <p @click="toMyOrder(1)" :class="{clickP:tabId==1}" class="point">采购订单</p>
        <!--设计院有设计销售订单，设计采购订单-->
       <p @click="toMyOrder(21)" :class="{clickP:tabId==21}" class="point" v-if="currentRole &&currentRole.identityCd=='identity20'">设计销售订单</p>
       <p @click="toMyOrder(22)" :class="{clickP:tabId==22}" class="point" v-if="currentRole &&currentRole.identityCd=='identity20'">设计采购订单</p>
        <!--船厂和建造订单-->
       <p @click="toMyOrder(3)" :class="{clickP:tabId==3}" class="point" v-if="currentRole &&currentRole.identityCd=='identity30'">建造订单</p>
     </div>
     <div class="nav">
       <p v-for="item in options2" @click="choose(item)" :class="{clickP:statusCd==item.value}" class="point" v-if="item.type.indexOf(tabId)>-1">
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
           <span>{{item.crtTm}}</span><span>订单号：{{tabId==1 ? item.orderCd : item.cd}}</span><span>{{tabId ==1 ? item.shopNm : item.consumerNm}}</span>
         </div>
         <div class="boxBodyer">
           <div class="infoBox" v-if="tabId==1">
             <div v-for="v in item.itms">
                <p v-show="v.goodsImgUrl" @click="toProDetail(v.goodsId)"><img :src="v.goodsImgUrl"></p>
                <p class="nmBox" :style="v.goodsImgUrl?'flex: 3;':'flex: 4;'">
                  <span style="color: #333333;">{{v.goodsNm }}</span>
                  <span style="color: #999999;">{{v.goodsSkuAttrNm}}</span>
                </p>
                <p style="color: #E4393C;">￥ {{v.goodsPrice}}</p><p>{{v.qty}}</p><p>{{v.goodsUnit}}</p>
             </div>

           </div>
           <div class="infoBox" v-else>
             <div>
               <p class="nmBox" style="flex: 4;">
                 <span style="color: #333333;">{{item.goodsNms}}</span>
                 <span style="color: #333333;">实收款：￥ {{item.paid}}</span>
               </p>
             </div>
           </div>
           <div style="color: #E4393C;">￥ {{tabId==1 ? item.orderPrice : item.amt}}</div>
           <div>{{item.statusNm}}</div>
           <div class="btnList" v-show="tabId==1">
             <el-button @click="toDetail(item.id)" type="text" size="small" :style="item.statusCd==3?'color:#666;':''" v-if="item.statusCd!=1&&item.statusCd!=5" class="blueBorder">查看详情</el-button>
             <!--<el-button @click="toPay(item.id)" type="danger" size="small" plain v-if="tabId!=1 && item.statusCd==1" class="pay">确认收款</el-button>-->
             <!--分期申请-->
             <p @click="toPay(item)" type="danger" size="small" plain v-if="item.stageStatus==2" class="pay">支付信息</p>
             <!--非分期-->
             <p @click="toPay(item)" type="danger" size="small" plain v-if="item.status==10 && !item.stageStatus" class="pay">去支付</p>
             <el-link v-if="item.stageStatus==1" :underline="false" style="color:#E4393C;font-size:12px"  >分期申请中</el-link>
             <el-link v-if="item.stageStatus==3" :underline="false" style="color:#E4393C;font-size:12px"  >分期申请未通过</el-link>
             <el-link v-if="item.offlineStatus==1" :underline="false" style="color:#E4393C;font-size:12px"  >线下付款申请中</el-link>
             <el-link v-if="item.offlineStatus==3" :underline="false" style="color:#E4393C;font-size:12px"  >线下付款申请未通过</el-link>
             <!--<el-button @click="toPay(item)" type="danger" size="small" plain v-if="item.status==10 || (item.stageStatus==2 && item.orderPrice!==item.payAmt)" class="pay">去支付</el-button>-->
             <p class="blueBg" :underline="false" @click="toInvoice(item)" v-if="item.status>10 && item.status < 100 && !item.invoiceType && item.orderPrice===item.payAmt">申请开票</p>
             <el-link style="font-size:12px" :underline="false"  v-if="item.invoiceType==1">已申请开票</el-link>
             <p @click="cancelOrder(item.id)" v-if="item.statusCd==1" class="grayBorder">取消订单</p>
             <!--<el-button type="text" size="small" v-if="item.statusCd==2||item.statusCd==3||item.statusCd==4" class="blueBg">查看合同</el-button>-->
             <el-button type="text"  size="small" v-if="item.statusCd==3||item.statusCd==4" class="blueWord" @click="trace(item)">查看物流</el-button>
             <el-button type="text" size="small" v-if="item.statusCd==3" @click="confirmGoods(item.id)" class="blueBg" >确认收货</el-button>
             <el-button type="text" size="small" class="grayBorder"  v-if="item.statusCd==4||item.statusCd==5" @click="delOrder(item)">删除订单</el-button>
             <p class="blueBorder" @click="toReturn(item)" :underline="false" v-if="item.status==30|| item.status==40 || item.status==50 || item.status==180 || item.status==190" >退货</p>
           </div>
           <div class="btnList" v-show="tabId!=1">
             <el-button @click="toPage('/order/orderDetail2?id='+item.id)" type="text" size="small" :style="item.statusCd==3?'color:#666;':''" v-if="item.statusCd!=1&&item.statusCd!=5" class="detail">查看详情</el-button>
             <el-button @click="getPayList(item.id)" type="danger" size="small" plain v-if="item.statusCd=='ORDER_STATUS.01'" class="pay">确认收款</el-button>
             <el-button @click="toPage('./contractDetail?id='+item.contractId)" type="text" size="small" class="contrat">查看合同</el-button>
             <el-button type="text" size="small" v-if="item.statusCd=='ORDER_STATUS.03'" class="confirm">确认完成</el-button>
           </div>
         </div>
       </div>

     </div>
  </div>
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
            dialogVisible:false,
            expressList:[],
            invoiceId:'',
            invoiceProNm:'',//发票明细
            currentInfo:null,
            type:'0',//1分期 2退货
            dialogTableVisible:false,
            payId:'',//查看付款记录的订单id
            orderCd:'',
            statusCd:'0',
            cd:'',
            shipNm:'',
            tabId:1,
            currentPage1: 1,      //前往哪页
            pageSize:4,       //每页数量
            total:0,         //总数
            options: [{
                value: '0',
                label: '全部',
                type:'1,21,22,3'
              }, {
                value: '1',
                label: '待付款',
                type:'1'
              }, {
              value: '1',
              label: '待收款',
              type:'21,22,3'
            }, {
              value: '2',
              label: '进行中',
              type:'21,22'
            },{
              value: '2',
              label: '建造中',
              type:'3'
            }, {
                value: '2',
                label: '待发货',
                type:'1'
              }, {
                value: '3',
                label: '待收货',
                type:'1'
              }, {
                value: '4',
                label: '已完成',
                type:'1,21,22,3'
              }],
            options2:[],
            value: '',
            tableData: [],
            currentRole:{},
            list:[{nm:11},{nm:11},{nm:11},{nm:11}],
            payList:[],//收款记录
          }
      },
      computed:{
        ...mapState([
          'bWidth',
          'width',
          'userInfo'
        ])
      },
      mounted() {
         this.tabId = this.until.getQueryString('cdType')
        this.setOptions()
         this.currentRole=JSON.parse(this.until.seGet('currentRole'))
         this.statusCd = this.until.getQueryString('statusCd') ? this.until.getQueryString('statusCd') : 0
         this.getInfo()
      },
      watch:{
        $route(){
          this.tabId = this.until.getQueryString('cdType')
          this.setOptions()
          this.statusCd = this.until.getQueryString('statusCd') ? this.until.getQueryString('statusCd') : 0
          this.getInfo()
        },
        tabId(){
          // this.$router.push('../personal/myOrder2?cdType='+this.tabId)
        }
      },
      methods: {
        setOptions(){
          this.options2 = this.options.filter(item=>{
            let index = item.type.split(',').findIndex(v=>{
              return v===this.tabId
            })
            return index>-1
          })
        },
        async toMyOrder(id){

          if(this.userInfo.pid){
            let flag=0
            let data1=await this.api.getAuthz()//获取当前子账号有的权限
            this.authz=data1.stringPermissions

            if(id==1){    //采购订单
              this.authz.forEach(i=>{
               if(i=='purchase:order'){
                  flag++
                }
              })
            }else if(id==21 && this.currentRole.identityCd=='identity20'){//设计销售订单
              this.authz.forEach(i=>{
               if(i=='design:sales:order'){
                  flag++
                }
              })
            }else if(id==22 && this.currentRole.identityCd=='identity20'){//设计采购订单
              this.authz.forEach(i=>{
               if(i=='design:procurement:order'){
                  flag++
                }
              })
            }else if(id==3 && this.currentRole.identityCd=='identity30'){//建造订单
              this.authz.forEach(i=>{
               if(i=='build:order'){
                  flag++
                }
              })
            }


            if(flag==1){
            	this.$router.push('../personal/myOrder2?cdType='+id)
            }else if(flag==0){
            	this.$message({
            	  message: '当前子账号没有权限',
            	  duration:'1500'
            	});
            }
          }else{
            this.$router.push('../personal/myOrder2?cdType='+id)
          }

        },
        //申请售后
        toReturn(item){
          this.currentInfo = item
          this.type=2
        },
        //确认收货
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
        async getInfo(){
          this.tableData = [];
          let qry = this.query.new()
          if(this.orderCd){
            this.query.toW(qry,'orderCd',this.orderCd,'LK')
          }
          if(this.tabId==1 && this.statusCd){
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
          if(this.tabId!=1 && this.statusCd){
            if(this.statusCd=='1'){
              this.query.toW(qry,'statusCd','ORDER_STATUS.01','EQ')
            }else if(this.statusCd=='2'){
              this.query.toW(qry,'statusCd','ORDER_STATUS.03','EQ')
            }else if(this.statusCd=='4'){
              this.query.toW(qry,'statusCd','ORDER_STATUS.04','EQ')
            }
          }
          this.query.toP(qry,this.currentPage1,this.pageSize)
          this.query.toO(qry,'crtTm','desc')
          let param={
            keyWord:this.shipNm,
            orgEnterId:this.currentRole.id,
          }
          let data
          if(this.tabId==21){ //设计销售订单
            this.query.toW(qry,'typesCd','ORDER_TYPE.08','EQ')
            data=await this.api.shipOderPage(this.query.toEncode(qry),param)
          }else if(this.tabId==22){//设计采购订单
            this.query.toW(qry,'typesCd','ORDER_TYPE.01','EQ')
            data=await this.api.shipOderPage(this.query.toEncode(qry),param)
          }else if(this.tabId==3){
            this.query.toW(qry,'typesCd','ORDER_TYPE.02','EQ')
            data=await this.api.shipOderPage(this.query.toEncode(qry),param)
          }else{
            data=await this.api.myOrderP(this.query.toEncode(qry))
          }
          this.total=data.page.total
          // this.$set(this)
          if(this.total>0){
            data.data.list.forEach((item,index)=>{
              if(item.status=='10'){
                item.statusCd=1
                item.statusNm='待支付'
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
              }
              item.index=index+1

              this.tableData.push(item)
            })
          }
          // console.log('订单')
          // console.log(data)
        },
        toDetail(id){
          this.$router.push('../order/orderDetail?id='+id)
        },
        // toPay(id){
        //   // console.log(id)
        //   this.$router.push('../order/toPay?id='+id)
        // },
        //去支付
        toPay(item){
          if(item.itms[0].goodsPrice==1){
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
            window.open('../sinovat/order/toPay?id='+item.id)
          }
        },
        //获取收付款记录
        async getPayList(id){
          if(id){
            this.payId = id
          }
          let qry = this.query.new()
          this.query.toP(qry,1,100)
          this.query.toW(qry,'orderId',this.payId,'EQ')
          let param={
            orgEnterId:this.currentRole.id
          }
          let data=await this.api.payment(this.query.toEncode(qry),param)
          this.payList = data.data.list

          this.dialogTableVisible = true
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
        cancelOrder(id){
          this.$confirm('是否确认取消订单?', '提示', {
            confirmButtonText: '确认',
            cancelButtonText: '取消',
            type: 'warning'
          }).then(() => {
            this.api.cancelPay(id)
            this.tableData=[]
            this.getInfo()
          })
        },
        toPage(url){
          // console.log(url)
          this.$router.push(url)
        },
        //跳转商城商品详情
        toProDetail(id){
          window.open('./Trade/productDetail?id='+id)
        },
        toSearch(){
          // console.log('搜索')
          this.tableData=[]
          this.getInfo()
        },
        //确认收款
        showPop(id){
          this.$confirm('是否确认收款?', '提示', {
            confirmButtonText: '确认',
            cancelButtonText: '取消',
            type: 'warning'
          }).then(() => {
            this.store.commit('changeLoading',true)
            this.api.confirm({id:id,audit:2}).then(res=>{
              this.$message({
                message: '确认成功',
                type: 'success',
                duration:'1500'
              });
              this.getPayList()
            })

          })

          // this.dialogVisible=true
          // this.info = item
          // this.search = item.cd
          // this.getData()
        },
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
			    this.$router.back()
        },
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
  .pay{background: #E4393C;border: none;color: #FFF;}
  .blueBg{background: #2778BE;color:#fff}
  .grayBorder{color: #999;border:1px solid #999;}
  .blueWord{color: #2778BE;}
  .blueBorder{color: #2778BE;border:1px solid #2778BE;}
  .confirm{background-color: #2778BE;color: #FFF;}
  .sail{
    margin: 5px auto;
    display: flex;
    color: #FFFFFF;
    background:#2778BE;
    width: 83px;
    height: 28px;
    align-items: center;
    justify-content: center;
    border-radius: 3px;
    cursor: pointer;
  }
  .sail2{
    margin: 5px auto;
    display: flex;
    background:@borderColor;
    width: 83px;
    height: 28px;
    align-items: center;
    justify-content: center;
    border-radius: 3px;
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
              img{
                cursor: pointer;
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
            .pay{background: #E4393C;border: none;color: #FFF;}
            .cancel,.delete{color: #999;}
            .detail,.logistics{color: #2778BE;}
            .contrat{color: #2778BE;border-color: #2778BE;}
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
