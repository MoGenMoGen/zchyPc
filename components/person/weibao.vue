<template>
<!--  维保记录-->
    <div>
      <div class="line3">
        <p>
          序号
        </p>
        <p>
          服务单号
        </p>
        <p>
          设备名称
        </p>
        <p>
          问题描述
        </p>
        <p>
          备注信息
        </p>
        <p>
          提交时间
        </p>
        <p>
          完成时间
        </p>
      </div>
      <div class="line4">
        <div v-for="(item,index) in list" :key="index">
          <p>{{index+1}}</p>
          <p>{{item.billCd}}</p>
          <p>{{item.deviceNm}}</p>
          <p>{{item.description}}</p>
          <p>{{item.rmks}}</p>
          <p>{{item.submitTm}}</p>
          <p>{{item.completeTm}}</p>
        </div>
      </div>
      <div class="Footer">
        <el-pagination
          background
          @size-change="handleSizeChange"
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
    export default {
        name: "weibao",

        data(){
            return{
              total:0,
              pageSize:5,
              currentPage3: 1,
              list:[],
              currentRole:{},

            }
        },
        mounted() {
          this.currentRole=JSON.parse(this.until.seGet('currentRole'))
          this.id= this.until.getQueryString('id')
          this.getList()
        },
        methods:{
          async getList(){
            let qry = this.query.new()
            let param={
              orgId:this.currentRole.id
            }
            this.query.toW(qry,'docsId',this.id,'EQ')
            this.query.toP(qry,this.currentPage3,this.pageSize)
            let data=await this.api.maintenR(this.query.toEncode(qry),param)
            this.total=data.page.total
            this.list=data.data.list
            console.log('订单')
            console.log(this.list)
          },
          handleSizeChange(val) {
            console.log(`每页 ${val} 条`);
          },
          handleCurrentChange(val) {
            console.log(`当前页: ${val}`);
            this.currentPage3=val
            this.getList()
          },
        },
    }
</script>

<style scoped lang="less">
  .line3{
    display: flex;
    align-items: center;
    height: 60px;
    padding: 0 31px;
    background:#F7F7F7;
    font-size: 14px;
    color: #666666;
    p{
      padding: 0 5px;
    }
    p:nth-of-type(1){
      flex:0.5;
    }
    p:nth-of-type(2){
      flex:1.5;
    }
    p:nth-of-type(3){
      flex:1;
    }
    p:nth-of-type(4){
      flex:1.5;
    }
    p:nth-of-type(5){
      flex:1;
    }
    p:nth-of-type(6){
      flex:1;
        text-align: center;

    }
    p:nth-of-type(7){
      flex:1;
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
      p:nth-of-type(1){
        flex:0.5;
      }
      p:nth-of-type(2){
        flex:1.5;
      }
      p:nth-of-type(3){
        flex:1;
      }
      p:nth-of-type(4){
        flex:1.5;
      }
      p:nth-of-type(5){
        flex:1;
      }
      p:nth-of-type(6){
        flex:1;
        text-align: center;
      }
      p:nth-of-type(7){
        flex:1;
        text-align: center;


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
