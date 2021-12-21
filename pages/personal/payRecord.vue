<template>
<!--  资金管理-->
    <div class="main">
      <div style="padding:0 30px">
        <tobbar :title="title" :showAll="false"></tobbar>
      </div>
      <p style="border-bottom: 1px solid rgba(0,0,0,0.1);"></p>
      <div class="line2">
        <p style="margin-right: 10px">订单编号：</p>
        <div class="search">
          <p>
            <input placeholder="订单编号查询" v-model="cd">
          </p>

          <p @click="toSearch"><img :src="searchIc"></p>
        </div>
      </div>
      <div class="line22" v-if="tabId">

        <div>
          <p v-for="(item,index) in tabList" :key="item.id"  :class="{active2: tabId==item.id}" @click="chooseTab(item,index)">
            <span>{{item.nm}}</span>
          </p>
          <!--          30*(tabIndex+1)加上一个间距-->
          <!--          <span class="btm" :style="{left:tabIndex*64+30*(tabIndex+1)+'px'}"></span>-->
        </div>
      </div>
      <div class="line22" v-if="!tabId || tabId===2">

        <div>
          <p v-for="(item,index) in tabList2" :key="item.id"  :class="{active2: tabId2==item.id}" @click="chooseTab2(item.id)">
            <span>{{item.nm}}</span>
          </p>
          <!--          30*(tabIndex+1)加上一个间距-->
          <!--          <span class="btm" :style="{left:tabIndex*64+30*(tabIndex+1)+'px'}"></span>-->
        </div>
      </div>
      <el-table
        v-if="tabId==1"
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
          prop="orderAmt"
          label="订单总额"
          width="145">
          <template slot-scope="scope">
            ￥{{fmoney(scope.row.orderAmt)}}
          </template>
        </el-table-column>
        <el-table-column
          width="145"
          label="计划完成金额">
          <template slot-scope="scope">
            ￥{{fmoney(scope.row.fees)}}
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
            ￥{{fmoney(scope.row.alFees)}}
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
          prop="alFees"
          label="已收">
          <template slot-scope="scope">
            ￥{{scope.row.alFees}}
          </template>
        </el-table-column>
        <el-table-column
          prop="alFees"
          label="待收">
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
        v-if="!tabId || tabId==2"
        :data="list"
        :header-cell-style="{background:'#f7f7f7'}"
        style="width: 100%">
        <el-table-column
          label="订单编号"
          width="180"
          v-if="tabId2==22">
          <template slot-scope="scope">
            <p @click="toOrder(scope.row)" class="cursor">{{scope.row.orderCd}}</p>
          </template>
        </el-table-column>
        <el-table-column
          label="合同编号"
          width="180"
          v-if="tabId2==11">
          <template slot-scope="scope">
            <p @click="toOrder(scope.row)" class="cursor">{{scope.row.contractNo}}</p>
          </template>
        </el-table-column>
        <el-table-column
          label="订单总额"
          width="145">
          <template slot-scope="scope">
            ￥{{fmoney(scope.row.orderAmt)}}
          </template>
        </el-table-column>
        <el-table-column
          width="145"
          label="计划完成金额">
          <template slot-scope="scope">
            ￥{{fmoney(scope.row.fees)}}
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
            ￥{{fmoney(scope.row.alFees)}}
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
          prop="alFees"
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
        <el-table-column
          label="操作" v-if="tabId">
          <template slot-scope="scope">
            <span class="sail" @click="confirm(scope.row)" v-if="scope.row.statusCd==1">确认收款</span>
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
          <!--订单总额-->
        <!--</p>-->
        <!--<p>-->
          <!--{{tabId==1? '已收' : '已付'}}-->
        <!--</p>-->
        <!--<p>-->
          <!--{{tabId==1? '待收' : '待付'}}-->
        <!--</p>-->
        <!--<p>订单类型</p>-->
        <!--<p v-show="tabId==2"></p>-->
      <!--</div>-->
      <!--<div class="line4">-->
        <!--<div v-for="(item,index) in list" :key="index">-->
          <!--<p @click="toOrder(item)">{{item.orderCd}}</p>-->
