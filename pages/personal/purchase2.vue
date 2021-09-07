<template>
<!-- 供应商-资金管理-代购收款-->
    <div class="main">
      <div style="padding:0 30px">
        <tobbar :title="title" :show-all="false"></tobbar>
      </div>
      <p style="border-bottom: 1px solid rgba(0,0,0,0.1);"></p>

      <div class="line1">

        <div class="search" >
          <p>
            <input v-model="search" placeholder="搜索订单编号" >
          </p>
          <p @click="toSearch"><img :src="searchIc"></p>
        </div>
      </div>
      <div class="line2">
        <div>
          <p v-for="(item,index) in tabList" :key="item.id"  :class="{active2: tabId==item.id}" @click="chooseTab(item,index)">
            <span>{{item.nm}}</span>
          </p>
        </div>
      </div>
      <el-table
        v-if="tabId==1"
        :data="list"
        :header-cell-style="{background:'#f7f7f7'}"
        style="width: 100%">
        <el-table-column
          label="订单编号"
          width="190">
          <template slot-scope="scope">
            <p @click="toOrder(scope.row)" class="cursor">{{scope.row.orderCd}}</p>
          </template>
        </el-table-column>
        <el-table-column
          prop="payerCompany"
          label="付款单位"
          width="210">
        </el-table-column>
        <el-table-column
          prop="orderAmt"
          label="订单总额"
          width="145">
          <template slot-scope="scope">
            ￥{{scope.row.orderAmt}}
          </template>
        </el-table-column>
        <el-table-column
          width="145"
          label="计划完成金额">
          <template slot-scope="scope">
            ￥{{scope.row.fees}}
          </template>
        </el-table-column>
        <el-table-column
          width="125"
          label="计划完成时间">
          <template slot-scope="scope">
            {{scope.row.planDate}}
          </template>
        </el-table-column>
        <el-table-column
          width="145"
          label="实际完成金额">
          <template slot-scope="scope">
            ￥{{scope.row.alFees}}
          </template>
        </el-table-column>
        <el-table-column
          width="125"
          label="实际完成时间">
          <template slot-scope="scope">
            {{scope.row.complateDate}}
          </template>
        </el-table-column>
        <el-table-column
          width="115"
          label="状态">
          <template slot-scope="scope">
            {{scope.row.statusNm}}
          </template>
        </el-table-column>
        <!-- <el-table-column
          prop="planDate"
          label="付款时间"
          width="150">
        </el-table-column>
        <el-table-column
          label="款项金额"
          width="180">
          <template slot-scope="scope">
            ￥{{scope.row.fees}}
          </template>
        </el-table-column>
        <el-table-column
          width="180"
          label="订单总额">
          <template slot-scope="scope">
            ￥{{scope.row.orderAmt}}
          </template>
        </el-table-column>
        <el-table-column
          width="180"
          label="已付">
          <template slot-scope="scope">
            ￥{{scope.row.alFees}}
          </template>
        </el-table-column>
        <el-table-column
          prop="alFees"
          label="待付">
          <template slot-scope="scope">
            ￥{{scope.row.orderAmt-scope.row.alFees}}
          </template>
        </el-table-column>
        <el-table-column
          label="订单类型">
          <template slot-scope="scope">
            {{scope.row.isShop? '非商城':'商城'}}
          </template>
        </el-table-column> -->
      </el-table>
      <el-table
        v-if="tabId==2"
        :data="list"
        :header-cell-style="{background:'#f7f7f7'}"
        style="width: 100%">
        <el-table-column
          label="订单编号"
          width="180">
          <template slot-scope="scope">
            <p @click="toOrder(scope.row)" class="cursor">{{scope.row.orderCd}}</p>
          </template>
        </el-table-column>
        <el-table-column
          prop="payerNm"
          label="付款单位"
          width="210">
        </el-table-column>
        <el-table-column
          prop="orderAmt"
          label="订单总额"
          width="145">
          <template slot-scope="scope">
            ￥{{scope.row.orderAmt}}
          </template>
        </el-table-column>
        <el-table-column
          width="145"
          label="计划完成金额">
          <template slot-scope="scope">
            ￥{{scope.row.fees}}
          </template>
        </el-table-column>
        <el-table-column
          width="125"
          label="计划完成时间">
          <template slot-scope="scope">
            {{scope.row.planDate}}
          </template>
        </el-table-column>
        <el-table-column
          width="145"
          label="实际完成金额">
          <template slot-scope="scope">
            ￥{{scope.row.alFees}}
          </template>
        </el-table-column>
        <el-table-column
          width="125"
          label="实际完成时间">
          <template slot-scope="scope">
            {{scope.row.complateDate}}
          </template>
        </el-table-column>
        <el-table-column
          width="115"
          label="状态">
          <template slot-scope="scope">
            {{scope.row.statusNm}}
          </template>
        </el-table-column>
        <!-- <el-table-column
          prop="planDate"
          label="付款时间"
          width="150">
        </el-table-column>
        <el-table-column
          label="款项金额"
          width="180">
          <template slot-scope="scope">
            ￥{{scope.row.fees}}
          </template>
        </el-table-column>
        <el-table-column
          width="180"
          label="订单总额">
          <template slot-scope="scope">
            ￥{{scope.row.orderAmt}}
          </template>
        </el-table-column>
        <el-table-column
          width="180"
          label="已收">
          <template slot-scope="scope">
            ￥{{scope.row.alFees}}
          </template>
        </el-table-column>
        <el-table-column
          width="180"
          label="待收">
          <template slot-scope="scope">
            ￥{{scope.row.orderAmt-scope.row.alFees}}
          </template>
        </el-table-column>
        <el-table-column
          width="180"
          label="订单类型">
          <template slot-scope="scope">
            {{scope.row.isShop? '非商城':'商城'}}
          </template>
        </el-table-column> -->
        <el-table-column
          width="180"
          fixed="right"
          align="center"
          label="操作">
          <template slot-scope="scope">
            <span class="sail" @click="showPop(scope.row)" v-if="scope.row.statusCd==1">确认收款</span>
            <!--<span class="sail2" v-if="scope.row.statusCd==1">待确认收款</span>-->
            <span class="sail2" v-if="scope.row.statusCd==2">已确认收款</span>
            <span class="sail2" v-if="scope.row.statusCd==3">拒绝确认收款</span>
          </template>
        </el-table-column>
      </el-table>
      <!--<div class="line3">-->
        <!--<p>-->
          <!--订单编号-->
        <!--</p>-->
        <!--<p>-->
          <!--付款单位-->
        <!--</p>-->
        <!--<p>-->
          <!--付款时间-->
        <!--</p>-->
        <!--<p>款项金额</p>-->
        <!--<p>-->
          <!--订单总额-->
        <!--</p>-->
        <!--<p>-->
          <!--{{tabId==1? '已付':'已收'}}-->
        <!--</p>-->
        <!--<p>-->
          <!--{{tabId==1? '未付':'未收'}}-->
        <!--</p>-->
        <!--<p>订单类型</p>-->
        <!--<p v-show="tabId==2">-->
          <!--操作-->
        <!--</p>-->
      <!--</div>-->
      <!--<div class="line4">-->
        <!--<div v-for="(item,index) in list" :key="index">-->
          <!--<p @click="toOrder(item)">{{item.orderCd}}</p>-->
          <!--<p>{{item.payerNm}}</p>-->
          <!--<p>{{item.planDate}}</p>-->
          <!--<p>￥{{item.fees}}</p>-->
          <!--<p>￥{{item.orderAmt}}</p>-->
          <!--<p>￥{{item.alFees}}</p>-->
          <!--<p>￥{{item.orderAmt-item.alFees | numFilter}}</p>-->
          <!--<p>{{item.isShop? '非商城':'商城'}}</p>-->
          <!--<p  v-show="tabId==2">-->
            <!--<span class="sail" @click="showPop(item)" v-if="item.statusCd==1">确认收款</span>-->
            <!--&lt;!&ndash;<span class="sail2" v-if="item.audit==1">等待确认</span>&ndash;&gt;-->
            <!--<span class="sail2" v-if="item.statusCd==2">已确认收款</span>-->
            <!--<span class="sail2" v-if="item.statusCd==3">拒绝确认收款</span>-->
          <!--</p>-->
        <!--</div>-->
      <!--</div>-->
      <div class="line5">
        <el-pagination
          v-if="this.list.length != 0"
          @size-change="handleSizeChange"
          @current-change="handleCurrentChange"
          :current-page="currentPage"
          :page-size="pageSize"
          prev-text="上一页"
          next-text="下一页"
          layout="total, prev, pager, next, jumper"
          :total="total">
        </el-pagination>
      </div>
      <!--下面无用-->
      <!--<div v-if="dialogVisible" class="pop" >-->
        <!--<div class="pop-cont">-->
          <!--<div class="pop-cont-1">-->
            <!--<p>-->
              <!--<span>确认收款</span>-->
              <!--<img :src="deleteIc" @click="dialogVisible=false,search=''">-->
            <!--</p>-->
          <!--</div>-->
          <!--<div class="pop-cont-2">-->
            <!--<p>订单编号：<span>{{info.cd}}</span></p>-->
            <!--<p>付款名称：<span>{{info.payerNm}}</span></p>-->
            <!--<p>订单总额：<span>￥{{info.amt}}</span></p>-->
            <!--<p>未收金额：<span style="color: #E3393C">￥{{info.amt-info.paid | numFilter}}</span></p>-->
          <!--</div>-->
          <!--<div class="pop-cont-3">-->
            <!--<p class="pop-cont-3-t">阶段收款</p>-->
            <!--<div class="pop-cont-3-i" v-for="item in list" :key="item.id">-->
