<template>
<!--我的合同 包括 船舶合同和产品合同-->
<div id="home">
  <div class="title">
    <p v-if="this.cd ==1"><span class="lineC"></span>船舶合同</p>
    <!-- 如果是供应商显示的是销售合同 -->
    <p v-if="this.cd ==2"><span class="lineC"></span>{{currentRole.identityCd=='identity60'?'销售合同':'产品合同'}}</p>
    <p v-if="this.cd ==31"><span class="lineC"></span>设计销售合同</p>
    <p v-if="this.cd ==32"><span class="lineC"></span>设计采购合同</p>
    <p v-if="this.cd ==4"><span class="lineC"></span>采购合同</p>
    <p v-if="this.cd ==5"><span class="lineC"></span>建造合同</p>
    <p v-if="this.cd ==6"><span class="lineC"></span>建造监理合同</p>
    <p v-if="this.cd ==7"><span class="lineC"></span>检验检测合同</p>
    <p @click="back"><i class="el-icon-arrow-left"></i>返回</p>
  </div>
  <div class="search">
    <!--<div class="selected">-->
      <!--<span>合同类型：</span>-->
      <!--<el-select v-model="value" placeholder="请选择合同" class="border1">-->
        <!--<el-option-->
          <!--v-for="item in options"-->
          <!--:key="item.cd"-->
          <!--:label="item.nm"-->
          <!--:value="item.cd">-->
        <!--</el-option>-->
      <!--</el-select>-->
    <!--</div>-->
    <div class="searched">
      <input v-model="search" placeholder="搜索合同名称" />
      <button @click="toSearch"><i class="el-icon-search"></i></button>
    </div>
  </div>
  <div class="ctDetail" v-for="(item,index) in ctList" :key="index" @click="toDetail(item.id)">
    <!-- <p>合同编号：<span>{{item.contractNo}}</span></p>
    <p>合同名称：<span>{{item.nm}}</span></p>
    <p>合同总额：<span class="red">￥{{item.totalPrice}}元</span></p>
    <p>签约日期：<span>{{item.signTm}}</span></p>
    <p>已付金额：<span>￥{{item.paid}}元</span></p>
    <i class="el-icon-arrow-right"></i>
    <p>交付日期：<span>{{item.deliveryTm}}</span></p>
    <p>未付金额：<span>￥{{item.nPaid}}元</span></p> -->
    <div class="ctDetail-left">
      <div class="ctDetail-item" style="margin-bottom: 10px;">
        <p>合同编号：<span>{{item.contractNo}}</span></p>
        <p style="width: 45%;">合同名称：<span>{{item.nm}}</span></p>
        <p style="width: 25%;">签约日期：<span>{{item.signTm}}</span></p>
      </div>
      <div class="ctDetail-item">
        <p>合同总额：<span>￥{{item.totalPrice}}元</span></p>
        <p style="width: 45%;">已付金额：<span>￥{{item.paid}}元</span></p>
        <p style="width: 25%;">未付金额：<span class="red">￥{{item.nPaid}}元</span></p>
      </div>
    </div>
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
      layout:'person',
      name: "myContract",
      components: { },
      data(){
        return{
          cd:'',
          search:'',
          total:0,
          pageSize:10,
          currentPage3: 1,
          ctList:[],
          options: [],
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
        $route(){
          this.cd = this.until.getQueryString('myCd')
          this.getOptions()
        },
        // value(e){
        //   if(this.value=='CONTRACT_TYPE.06'){
        //     this.$router.push('../personal/myContract?myCd=1')
        //     // this.value='CONTRACT_TYPE.06'
        //   }else if(this.value=='CONTRACT_TYPE.07'){
        //     this.$router.push('../personal/myContract?myCd=2')
        //     // this.value='CONTRACT_TYPE.07'
        //   }else if(this.value=='CONTRACT_TYPE.01'){
        //     this.$router.push('../personal/myContract?myCd=3')
        //     // this.value='CONTRACT_TYPE.01'
        //   }else if(this.value=='CONTRACT_TYPE.08'){
        //     this.$router.push('../personal/myContract?myCd=4')
        //     // this.value='CONTRACT_TYPE.08'
        //   }else if(this.value=='CONTRACT_TYPE.02'){
        //     this.$router.push('../personal/myContract?myCd=5')
        //     // this.value='CONTRACT_TYPE.02'
        //   }else if(this.value=='CONTRACT_TYPE.03'){
        //     this.$router.push('../personal/myContract?myCd=6')
        //     // this.value='CONTRACT_TYPE.03'
        //   }else if(this.value=='CONTRACT_TYPE.04'){
        //     this.$router.push('../personal/myContract?myCd=7')
        //     // this.value='CONTRACT_TYPE.04'
        //   }
        // },
      },

      mounted() {
        this.currentRole=JSON.parse(this.until.seGet('currentRole'))
        this.cd= this.until.getQueryString('myCd')
        this.getOptions()
        // this.getInfo()
      },
      methods:{
        async getOptions(){
          // this.options=[]
          // let data1=await this.api.dataDictionary('CONTRACT_TYPE')
          // data1.forEach((item,index)=>{
          //   if(this.currentRole.identityCd=='identity10'){
          //     if(item.nm=='船舶合同'||item.nm=='产品合同'){
          //       this.options.push({value: item.cd,label: item.nm})
          //     }
          //   }
          //   if(this.currentRole.identityCd=='identity20'){
          //     if(item.nm=='设计合同'||item.nm=='采购合同'){
          //       this.options.push({value: item.cd,label: item.nm})
          //     }
          //   }
          //   if(this.currentRole.identityCd=='identity30'){
          //     if(item.nm=='采购合同'||item.nm=='建造合同'){
          //       this.options.push({value: item.cd,label: item.nm})
          //     }
          //   }
          //   if(this.currentRole.identityCd=='identity40'){
          //     if(item.nm=='监理合同'||item.nm=='船舶合同'||item.nm=='产品合同'){
          //       this.options.push({value: item.cd,label: item.nm})
          //     }
          //   }
          //   if(this.currentRole.identityCd=='identity50'){
          //     if(item.nm=='检验合同'||item.nm=='船舶合同'||item.nm=='产品合同'){
          //       this.options.push({value: item.cd,label: item.nm})
          //     }
          //   }
          //   if(this.currentRole.identityCd=='identity60'){
          //     if(item.nm=='采购合同'||item.nm=='销售合同'){
          //       this.options.push({value: item.cd,label: item.nm})
          //     }
          //   }
          // })
          if(this.cd=='1'){
            this.value='CONTRACT_TYPE.06'
          }else if(this.cd=='2'){
            this.value='CONTRACT_TYPE.07'
          }else if(this.cd=='31'){  //设计采购合同
            this.value='CONTRACT_TYPE.08'
          }else if(this.cd=='32'){ //设计销售合同
            this.value='CONTRACT_TYPE.01'
          }else if(this.cd=='4'&&this.currentRole.identityCd!='identity30'&&this.currentRole.identityCd!='identity60'){
            this.value='CONTRACT_TYPE.06'
          }else if(this.cd=='4'&&(this.currentRole.identityCd=='identity30'||this.currentRole.identityCd=='identity60')){
            this.value='CONTRACT_TYPE.07'
          }else if(this.cd=='5'){
            this.value='CONTRACT_TYPE.02'
          }else if(this.cd=='6'){ //监理合同
            this.value='CONTRACT_TYPE.03'
          }else if(this.cd=='7'){
            this.value='CONTRACT_TYPE.04'
          }
          this.getInfo()
          console.log(this.value)
        },
        async getInfo(){
          this.ctList=[]
          let qry = this.query.new()
          this.query.toP(qry,this.currentPage3,this.pageSize)
          this.query.toO(qry,'crtTm','desc')
          if(this.currentRole.identityCd=='identity10'&&this.cd ==1){//船东身份除产品合同都是船舶合同
            this.query.toW(qry,'contractTypeCd','CONTRACT_TYPE.07','NE')
          }else if(this.currentRole.identityCd=='identity10'&&this.cd ==2){
            this.query.toW(qry,'contractTypeCd','CONTRACT_TYPE.07','EQ')
          }else{
            this.query.toW(qry,'contractTypeCd',this.value,'EQ')
          }

          if((this.currentRole.identityCd=='identity60'||this.currentRole.identityCd=='identity30')&&this.cd ==4){
            this.query.toW(qry,'partyaId',this.currentRole.id,'EQ')
          }else if(this.currentRole.identityCd=='identity60'&&this.cd ==2){
            this.query.toW(qry,'partybId',this.currentRole.id,'EQ')
          }
          let param={
            keyWord:this.search,
            orgEnterId:this.currentRole.id
          }
          let data=await this.api.contractList(this.query.toEncode(qry),param)
          this.total=data.page.total
          this.ctList=data.data.list
          this.ctList.forEach((item,index)=>{
            item.signTm = item.signTm.substring(0,10)
            item.totalPrice= item.totalPrice.toFixed(2)
            item.paid=item.paid.toFixed(2)
            item.nPaid=(item.totalPrice-item.paid).toFixed(2)
            item.totalPrice = this.formatNumberRgx(item.totalPrice)
            item.paid = this.formatNumberRgx(item.paid)
            item.nPaid = this.formatNumberRgx(item.nPaid)
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
        formatNumberRgx(num) {
            let parts = num.toString().split(".")
            parts[0] = parts[0].replace(/\B(?=(\d{3})+(?!\d))/g, ",")
            return parts.join(".")
        }
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
  .ctDetail{
    width: 90%;
    margin: 0 auto;
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
      width: 95%;
      display: flex;
      flex-direction: column;
      .ctDetail-item {
        display: flex;
        align-items: center;
        p {
          display: flex;
          align-items: center;
          width: 30%;
          span {
            max-width: 65%;
            display: inline-block;
            overflow: hidden;
            text-overflow: ellipsis;
            white-space: nowrap;
          }
        }
      }
    }
    // p:nth-child(1){
    //   width: 100%;
    // }
    // >p{
    //   width: 45%;
    //   color: #333333;
    //   font-size: 15px;
    // }
  }
  .Footer{
    margin-top: 40px;
    width: 100%;
    display: inline-flex;
    justify-content: center;
  }
</style>
