<template>
<!--  产品合同   未使用该页面-->
  <div id="home">
    <div class="title">
      <p><span class="lineC"></span>产品合同</p>
      <p @click="back"><i class="el-icon-arrow-left"></i>返回</p>
    </div>
    <div class="search">
      <div class="selected">
        <span>合同类型：</span>
        <el-select v-model="value" placeholder="建造合同" class="border1">
          <el-option
            v-for="item in options"
            :key="item.value"
            :label="item.label"
            :value="item.value">
          </el-option>
        </el-select>
      </div>
      <div class="searched">
        <input v-model="search" placeholder="搜索合同名称" />
        <button @click="toSearch"> <i class="el-icon-search"></i></button>
      </div>
    </div>
    <div class="ctDetail" v-for="(item,index) in ctList" :key="index" @click="toPage">
      <p>合同编号：<span>{{item.ct1}}</span></p>
      <p>船舶名称：<span>{{item.ct2}}</span></p>
      <p>合同总额：<span>￥{{item.ct3}}</span></p>
      <p>签约日期：<span>{{item.ct4}}</span></p>
      <p>已付金额：<span>￥{{item.ct5}}</span></p>
      <p>交付日期：<span>{{item.ct6}}</span></p>
      <p>未付金额：<span>￥{{item.ct7}}</span></p>
    </div>
    <div class="Footer">
      <el-pagination
        background
        @size-change="handleSizeChange"
        @current-change="handleCurrentChange"
        :current-page.sync="currentPage3"
        :page-size="54"
        layout="prev, pager, next, jumper"
        :total="800">
      </el-pagination>
    </div>
  </div>
</template>

<script>
  import {mapState} from "vuex";
  export default {
    layout:'person',
    name: "ptContract",
    components: { },
    data(){
      return{
        search:'',
        currentPage3: 1,
        ctList:[{
          ct1:'ZCHY20200515120108',
          ct2:'新型 4000KW 中型休闲渔船',
          ct3:'¥ 132,000.00',
          ct4:'2020-04-16',
          ct5:'¥ 100,000.00',
          ct6:'2020-12-16',
          ct7:'¥ 32,000.00'
        },{
          ct1:'ZCHY20200515120108',
          ct2:'新型 4000KW 中型休闲渔船',
          ct3:'¥ 132,000.00',
          ct4:'2020-04-16',
          ct5:'¥ 100,000.00',
          ct6:'2020-12-16',
          ct7:'¥ 32,000.00'
        },{
          ct1:'ZCHY20200515120108',
          ct2:'新型 4000KW 中型休闲渔船',
          ct3:'¥ 132,000.00',
          ct4:'2020-04-16',
          ct5:'¥ 100,000.00',
          ct6:'2020-12-16',
          ct7:'¥ 32,000.00'
        }],
        options: [{
          value: '选项1',
          label: '建造合同'
        }, {
          value: '选项2',
          label: '设备合同'
        }],
        value: ''
      }
    },
    computed:{
      ...mapState([
        'bWidth',
        'width'
      ])
    },
    methods:{
      toSearch(){

      },
      handleSizeChange(val) {
        console.log(`每页 ${val} 条`);
      },
      handleCurrentChange(val) {
        console.log(`当前页: ${val}`);
      },
      toPage(){
        this.$router.push('./contractDetail')
      },
      back(){
        this.$router.go(-1)
      },
    },
  }
</script>


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
        border: 1px solid #DFDFDF;
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
    height: 181px;
    border-bottom: 1px solid #F3F3F3;
    display: flex;
    flex-wrap: wrap;
    padding: 30px 35px;
    cursor: pointer;
    p:nth-child(1){
      width: 100%;
    }
    p:nth-child(3){
      color: #FF3E3E;
      font-size: 15px;
    }
    >p{
      width: 50%;

    }
  }
  .Footer{
    margin-top: 40px;
    width: 100%;
    display: inline-flex;
    justify-content: center;
  }
</style>