<!--&lt;!&ndash;              <p>{{item.step}}</p>&ndash;&gt;-->
              <!--<p>付款时间：{{item.payDt}}</p>-->
              <!--<p>付款金额：￥<span style="color: red">{{item.payment}}</span></p>-->
              <!--<p  class="pop-has"v-if="item.audit==2"><img :src="checked">已确认收款</p>-->
              <!--<p @click="submit(item)" class="pop-confirm" v-if="item.audit!==2 &&item.audit!==3">确认收款</p>-->
            <!--</div>-->
          <!--</div>-->
        <!--</div>-->
      <!--</div>-->


    </div>
</template>

<script>
    import tobbar from "../../components/person/tobbar";
    import searchIc from '../../assets/img/personal/search.png'
    import deleteIc from '../../assets/img/personal/delete.png'
    import jifen from '../../assets/img/personal/jifen.png'
    import test1 from '../../assets/img/personal/test1.png'
    import successIc from '../../assets/img/success.png'
    import card from "../../components/person/card";
    import featured from "../../components/onlineTrading/featured";
    import cltCard from "../../components/person/cltCard";
    import checked from '../../assets/img/personal/checked.png'
    import {mapState} from "vuex";
    export default {
        name: "allShip",
      data(){
        return{
          searchIc,
          deleteIc,
          successIc,
          jifen,
          test1,
          checked,
          search:'',
          orgEnterId:'',
          dialogVisible:false,
          title:'资金管理',
          info:{},
          form:{
            id: "5001188670231552",
            audit: "2",
            options: "确认收到款"
          },
          tabList:[{
            id:1,
            nm:'采购付款',
          },
            {
              id:2,
              nm:'建造收款',
            },],
          tabList1:[
            {
              id:1,
              nm:'采购付款',
            },
            {
              id:2,
              nm:'建造收款',
            },
          ],
          tabList2:[
            {
              id:1,
              nm:'采购付款',
            },
            {
              id:2,
              nm:'设计收款',
            },
          ],
          tabList3:[
            {
              id:2,
              nm:'监理收款',
            },
            {
              id:1,
              nm:'订单付款',
            },
          ],
          tabList4:[
            {
              id:2,
              nm:'检验检测',
            },
            {
              id:1,
              nm:'订单付款',
            },
          ],
          tabList5:[
            {
              id:2,
              nm:'销售收款',
            },
            {
              id:1,
              nm:'采购付款',
            },
          ],
          tabList6:[
            {
              id:2,
              nm:'监理收款',
            },
            {
              id:1,
              nm:'订单付款',
            },
          ],
          currentPage:1,
          pageSize:10,
          total: 0,
          tabId:1,
          tabIndex:0,
          list:[],
          list2:[],
          currentRole:{},
          userInfo:{},

        }
      },
      layout:'person',
        components:{
            tobbar,
            card,
            featured,
            cltCard
        },
      mounted(){
        this.tabId = this.until.getQueryString('myCd')? this.until.getQueryString('myCd') : 1
        this.getData()
      },
      computed:{
        ...mapState([
          'bWidth',
          'width',
          'loading',
          // 'currentRole'
        ]),
      },
      watch:{
        $route(){
          this.tabId = this.until.getQueryString('myCd')? this.until.getQueryString('myCd') : 1
          this.currentPage=1
          this.getData()
        },
        search(){
          if(this.search == ''){
            this.currentPage = 1
            this.getData()
          }
        },

        currentRole(){
          if(this.currentRole.identityCd=='identity30'){ //船厂
            this.tabList = this.tabList1
          }
          if(this.currentRole.identityCd=='identity20'){ //设计院
            this.tabList = this.tabList2
          }
          if(this.currentRole.identityCd=='identity40'){ //监理监造
            this.tabList = this.tabList3
          }
          if(this.currentRole.identityCd=='identity50'){ //质量检测
            this.tabList = this.tabList4
          }
          if(this.currentRole.identityCd=='identity60'){ //供应商
            this.tabList = this.tabList5

          }
        }
      },
      filters:{
        numFilter(value) {
          const realVal = parseFloat(value).toFixed(2);
          return realVal;
        },
        },
        methods:{
          toOrder(item){
            if(item.isShop){ //非商城跳转合同详情
              if(item.contractId){ //有合同单号
                window.open('./personal/contractDetail?id='+item.contractId)
              }else {
                window.open('./order/orderDetail2?id='+item.orderId)
              }
            }else { //商城跳转订单详情
              window.open('./order/orderDetail?id='+item.orderId)
            }
          },
          submit(item){
            this.form.id = item.id
            console.log('id',this.form.id)
            return
            this.store.commit('changeLoading',true)
            this.api.confirm(this.form).then(res=>{
              this.$message({
                message: '确认成功',
                type: 'success',
                duration:'1500'
              });
            })
            this.dialogVisible=false
            this.search =''
          },
          toSearch(){
            this.currentPage=1
            this.getData()
          },
            handleSizeChange(val) {
                console.log(`每页 ${val} 条`);
            },
          handleCurrentChange(val) {
            this.currentPage = val
            this.getData()
          },
            async getData(){

              let qry = this.query.new()
              this.currentRole=JSON.parse(this.until.seGet('currentRole'))
              this.userInfo=JSON.parse(this.until.seGet('userInfo'))
              this.query.toP(qry,this.currentPage,this.pageSize)
              if(this.search){
                this.query.toW(qry,"cd",this.search,'LK')
              }
              let param={
                orgEnterId:this.currentRole.id
                // orgEnterId: '4982862803145728'//测试
              }
              let data = null
              if(  //非商城收付款记录
                (this.currentRole.identityCd=='identity30' && this.tabId==2) ||
                (this.currentRole.identityCd=='identity20' && this.tabId==2) ||
                (this.currentRole.identityCd=='identity40' && this.tabId==2) ||
                (this.currentRole.identityCd=='identity50' && this.tabId==2) ||
                (this.currentRole.identityCd=='identity60' && this.tabId==2)
              ){
                data=await this.api.payment2(param)
              }else {
                this.query.toW(qry,"payerId",this.userInfo.subId,'LK')
                data=await this.api.payment(this.query.toEncode(qry),param)
              }

              this.list = data.data.list
              this.total=data.page.total
            },
            async chooseTab(item,index){
				if(this.userInfo.pid){
					let flag=0
					let data1=await this.api.getAuthz()//获取当前子账号有的权限
					this.authz=data1.stringPermissions
          //采购付款
					if((this.currentRole.identityCd=='identity30'||this.currentRole.identityCd=='identity20'||this.currentRole.identityCd=='identity60')&&item.id==1){
					  this.authz.forEach(i=>{
					  	if(i=='purchash:payment'){
                flag++
              }
					  })
					}else if(this.currentRole.identityCd=='identity30'&&item.id==2){ //建造收款
					  this.authz.forEach(i=>{
					  	if(i=='build:collect'){
                flag++
              }
					  })
					}

					if(this.currentRole.identityCd=='identity20'&&item.id==2){ //设计收款
					  this.authz.forEach(i=>{
					  	if(i=='design:collection'){
                flag++
              }
					  })
					}
          if(this.currentRole.identityCd=='identity40'&&item.id==2){ //监理收款
            this.authz.forEach(i=>{
            	if(i=='supervision:collection'){
                flag++
              }
            })
          }else if((this.currentRole.identityCd=='identity40'||this.currentRole.identityCd=='identity50')&&item.id==1){ //订单付款
            this.authz.forEach(i=>{
            	if(i=='order:collection'){
                flag++
              }
            })
          }
          if(this.currentRole.identityCd=='identity50'&&item.id==2){ //检验检测
            this.authz.forEach(i=>{
            	if(i=='check:payment'){
                flag++
              }
            })
          }
          if(this.currentRole.identityCd=='identity60'&&item.id==2){ //销售收款
            this.authz.forEach(i=>{
            	if(i=='sales:receipts'){
                flag++
              }
            })
          }


					if(flag==1){
						this.$router.push('./purchase2?myCd='+item.id)
					}else if(flag==0){
						this.$message({
						  message: '当前子账号没有权限',
						  duration:'1500'
						});
					}
				}else{
					this.$router.push('./purchase2?myCd='+item.id)
				}



            },
            showPop(item){
              this.$confirm('是否确认收款?', '提示', {
                confirmButtonText: '确认',
                cancelButtonText: '取消',
                type: 'warning'
              }).then(() => {
                this.store.commit('changeLoading',true)
                this.api.confirm({id:item.id,audit:2}).then(res=>{
                  this.$message({
                    message: '确认成功',
                    type: 'success',
                    duration:'1500'
                  });
                  this.getData()
                })

              })

              // this.dialogVisible=true
              // this.info = item
              // this.search = item.cd
              // this.getData()
            },
        },


    }
