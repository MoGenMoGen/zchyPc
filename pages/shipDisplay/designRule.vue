<template>
  <div id="home" :style="{width:bWidth + 'px'}">
    <div class="header">
      <img :height="bannerHeight+'px'" class="adert" alt="" src="../../assets/img/shipDisplay/设计规范-大.png" :style="{width:bWidth + 'px'}"/>
      <el-breadcrumb separator-class="el-icon-arrow-right" :style="{width:width+'px'}" class="bc main">
        <el-breadcrumb-item :to="{ path: './shipMan' }">船舶管理</el-breadcrumb-item>
        <el-breadcrumb-item :to="{ path: './shipDesign' }">船舶设计</el-breadcrumb-item>
        <el-breadcrumb-item ><p class="textRed">设计规范</p></el-breadcrumb-item>
      </el-breadcrumb>
    </div>
    <div class="content main" :style="{width:width + 'px'}">
      <div class="partA">
        <rule-menu type="1" @getList="getList"></rule-menu>
      </div>
      <div class="partB">
        <div class="info" v-for="(item,index) in list" :key="index"  @click="toPage(item)">
          <div class="min">
            <div class="left">
              <p>{{item.nm}}</p>
              <p>{{item.cont}}</p>
            </div>
            <div class="right">
              <p>查看详情<i class="el-icon-arrow-right"></i></p>
            </div>
          </div>
        </div>
        <div class="pageInfo">
          <el-pagination
            v-if="list"
            background
            @current-change="handleCurrentChange"
            :current-page.sync="pageNo"
            :page-size="pageSize"
            layout="prev, pager, next"
            :total="total">
          </el-pagination>
        </div>
      </div>
    </div>
    <div class="footerImg" :style="{width:bWidth+'px'}">
      <img src="../../assets/img/footer.png" alt=""/>
    </div>
  </div>
</template>

<script>
  import RuleMenu from "./components/ruleMenu";
  import {mapState} from "vuex";
  export default {
    name: "designRule",
    data(){
      return{
        id:'',
        bannerHeight:450,
        pageNo:1,
        pageSize:10,
        total:0,
        list:[],
      }
    },
    components: {RuleMenu},
    computed: {
      ...mapState([
        'bWidth',
        'width',
        'currentRole'
      ])
    },
    watch:{

      $route(e){
        this.id = this.until.getQueryString('cid') ?  this.until.getQueryString('cid') : ''
        this.getList()
      },
    },
    async asyncData(context){
      if(process.client){
        return
      }
      let qry = context.app.query.new()
      context.app.query.toW(qry,'pid','4942035725390848','EQ')
      context.app.query.toO(qry,'seq','asc')
      let classify = await context.app.api.getClassify(context.app.query.toEncode(qry))
      let id = context.query.cid ?context.query.cid : classify[0].id

      let qry2 = context.app.query.new()
      context.app.query.toW(qry2,'cid',id,'EQ')
      // context.app.query.toW(qry2,'type',1,'EQ')
      context.app.query.toP(qry2,1,100)
      let data = await context.app.api.standardList(context.app.query.toEncode(qry2))

      return{
        classify:classify,
        data:data
      }
    },
    mounted(){
      this.id = this.until.getQueryString('cid') ?  this.until.getQueryString('cid') : ''

      this.getList()
    },
    methods:{
      toPage(item){
        let url = './designDetail?id='+item.id+"&cid="+this.id
        this.$router.push(url)
      },
      handleCurrentChange(val){
        this.pageNo = val
        this.getList()
      },
      async getList(val){
        let qry = this.query.new()
        if(!this.id && val){
          this.id = val
        }
        console.log('获取数据',this.id)
        this.query.toW(qry,'cid',this.id,'EQ')
        // this.query.toW(qry,'type',1,'EQ')
        this.query.toP(qry,this.pageNo,this.pageSize)
        let data = await this.api.standardList(this.query.toEncode(qry))
        this.list = data.data.list
        this.list.forEach(item =>{
          item.cont = item.cont.replace(/<\/?[^>]*>/g, "").slice(0,150) + '...';
          item.cont = item.cont.replace(/(&nbsp;)/g, "");
          // console.log(item.id)
        })
        this.total = data.page.total
      }
    }
  }

</script>

<style lang="less" scoped>
  @import url("../../assets/css/init.less");
  #home{
    background-color: #FFFFFF;
    display: flex;
    flex-wrap: wrap;
    .pageInfo{
      margin-top: 20px;
    }
    .textRed{
      color: @themeColor;
    }
    .bc{
      height: 54px;
      display: flex;
      align-items: center;
    }
  }
  .header{
    width: 100%;
  }
  .content{
    padding-top: 50px;
    display: flex;
    .partA{
      /*width: 13.3%;*/
    }
    .partB{
      padding: 0 40px;
      flex: 1;
      .info{
        display: flex;
        padding: 20px 10px;
        box-sizing: border-box;
        border-bottom: 1px solid #E8E8E8;
        cursor: pointer;
        &:hover{
          background: #F8F8F8;
        }
        &:first-of-type{

        }
        .min{
          display: flex;
          display: -webkit-flex;
          align-items: center;

          .left{
            flex: 1;
            p:nth-child(1){
              font-size: 16px;
              color: #333333;
              line-height: 30px;
            }
            p:nth-child(2){
              color: #666666;
              font-size: 12px;
              line-height: 25px;

            }
          }
          .right{
            display: flex;
            align-items: center;
            padding-left: 15px;
            p{
              color: #999999;
              cursor: pointer;
              font-size: 12px;
              margin: auto;
            }
          }
        }

      }
    }
  }
  .footerImg{
    width: 100%;
    margin-top: 53px;
  }

</style>
