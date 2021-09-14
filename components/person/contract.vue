<template>
<!--我的合同 包括 船舶合同和产品合同-->
<div id="home">
  <div class="allDetail">
    <p>总金额：<span class="red">￥10.00元</span></p>
    <p>已付金额：<span class="red">￥10.00元</span></p>
    <p>未付金额：<span class="red">￥10.00元</span></p>
  </div>
  <div class="ctDetail" v-for="(item,index) in ctList" :key="index" @click="toDetail(item.id)">
    <div class="ctDetail-left">
      <div class="ctDetail-item" style="margin-bottom: 10px;">
        <p>合同编号：<span>{{item.contractNo}}</span></p>
        <p>船舶名称：<span>{{item.nm}}</span></p>
        <p>签约日期：<span>{{item.signTm}}</span></p>
      </div>
      <div class="ctDetail-item">
        <p>合同金额：<span>￥{{item.totalPrice}}元</span></p>
        <p>已付金额：<span>￥{{item.paid}}元</span></p>
        <p>未付金额：<span class="red">￥{{item.nPaid}}元</span></p>
      </div>
    </div>
    <!-- <p>交付日期：<span>{{item.deliveryTm}}</span></p> -->
    <i class="el-icon-arrow-right"></i>
  </div>
  <div class="Footer">
    <el-pagination
      background
      @current-change="handleCurrentChange"
      :current-page.sync="currentPage3"
      :page-size="pageSize"
      layout="prev, pager, next, jumper"
      :total="total">
    </el-pagination>
  </div>
</div>
</template>

<script>
  import {mapState} from "vuex";

    export default {
      name: "contract",
      components: { },
      data(){
        return{
          id: '',
          total:0,
          pageSize:10,
          currentPage3: 1,
          ctList:[],
          value: '',
          currentRole:{},
        }
      },
      computed:{
        ...mapState([
          'bWidth',
          'width'
        ])
      },
      watch:{

      },

      mounted() {
        this.id= this.until.getQueryString('id')
        this.currentRole=JSON.parse(this.until.seGet('currentRole'))
        this.getInfo()
      },
      methods:{
        async getInfo(){
          this.ctList=[]
          let qry = this.query.new()
          this.query.toP(qry,this.currentPage3,this.pageSize)
          this.query.toO(qry,'crtTm','desc')
          let param={
            docsId:this.id,
            orgEnterId:this.currentRole.id
          }
          let data=await this.api.shipContractList(this.query.toEncode(qry),param)
          this.total=data.page.total
          this.ctList=data.data.list
          this.ctList.forEach((item,index)=>{
            item.totalPrice=item.totalPrice.toFixed(2)
            item.paid=item.paid.toFixed(2)
            item.nPaid=(item.totalPrice-item.paid).toFixed(2)
            this.$set(this.ctList,index,this.ctList[index])
          })
          console.log(this.ctList)
        },
        toSearch(){
          this.currentPage3 = 1
          this.getInfo()
        },
        handleSizeChange(val) {
          console.log(`每页 ${val} 条`);
        },
        handleCurrentChange(val) {
          console.log(`当前页: ${val}`);
          this.currentPage3=val
          this.getInfo()
        },
        toDetail(id){
          this.$router.push('./contractDetail?id='+id)
        },
        back(){
          this.$router.go(-1)
        },
      },
    }
</script>

<style lang="less">
  .el-input__inner::placeholder {
    color: #999999;
  }
</style>
<style lang="less" scoped>
  @import url("../../assets/css/init.less");
#home{
  margin-top: -30px;
}
  .title{
    height: 59px;
    border-bottom: 1px solid #F3F3F3;
    width: 100%;
    display: inline-flex;
    justify-content: space-between;
    line-height: 59px;
    p:nth-child(1){
      color: #333333;
      font-size: 20px;
      display: flex;
      align-items: center;
      margin-left: 30px;
      .lineC{
        display: block;
        height: 18px;
        width: 3px;
        background-color: #2778BE;
        margin-right: 8px;
      }
    }
    p:nth-child(2) {
      color: #666666;
      font-size: 14px;
      margin-right: 24px;
      cursor: pointer;
    }

  }
  .search{
    height: 78px;
    border-bottom: 1px solid #F3F3F3;
    display: inline-flex;
    align-items: center;
    justify-content: space-around;
    .selected{
      margin-left: 34px;
      .border1{
        width: 300px;
        /*border: 1px solid #DFDFDF;*/
        border-radius: 5px;
      }
    }
    .searched{
      margin-left: 30px;
      height: 38px;
      line-height: 38px;
      border-right: 0;
      border-radius: 5px;
      display: flex;
      display: -webkit-flex;
      align-items: center;
      >input{
        flex: 1;
        height: 100%;
        text-indent: 15px;
        border: 1px solid @themeColor;
        border-right: 0;
        box-sizing: border-box;
        border-radius: 2px 0 0 2px;
        width: 300px;
        color: #999999;
      }
      button{
        background: @themeColor;
        border: 1px solid @themeColor;
        color: #ffffff;
        height: 100%;
        padding: 0 20px;
        border-radius: 0 2px 2px 0;
        cursor: pointer;
        box-sizing: border-box;
      }
    }
  }
  .allDetail {
    border-bottom: 1px solid #F3F3F3;
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 20px 15px;
    box-sizing: border-box;
    width: 100%;
    font-size: 19px;
    p {
      width: 33%;
      .red {
        color: #FF3E3E;
      }
    }
  }
  .ctDetail{
    border-bottom: 1px solid #F3F3F3;
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 20px 15px;
    cursor: pointer;
    .red{
      color: #FF3E3E;
    }
    i{
      color: #333333;
    }
    .ctDetail-left {
      width: 90%;
      display: flex;
      flex-direction: column;
      .ctDetail-item {
        display: flex;
        align-items: center;
        p {
          width: 33%;
        }
      }
    }
  }
  .Footer{
    margin-top: 40px;
    width: 100%;
    display: inline-flex;
    justify-content: center;
  }
</style>
