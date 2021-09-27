<template>
  <div>
    <!-- 发票帮助中心 -->
    <div class="list-box">
      <p v-for="(item,idnex) in list" :key="index" @click="toDetail(item.id)">· {{item.title}}</p>
    </div>
    <div class="footPage">
      <el-pagination background @current-change="handleCurrentChange" :current-page.sync="currentPage1"
        :page-size="pageSize" layout="prev, pager, next, jumper" :total="total">
      </el-pagination>
    </div>
  </div>
</template>

<script>
  export default {
    name: "billHelp",
    props: {

    },
    data() {
      return {
        list: [],
        currentPage1: 1,      //前往哪页
        pageSize:20,       //每页数量
        total:0,         //总数
      }
    },
    mounted() {
      this.getList()
    },
    methods: {
      toDetail(id) {
        this.$router.push(`./myBillHelpDetail?id=${id}`)
      },
      handleCurrentChange(val){
        this.currentPage1=val
        this.list=[]
        this.getList()
      },
      getList() {
        let qry = this.query.new()
        this.query.toP(qry,this.currentPage1,this.pageSize)
        this.api.getBillHelpList(this.query.toEncode(qry)).then(res => {
          this.list = res.list
        })
      }
    }
  }
</script>

<style scoped lang="less">
  .list-box {
    display: flex;
    flex-direction: column;
    width: 100%;
    padding: 20px 30px;
    box-sizing: border-box;
    font-size: 14px;
    color: #333;
    line-height: 28px;
    >p {
      margin: 10px 0;
      cursor: pointer;
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
