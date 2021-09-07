<template>
  <div class="left1">
    <!--客户档案-->
    <div class="document">
      <div class="title">
        <p><img src="@/assets/img/personal/首页/客户档案.png" alt=""/>客户档案<span class="more" @click="toPage('./profile')">查看更多<i class="el-icon-arrow-right"></i></span></p>
      </div>
      <div class="content2">
        <el-table
          :data="documentList"
          @row-click="toDetail"
          style="width: 100%">
          <el-table-column
            type="index"
            align="center"
            width="60"
            label="序号">
          </el-table-column>
          <el-table-column
            prop="custNm"
            width="160"
            label="客户名称">
          </el-table-column>
          <el-table-column
            width="70"
            prop="linkman"
            align="center"
            label="联系人">
          </el-table-column>
          <el-table-column
            width="160"
            prop="mob"
            align="center"
            label="联系电话">
          </el-table-column>
          <el-table-column
            align="left"
            label="客户地址">
            <template slot-scope="scope">
              {{scope.row.addrNm}}-{{scope.row.address}}
            </template>
          </el-table-column>
        </el-table>
      </div>
    </div>
    <!--收款记录-->
    <div class="money">
      <div class="title">
        <p><img src="@/assets/img/personal/首页/收款记录.png" alt=""/>收款记录<span class="more" @click="toPage('./payRecord?myCd=1')">查看更多<i class="el-icon-arrow-right"></i></span></p>
      </div>
      <div class="content2">
        <el-table
          :data="moneyList"
          style="width: 100%">
          <el-table-column
            prop="cd"
            align="center"
            width="90"
            label="订单编号">
          </el-table-column>
          <el-table-column
            prop="payerNm"
            width="190"
            label="付款单位">
          </el-table-column>
          <el-table-column
            width="95"
            prop="amt"
            align="center"
            label="订单金额">
          </el-table-column>
          <el-table-column
            width="95"
            prop="ym"
            align="center"
            class-name="blue"
            label="已收">
          </el-table-column>
          <el-table-column
            prop="am"
            width = "95"
            align="center"
            class-name="red"
            label="未收">
          </el-table-column>
          <el-table-column
            align="center"
            width="90"
            fixed="right"
            prop="operations"
            label="操作">
            <template slot-scope="scope">
              <div class="btnList">
                <button class="button3" @clcik="confirm(scope.row)">确认收款</button>
              </div>
            </template>
          </el-table-column>
        </el-table>
      </div>
    </div>
  </div>
</template>

<script>
  import {mapState} from "vuex";
  export default {
    name: "shipowner",
    components: {
    },
    data(){

      return {

        moneyList: [{
          no: '0123456',
          nm: '宁波中创海洋科技有限公司',
          mn: '￥100,000',
          ym: '￥8,000',
          am: '￥100,000',
        }, {
          no: '0123456',
          nm: '宁波中创海洋科技有限公司',
          mn: '￥100,000',
          ym: '￥8,000',
          am: '￥100,000',
        }, {
          no: '0123456',
          nm: '宁波中创海洋科技有限公司',
          mn: '￥100,000',
          ym: '￥8,000',
          am: '￥100,000',
        }, {
          no: '0123456',
          nm: '宁波中创海洋科技有限公司',
          mn: '￥100,000',
          ym: '￥8,000',
          am: '￥100,000',
        }],
        documentList: [{
          no: '1',
          nm: '宁波聚联科技',
          pn: '张先生',
          tel: '123456789012',
          add: '浙江宁波镇海329创业社区',
        }, {
          no: '2',
          nm: '宁波聚联科技',
          pn: '张先生',
          tel: '123456789012',
          add: '浙江宁波镇海329创业社区',
        }, {
          no: '3',
          nm: '宁波聚联科技',
          pn: '张先生',
          tel: '123456789012',
          add: '浙江宁波镇海329创业社区',
        }, {
          no: '4',
          nm: '宁波聚联科技',
          pn: '张先生',
          tel: '123456789012',
          add: '浙江宁波镇海329创业社区',
        }, {
          no: '5',
          nm: '宁波聚联科技',
          pn: '张先生',
          tel: '123456789012',
          add: '浙江宁波镇海329创业社区',
        }],

      }

    },
    computed:{
      ...mapState([
        "currentRole",
      ])
    },
    watch:{

    },
    mounted(){

      this.getData()
    },
    methods:{
      async getData(){
        this.getCutList()
        this.getList()
      },
      //客户档案
      async  getCutList(){
        let qry=this.query.new();
        this.query.toP(qry,1,5)
        this.query.toO(qry,'crtTm',"desc")
        let res= await this.api.custInfoList(this.query.toEncode(qry))
        this.documentList=res.data.list
        // this.total=20
      },
      //收款记录
      async getList(){
        let qry = this.query.new()
        this.query.toP(qry,1,4)
        let param={
          orgEnterId:this.currentRole.id
          // orgEnterId: '4982862803145728'//测试
        }
        let data=await this.api.payment(this.query.toEncode(qry),param)
        this.moneyList = data.data.list
      },
      toPage(url){
        if(url){
          this.$router.push(url)
        }
      },
      //客户档案详情
      toDetail(item){
        this.$router.push({
          path:'/personal/profileDetail',
          query:{
            info:JSON.stringify(item)
          }
        })
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
  }
</script>
<style lang="less">
  .left1{
    .content2{
      //选中时更换字体 背景颜色   需设置高亮才能生效
      .el-table__body tr.current-row>td{
        background-color: #FFFFFF !important;
        color: #2778BE;
      }
      .red{
        color: #E3393C;
      }
      .blue{
        color: #2778BE;
      }
      .btnList{
        display: flex;
        justify-content: space-around;
        flex-direction: column;
        .button3{
          width: 74px;
          height: 31px;
          background-color: #2778BE;
          color: #FFFFFF;
          border: none;
          border-radius: 3px;
        }
      }
      .el-table__header thead{
        color: #666666;
        font-size:14px;
      }
      .el-table__header th{
        background-color: #F7F7F7;
        height: 52px;
      }
      .el-table__body tr{
        color: #333333;
        font-size:15px;
        height: 73px;
      }
    }
  }
</style>

<style lang="less" scoped>
  @import url("../../assets/css/init.less");
  .left1{
    width: 67.3%;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    padding-top: 31px;
    .title{
      width: 100%;
      height: 56px;
      box-sizing: border-box;
      border-bottom: 1px solid #F3F3F3;
      .more{
        float: right;
        font-size: 12px;
        color: #666666;
        margin-right: 30px;
        cursor: pointer;
      }
      >p{
        font-size: 16px;
        color: #333333;
        line-height: 56px;
        margin-left: 20px;
      }
      img{
        padding-right: 10px;
        height: 19px;
        vertical-align: sub;
      }
    }
    /*经销商 客户档案*/
    .document{
      width: 100%;
      background-color: #FFFFFF;
      margin-bottom: 20px;
      min-height: 473px;
    }
    /*经销商 收款记录 */
    .money{
      width: 100%;
      background-color: #FFFFFF;
      min-height: 400px;
    }
  }
</style>