<!--&lt;!&ndash;          <p>{{item.payerNm}}</p>&ndash;&gt;-->
<!--&lt;!&ndash;          <p>{{item.payDt}}</p>&ndash;&gt;-->
          <!--<p>￥{{item.orderAmt}}</p>-->
          <!--<p>￥{{item.alFees}}</p>-->
          <!--<p>￥{{item.orderAmt-item.alFees | numFilter}}</p>-->
          <!--<p>{{item.isShop? '非商城':'商城'}}</p>-->
          <!--<p  v-show="tabId==2">-->
            <!--<span class="sail" @click="confirm(item)">确认收款</span>-->
          <!--</p>-->
        <!--</div>-->
      <!--</div>-->

      <div class="footPage">
        <!-- <page-ination :total="total" :pageSize="pageSize" @changeP='pageChange'></page-ination> -->
        <el-pagination
          background
          @current-change="handleCurrentChange"
          :current-page.sync="currentPage"
          :page-size="pageSize"
          layout="prev, pager, next, jumper"
          :total="total">
        </el-pagination>
      </div>

    </div>
</template>

<script>
    import {mapState} from "vuex";
    import tobbar from "../../components/person/tobbar";
    import searchIc from '../../assets/img/personal/search.png'
    import deleteIc from '../../assets/img/personal/delete.png'
    import successIc from '../../assets/img/success.png'
    export default {
        name: "allShip",
        components:{
            tobbar,
        },
      computed: {
        ...mapState([
          // 'currentRole'
        ])
      },
        data(){
            return{
                title:'资金管理',
               tabId:'',
                searchIc,
                deleteIc,
                successIc,
                value: '',
              cd:'',
              tabList:[
                {
                  id:1,
                  nm:'收款记录',
                },
                {
                  id:2,
                  nm:'付款记录',
                },
              ],
              tabId2:11,
              tabList2:[
                {
                  id:11,
                  nm:'船舶付款',
                },
                {
                  id:22,
                  nm:'商城付款',
                },
              ],
              list:[],
              currentPage:1,
              pageSize:20,
              total:0,
              userInfo:{},
              currentRole:{},
            }
        },
        layout:'person',
        mounted(){

          this.tabId = this.until.getQueryString('myCd') ? this.until.getQueryString('myCd') : ''
          this.tabId2 = this.until.getQueryString('myCd2') ? this.until.getQueryString('myCd2') : ''
           console.log(11111,this.tabId, this.tabId2);
           console.log(this.tabId===2);
          let info = this.tabList.find(item=>item.id==this.tabId)
          if(info){
            this.title = info.nm
          }
          this.getData()
        },
      filters:{
        numFilter(value) {
          const realVal = parseFloat(value).toFixed(2);
          return realVal;
        },
      },
      methods:{
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
        //跳转订单详情
        toOrder(item){
          if(!item.orderId){ //非商城跳转合同详情
            window.open('./personal/contractDetail?id='+item.contractId)
          }else { //商城跳转订单详情
            window.open('./order/orderDetail?id='+item.orderId+'&cd='+item.orderCd)
          }
        },
          //选择卡
          chooseTab(item,index){
            console.log(111,item.id);
            this.$router.push('./payRecord?myCd='+item.id)
          },
        //选择卡
       async chooseTab2(id){
          // this.tabId2 = item.id
          // this.currentPage1=1
          // this.getData()
          console.log(id);
         if(this.tabId2 == id){
           return
         }
         // console.log('1111111111')
         //  console.log(id)
          if(this.userInfo.pid){
            let flag=0
            let data1=await this.api.getAuthz()//获取当前子账号有的权限
            this.authz=data1.stringPermissions

            if(id==11){    //船舶付款
              // console.log('船舶')
              this.authz.forEach(i=>{
                if(i=='payment:recordsShip'){
                  flag++
                }
              })
            }else if(id==22 && this.currentRole.identityCd!="identity10"){//商城订单
              // console.log('商城')
              this.authz.forEach(i=>{
                if(i=='payment:recordsShop'){
                  flag++
                }
              })
            }
            if(flag==1){
              this.$router.push('../personal/payRecord?myCd2='+id)
            }else if(flag==0){
              this.$message({
                message: '当前子账号没有权限',
                duration:'1500'
              });
            }
          }else{
            this.$router.push('../personal/payRecord?myCd2='+id)
          }

        },
        handleCurrentChange(val){
          this.currentPage1=val
          this.getData()
        },
          //获取数据
          async getData(){
            let qry = this.query.new()
            this.currentRole=JSON.parse(this.until.seGet('currentRole'))
            this.userInfo=JSON.parse(this.until.seGet('userInfo'))
            this.query.toP(qry,this.currentPage,this.pageSize)
            if(this.cd) {
              this.query.toW(qry,"orderCd",this.cd,'LK')
            }
            let data = null
            let param={
              orgEnterId:this.currentRole.id
            }
            if(this.currentRole.identityCd=='identity70' && this.tabId==2){ //经销商 收款
               data=await this.api.payment2(param)
            }else if(this.currentRole.identityCd=='identity70' && this.tabId==1){  //付款
              this.query.toW(qry,"payeeId",this.userInfo.subId,'LK')
              data=await this.api.payment(this.query.toEncode(qry),param)
            }else {
              if(!this.tabId || this.tabId===2){
                if(this.tabId2==22) {
                  this.query.toW(qry,'payerId',this.currentRole.id+'','EQ')
                  let param2 = {
                    type: 1
                  }
                  data=await this.api.payment3(this.query.toEncode(qry),param2)
                } else {
                  this.query.toW(qry,'payerId',this.currentRole.id+'','EQ')
                  let param2 = {
                    type: 0
                  }
                  data=await this.api.payment3(this.query.toEncode(qry),param2)
                }
              }
            }
            this.list = data.data.list
            this.total=data.page.total
          },
         //搜索
         toSearch(){
            this.currentPage = 1
            this.getData()
         },
         //确认收款
          confirm(item){
            let param = {
              id: item.id,
              audit: "2",
              options: "确认收到款"
            }
            this.store.commit('changeLoading',true)
            this.api.confirm(param).then(res=>{
              this.$message({
                message: '确认成功',
                type: 'success',
                duration:'1500'
              });
            })
          }
        },
        watch:{
          $route(){
            this.tabId = this.until.getQueryString('myCd') ? this.until.getQueryString('myCd') : ''
            this.tabId2 = this.until.getQueryString('myCd2') ? this.until.getQueryString('myCd2') : ''
            let info = this.tabList.find(item=>item.id==this.tabId)
            if(info){
              this.title = info.nm
            }else {
              this.title = '资金管理'
            }
            this.currentPage = 1
            this.getData()
          }
        }
    }
