<template>
<!--我的推荐-->
  <div id="home">
    <div class="title">
      <p><span class="lineC"></span>我的推荐</p>
      <p @click="back"><i class="el-icon-arrow-left"></i>返回</p>
    </div>
    <!--<div class="content1">-->
      <!--<p>推荐客户：<span>{{this.recommend}}</span></p>-->
      <!--<p>成交数量：<span>{{this.dealNumber}}</span></p>-->
      <!--<p>获得佣金：<span class="textRed">{{this.coin}}</span></p>-->
      <!--<button>申请提现</button>-->
    <!--</div>-->
    <div class="content2">
      <el-table
        :data="List"
        style="width: 100%">
        <el-table-column
          type="index"
          label="序号"
          align="center"
          width="80">
        </el-table-column>
        <el-table-column
          prop="tDsCommissionBuyerRealNm"
          label="客户名称"
          >
        </el-table-column>
        <el-table-column
          prop="tDsCommissionType"
          label="客户类型"
          >
        </el-table-column>
        <el-table-column
          prop="tDsCommissionBuyerRealNm"
          label="联系人"
          >
        </el-table-column>
        <el-table-column
          prop="tDsCommissionBuyerPhone"
          label="联系电话"
          >
        </el-table-column>
        <el-table-column
          prop="crtTm"
          label="成交时间"
          >
        </el-table-column>
        <!--<el-table-column-->
          <!--prop="style"-->
          <!--label="状态"-->
          <!--width="110">-->
        <!--</el-table-column>-->
        <!--<el-table-column-->
          <!--prop="getCode"-->
          <!--label="获得佣金"-->
          <!--width="120"-->
          <!--fixed="right">-->
        <!--</el-table-column>-->
      </el-table>
<!--      &lt;!&ndash;原生&ndash;&gt;-->
<!--      <div class="titleA">-->
<!--        <p>序号</p>-->
<!--        <p>客户名称</p>-->
<!--        <p>联系人</p>-->
<!--        <p>联系电话</p>-->
<!--        <p>推荐时间</p>-->
<!--        <p>状态</p>-->
<!--        <p>获得佣金</p>-->
<!--      </div>-->
<!--      <div class="list" v-for="(item,index) in List" :key="index">-->
<!--        <p>{{item.cd}}</p>-->
<!--        <p>{{item.nm1}}</p>-->
<!--        <p>{{item.nm}}</p>-->
<!--        <p>{{item.tel}}</p>-->
<!--        <p>{{item.tm}}</p>-->
<!--        <p>{{item.style}}</p>-->
<!--        <p>{{item.getCode}}</p>-->
<!--      </div>-->
    </div>
    <div class="Footer">
      <el-pagination
        background
        @current-change="handleCurrentChange"
        :current-page.sync="pageNo"
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
    name: "myRecommend",
    components: { },
    data(){
      return{
        currentPage3: 1,
        recommend:'12',
        coin:'￥2,0000.00',
        dealNumber:'10',
        pageNo:1,
        pageSize:20,
        total:0,
        List:[],
      }
    },
    computed:{
      ...mapState([
        'bWidth',
        'width'
      ])
    },
    mounted(){
      this.getList()
    },
    methods:{
      async getList(){
        let qry = this.query.new()
        this.query.toP(qry,this.pageNo,this.pageSize)
        let data = await this.api.myRecommend(this.query.toEncode(qry))
        this.List = data.data.list
        console.log(this.list)
        this.List.forEach(item=>{
          item.crtTm = item.crtTm?item.crtTm.split(' ')[0]:''
        })
        this.total = data.page.total
      },

      handleCurrentChange(val) {
        this.pageNo = val
        this.getList()
      },
      back(){
        this.$router.go(-1)
      },
    },
  }
</script>

<style lang="less">
  .content2{
    .el-table__header thead{
      color: #666666;
      font-size:16px;
    }
    .el-table__header th{
      background-color: #F7F7F7;
      height: 60px;
      border-bottom: 1px solid #F7F7F7;
    }
    .el-table__body tr{
      color: #333333;
      font-size:15px;
      height: 76px;
    }
    .el-table__fixed-right{
      height: 100%!important;
    }
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
  .content1{
    width: 100%;
    display: inline-flex;
    align-items: center;
    justify-content: flex-end;
    height: 81px;
    padding-right: 20px;
    box-sizing: border-box;
    >p{
      font-size: 18px;
      color: #333333;
      line-height: 19px;
    }
    >button{
      height: 41px;
      width: 126px;
      background-color: #2778BE;
      color: #FFFFFF;
      border: 0;
      margin-left: 18.9%;
      border-radius: 4px;
      cursor: pointer;
    }
    .textRed{
      color: #E4393C;
    }
  }
  .content2{
    display: flex;
    width: 100%;
    justify-content: space-between;
    /*.titleA{*/
    /*  display: inline-flex;*/
    /*  height: 60px;*/
    /*  align-items: center;*/
    /*  justify-content: space-between;*/
    /*  background-color: #F7F7F7;*/
    /*  font-size: 16px;*/
    /*  color: #666666;*/
    /*  p:nth-child(1){*/
    /*    padding-left: 34px;*/
    /*    width: 66px;*/
    /*  }*/
    /*  p:nth-child(2){*/
    /*    width: 182px;*/
    /*  }*/
    /*  p:nth-child(3){*/
    /*    width: 75px;*/
    /*  }*/
    /*  p:nth-child(4){*/
    /*    width: 146px;*/
    /*  }*/
    /*  p:nth-child(5){*/
    /*    width: 125px;*/
    /*  }*/
    /*  p:nth-child(6){*/
    /*    width: 60px;*/
    /*  }*/
    /*  p:nth-child(7){*/
    /*    padding-right: 34px;*/
    /*    width: 100px;*/
    /*  }*/
    /*}*/
    /*.list{*/
    /*  display: inline-flex;*/
    /*  height: 76px;*/
    /*  align-items: center;*/
    /*  justify-content: space-between;*/
    /*  width: 100%;*/
    /*  font-size: 15px;*/
    /*  color: #333333;*/
    /*  p:nth-child(1){*/
    /*    padding-left: 34px;*/
    /*    width: 66px;*/
    /*  }*/
    /*  p:nth-child(2){*/
    /*    width: 182px;*/
    /*  }*/
    /*  p:nth-child(3){*/
    /*    width: 75px;*/
    /*  }*/
    /*  p:nth-child(4){*/
    /*    width: 146px;*/
    /*  }*/
    /*  p:nth-child(5){*/
    /*    width: 125px;*/
    /*  }*/
    /*  p:nth-child(6){*/
    /*    width: 60px;*/
    /*  }*/
    /*  p:nth-child(7){*/
    /*    padding-right: 34px;*/
    /*    width: 100px;*/
    /*    color: #E3393C;*/
    /*  }*/
    /*}*/
  }
  .Footer{
    margin-top: 40px;
    width: 100%;
    display: inline-flex;
    justify-content: center;
  }
</style>