</script>

<style scoped lang="less">
  @import url("../../assets/css/init.less");
  .main{
    width: 100%;
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
    /*搜索框*/
    .line1{
      width: 100%;
      display: flex;
      align-items: center;
      height: 78px;
      padding-left: 29px;
      padding-right: 16px;

      .search{

        display: flex;
        align-items: center;
        p:nth-of-type(1){
          width: 250px;
          border-top: 1px solid #DFDFDF;
          border-bottom: 1px solid #DFDFDF;
          border-left: 1px solid #DFDFDF;
          border-top-left-radius: 5px;
          border-bottom-left-radius: 5px;
          height: 38px;
          padding: 0 12px;
          align-items: center;
          display: flex;
          input{
            border: none;
            outline: none;
            color: #666666;
          }
          ::-webkit-input-placeholder { /* Chrome/Opera/Safari */
            color: #999999;
          }
          ::-moz-placeholder { /* Firefox 19+ */
            color: #999999;
          }
          :-ms-input-placeholder { /* IE 10+ */
            color: #999999;
          }
          :-moz-placeholder { /* Firefox 18- */
            color: #999999;
          }
        }
        p:nth-of-type(2){
          flex-shrink: 0;
          background:#2778BE;
          height: 38px;
          display: flex;
          align-items: center;
          justify-content: center;
          width: 55px;
          border-top-right-radius: 5px;
          border-bottom-right-radius: 5px;
          &:hover{
            cursor: pointer;
          }
          img{
            width: 18px;
          }
        }

      }
    }


    .line2{
      padding: 0 30px;
      margin-top: 15px;
      margin-bottom: 30px;
      width: 100%;

      div{
        /*padding: 0 30px;*/
        border-bottom:1px solid #2778BE;
        display: flex;
        height: 42px;
        align-items: center;
        position: relative;

        p{

          width: 129px;
          display: flex;
          align-items: center;
          justify-content: center;
          height: 100%;
          /*对所有属性过渡0.2s*/
          transition: all 0.2s;
          &:hover{
            cursor: pointer;
          }
        }
        .btm{
          position: absolute;
          width: 64px;
          height: 100%;
          border-bottom: 2px solid #2778BE;
          /*对left属性过渡5秒*/
          transition: left 0.5s;
        }
      }


    }


    .line3{
      display: flex;
      align-items: center;
      height: 60px;
      padding: 0 31px;
      background:#F7F7F7;
      p{
        flex: 0.5;
        text-align: center;

      }
      p:nth-of-type(1){
        flex:1.2;
      }
      p:nth-of-type(2){
        flex:0.7;
        text-align: center;
      }
      p:nth-of-type(3){
        flex:0.8;
      }
      p:nth-of-type(4){
        flex:0.6;
      }
      p:nth-of-type(5){
        flex:0.6;
        text-align: center;
      }
      p:nth-of-type(7){
        flex:0.6;
        text-align: center;
      }
      p:nth-of-type(8){
        flex:0.5;
        text-align: center;
      }

    }
    .line4{
      div{
        display: flex;
        height: 76px;
        align-items: center;
        border-bottom: 1px solid #DFDFDF;
        padding: 0 31px;
        color: #333333;
        font-size: 15px;
        flex: 1;
        p{
          flex: 0.5;
          text-align: center;

        }
        p:nth-of-type(1){
          flex:1.2;
          cursor: pointer;
        }
        p:nth-of-type(2){
          flex:0.7;
        }
        p:nth-of-type(3){
          flex:0.8;
        }
        p:nth-of-type(4){
          flex:0.6;
          text-align: center;
        }
        p:nth-of-type(5){
          flex:0.6;
          text-align: center;
        }
        p:nth-of-type(7){
          flex:0.6;
          text-align: center;
        }
        p:last-of-type{
          flex: 0.5;
          display: flex;
          justify-content: center;
          flex-direction: column;
          span:hover{
            cursor: pointer;
          }

        }
      }
    }
    .line5{
      padding: 30px 40px;
      width: 100%;
      display: flex;
      justify-content: flex-end;
    }

  }

  .active2{
    color: white;
    background: #2778BE;
    /*border-bottom: 2px solid #2572B5;*/
  }

  .pop{
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    background: rgba(0,0,0,0.1);
    .pop-cont{
      position: absolute;
      top: 50%;
      left: 50%;
      transform: translate(-50%,-50%);
      width:491px;
      height:771px;
      background:rgba(255,255,255,1);
      border-radius:8px;
      display: flex;
      flex-direction: column;
      .pop-cont-1{
        p{
          width: 100%;
          height: 78px;
          display: flex;
          align-items: center;
          justify-content: center;
          position: relative;
          font-size:18px;
          font-family:Microsoft YaHei;
          font-weight:400;
          color:rgba(51,51,51,1);
          border-bottom: 1px solid #DFDFDF;
          img{
            position: absolute;
            width:20px;
            height:20px;
            top:20px;
            right: 20px;
            &:hover{
              cursor: pointer;
            }
          }
        }
      }
      .pop-cont-2{
        padding: 30px;
        border-bottom: 1px solid #DFDFDF;
        p{
          margin-bottom:20px ;
          &:last-child{
            margin-bottom: 0;
          }
          span{
            margin-left: 5px;
          }

        }
      }
      .pop-cont-3{
        padding: 30px;
        flex: 1;
        overflow-y: scroll;
        .pop-cont-3-t{
          font-size:16px;
          font-family:Microsoft YaHei;
          font-weight:400;
          color:rgba(51,51,51,1);
        }
        .pop-cont-3-i{
          position: relative;
          width: 100%;
          height: 116px;
          background: #EFEFEF;
          padding: 19px;
          display: flex;
          flex-direction: column;
          justify-content: space-between;
          margin-top: 20px;
          border-radius: 4px;
          p:nth-of-type(1){

          }
          p:nth-of-type(2){

          }
          p:nth-of-type(3){

          }
          .pop-has{
            position: absolute;

            height: 18px;
            top:50%;
            transform: translateY(-50%);
            right: 20px;
            display: flex;
            align-items: center;
            color: #666666;
            img{
              width: 18px;
              margin-right: 10px;
            }


          }
          .pop-confirm{
            position: absolute;
            top:50%;
            transform: translateY(-50%);
            right: 20px;
            width:89px;
            height:33px;
            background:rgba(39,120,190,1);
            border-radius:3px;
            display: flex;
            align-items: center;
            justify-content: center;
            color: white;
            &:hover{
              cursor: pointer;
            }
            &:active{
              opacity: 0.5;
            }
          }
        }
      }



    }
  }

</style>