</script>

<style scoped lang="less">
  @import url("../../assets/css/init.less");
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
  .cursor{
    cursor: pointer;
  }
  .footPage{
    display: flex;
    justify-content: center;
    margin: 44px 0 20px;
    .btn_confirm{
      margin-left: 43px;
    }
  }
  .main{
    width: 100%;
    .line2{
      width: 100%;
      display: flex;
      align-items: center;
      height: 78px;
      padding-left: 29px;
      padding-right: 16px;
      div{
        flex:1;
        display: flex;
        align-items: center;
      }
      .slt{
        margin-right: 30px;
        p{
          margin-right: 10px;
        }
      }
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
    .line22{
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
      .active2{
        color: white;
        background: #2778BE;
        /*border-bottom: 2px solid #2572B5;*/
      }

    }
    .line3{
      display: flex;
      align-items: center;
      height: 60px;
      padding: 0 31px;
      background:#F7F7F7;
      font-size: 16px;
      color: #656565;
      p{
        text-align: center;
        flex:1;
      }
      /*p:nth-of-type(1){*/
        /*text-align: center;*/
        /*flex:1;*/
      /*}*/
      /*p:nth-of-type(2){*/
        /*text-align: center;*/
        /*flex:1;*/
      /*}*/
      /*p:nth-of-type(3){*/
        /*text-align: center;*/
        /*flex:1;*/
      /*}*/
      /*p:nth-of-type(4){*/
        /*text-align: center;*/
        /*flex:1;*/
      /*}*/


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
        p{
          text-align: center;
          flex:1;
        }
        p:nth-of-type(1){

          cursor: pointer;
        }
        p:nth-of-type(2){

        }
        p:nth-of-type(3){
          flex:1;
          text-align: center;

        }
        p:nth-of-type(4){
          flex:1;
          text-align: center;
          color: #E3393C;
        }

      }


    }
  }




</style>
