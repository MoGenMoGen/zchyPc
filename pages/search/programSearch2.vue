<template>
<div id="home" :style="{width:bWidth + 'px'}">
  <div class="header main" >
    <el-breadcrumb class="main" separator-class="el-icon-arrow-right" :style="{width:width + 'px'}">
      <el-breadcrumb-item :to="{ path: './index' }">全部结果</el-breadcrumb-item>
      <el-breadcrumb-item ><p class="textRed">{{search.keywords}}</p></el-breadcrumb-item>
    </el-breadcrumb>
  </div>
  <div class="content main" :style="{width:width + 'px'}">
    <div class="part" v-for="(item,index) in list" :key="index">
      <img :src="item.imgUrl" :style="{height: width*0.16 + 'px'}" />
      <p v-html="item.nm"></p>
      <p v-html="item.typesnm"></p>
    </div>
  </div>
  <div class="foot">
    <el-pagination
      v-if="this.list != ''"
      background
      @current-change="handleCurrentChange"
      :current-page.sync="search.pageNo"
      :page-size="search.size"
      layout="prev, pager, next, jumper"
      :total="total">
    </el-pagination>
  </div>
  <div class="footerImg">
    <img src="../../assets/img/footer.png" alt=""/>
  </div>
</div>
</template>

<script>
  import {mapState} from "vuex";

  export default {
        name: "programSearch",
      data(){
          return{
            total: 0,
            list:[],
            search:{
              keywords:'',
              pageNo:1,
              size:10,
            },
            time:0,
          }
      },
      computed: {
        ...mapState([
          'bWidth',
          'width',
          'currentRole',
          'typeSize'
        ])
      },
     watch:{
        $route(e){
          //路由变化时截取搜索关键词
            let arr = this.until.getQueryString('keyWord')
            this.search.keywords = arr
            console.log(this.time)
            setTimeout(()=>{
              this.time=arr+this.until.getQueryString('time')
            },200)
            // this.getData()

        },
        time(){
          this.getData()
        }
      },
      mounted() {
          //url截取搜索关键词
        let arr = this.until.getQueryString('keyWord')
        this.search.keywords = arr
        this.time=arr+this.until.getQueryString('time')
        // this.getData()
      },
      methods:{
          //获取数据
          async getData(){
            let qry = this.query.new()
            this.query.toW(qry,'nm',this.search.keywords,'LK')
            this.query.toO(qry,'seq','asc')
            this.query.toP(qry,this.search.pageNo,this.search.size)
            let data = await this.api.shipList(this.query.toEncode(qry))
            data.data.list.forEach(item => {
              item.nm = item.nm.replace(new RegExp(this.search.keywords,'g'),`<span style='color:#dd4b39'>${this.search.keywords}</span>`)
            })
            this.list = data.data.list
            this.total=data.page.total
          },
        //此时分页
        handleCurrentChange(val){
          this.search.pageNo = val
          this.getData()
        },
      },
    }
</script>

<style lang="less" scoped>
  @import url("../../assets/css/init.less");
  #home{
    background-color: #FFFFFF;
  }
  .header{
    height: 40px;
    display: flex;
    align-items: center;
    width: 100%;
    .textRed{
      color: #dd4b39;
      font-weight: bold;
    }
  }
  .content{
    margin-top: 20px;
    display: flex;
    flex-direction: row;
    flex-wrap: wrap;
    .part{
      width: 17%;
      margin-bottom: 40px;
      margin-right: 3.75%;
      >img{
        width: 100%;
      }
      >p{
        width: 100%;
        text-align: center;
        line-height: 30px;
      }
      &:nth-of-type(5n){
        margin-right: 0;
      }
    }



  }
  .foot{
    width: 100%;
    flex: 1;
    display: flex;
    justify-content: center;
    margin-bottom: 70px;
    margin-top: 50px;
  }
  .footerImg{
    width: 100%;
  }
</style>
