<template>
  <!--店铺首页-->
  <div id="home" :style="{width:bWidth+'px'}">
    <div class="header" v-if="info.id != null">
      <store-title  @changeTab="changeTab"  @search="search" :info="info"  ></store-title>
    </div>

    <el-carousel :height="bannerHeight+'px'" v-if="tabId === 0">
      <el-carousel-item v-for="item in bannerList" :key="item">
        <div class="banner"><img :src="item"></div>
      </el-carousel-item>
    </el-carousel>
    <div class="content main" :style="{width:width+'px'}"  v-if="tabId !== '-1'">
      <div class="top" >
        <div class="productLeft">
          <button @click="changeActive(item)" :class="{active:item.checked}" v-for="(item,index) in filterList" :key="index">
            {{item.nm}}
            <img :src="item.not" v-if="!item.checked" alt=""/>
            <img :src="item.asc" v-if="item.checked && item.sort==='asc'" alt=""/>
            <img :src="item.desc" v-if="item.checked && item.sort==='desc'" alt=""/>
          </button>
        </div>
        <div class="productRight">
          <el-pagination
            @size-change="handleSizeChange"
            @current-change="handleCurrentChange"
            :current-page.sync="pageNo"
            layout="total, prev, pager, next"
            :page-size="pageSize"
            :total="total">
          </el-pagination>
        </div>
      </div>
      <div class="mid" :style="{width:width+'px'}">
        <product-list v-if="tabId !== -1" :list="goodList"></product-list>
        <!--      店铺简介，富文本格式-->
        <div v-else class="mid-1">
          <div v-html="info.synopsis"></div>
        </div>
      </div>
      <div class="bottom" :style="{width:width+'px'}" v-if="tabId !== '-1'">
        <el-pagination
          @size-change="handleSizeChange"
          @current-change="handleCurrentChange"
          :current-page.sync="pageNo"
          :page-size="pageSize"
          layout="prev, pager, next, jumper"
          :total="total">
        </el-pagination>
      </div>
    </div>
    <shop-info v-if="tabId === '-1'" :info="info"></shop-info>
    <div class="footerImg" :style="{width:bWidth+'px'}">
      <img src="../../assets/img/footer.png" alt=""/>
    </div>
  </div>
</template>

<script>
  import {mapState} from "vuex";
  import StoreTitle from "../../components/onlineTrading/storeTitle";
  import ProductList from "../../components/onlineTrading/productList";
  import icDown from "../../assets/img/onlineTrade/向下.png"
  import icUp from "../../assets/img/onlineTrade/向上.png"
  import icDown2 from "../../assets/img/onlineTrade/向下-灰.png"
  import icUp2 from "../../assets/img/onlineTrade/向上-灰.png"

  import shopInfo from "./storeIntrodution";

  export default {
    name: "storeHome",
    data() {
      return {
        icUp,
        icDown,
        icUp2,
        icDown2,
        pageNo: 1,
        pageSize: 10,
        total: 0,//分页总数
        id: '',
        bannerHeight: 0,
        tabId: 0,//默认首页
        info: {},//店铺信息
        bannerList: [],
        goodList: [],//商品列表
        filterList: [
          {
            nm: '综合',
            asc: icUp,
            desc: icDown,
            not: icDown2,//没有选中
            checked: true,
            sort: "desc"
          },
          {
            nm: '销量',
            asc: icUp,
            desc: icDown,
            not: icDown2,//没有选中
            checked: false,
            sort: "desc",
          },
          {
            nm: '价格',
            asc: icUp,
            not: icDown2,//没有选中
            desc: icDown,
            checked: false,
            sort: "desc",
          },

        ],
        filterTxt: '综合',//排序字段
        sortTxt: 'desc',//升序还是降序

      }
    },
    computed: {
      ...mapState([
        'bWidth',
        'width'
      ])
    },
    watch: {
      bWidth(){
        if(this.bannerList.length>0){
          let myImg = new Image()
          myImg.src = this.bannerList[0]
          myImg.onload = () =>{
            this.bannerHeight = this.bWidth*myImg.height/myImg.width
          }
        }
      },
    },
    components: {ProductList, StoreTitle,shopInfo},
    async asyncData(context){
      if(process.client){
        return
      }
      let id = context.query.shopId
      let info = await context.app.api.shopIntro(id)

      let arr = null
      let qry = context.app.query.new()
      context.app.query.toW(qry, 'shopId', id, 'EQ')
      context.app.query.toP(qry, 1, 100)
      if(context.query.keyWord){
        context.app.query.toW(qry, 'nm', context.query.keyWord, 'LK')
      }
      arr = await context.app.api.shopGoodList(context.app.query.toEncode(qry))
      return {
        info:info,
        arr:arr,
      }
    },
    mounted() {

      this.id = this.until.getQueryString('shopId')
      this.getInfo()
      //this.tabId = this.until.getQueryString('tabId')
      if(!this.until.getQueryString('keyWord')){
        this.getList()
      }
      //this.id =this.$route.query.id


    },
    methods: {
      async getInfo() {
        this.info = await this.api.shopIntro(this.id)
        this.bannerList = this.info.bannerImg.split(",")
        let myImg = new Image()
        myImg.src = this.bannerList[0]
        myImg.onload = () =>{
          this.bannerHeight = this.bWidth*myImg.height/myImg.width
        }
      },
      //  设置排序字段和顺序
      setOrder(qry) {
        if (this.filterTxt === '综合') {
          this.query.toO(qry, 'topSeq', this.sortTxt)
        } else if (this.filterTxt === '销量') {
          this.query.toO(qry, 'salesQty', this.sortTxt)
        } else if (this.filterTxt === '价格') {
          this.query.toO(qry, 'origPrice', this.sortTxt)
        }
      },

      //  商品列表
      getList() {
        let qry = this.query.new()
        this.query.toW(qry, 'shopId', this.id, 'EQ')
        this.setOrder(qry)
        this.query.toP(qry, this.pageNo, this.pageSize)
        //设置分类
        if (this.tabId !== -1 && this.tabId !== 0 && this.tabId !== 1) {
          this.query.toW(qry, 'tid', this.tabId, 'EQ')
        }
        this.api.shopGoodList(this.query.toEncode(qry)).then((res) => {
          this.goodList = res.data.list
          this.total = res.page.total
        })


      },


      //切换分类tab,子组件上传
      changeTab(item) {
        this.tabId = item.id
        this.pageNo = 1
        this.getList()
      },
      //店内搜索
      search(val){
        this.tabId = 1;
        this.pageNo = 1
        let qry = this.query.new()
        this.query.toW(qry, 'shopId', this.id, 'EQ')
        this.query.toW(qry, 'nm', val, 'LK')
        this.setOrder(qry)
        this.query.toP(qry, this.pageNo, this.pageSize)
        //设置分类
        if (this.tabId !== -1 && this.tabId !== 0 && this.tabId !== 1) {
          this.query.toW(qry, 'tid', this.tabId, 'EQ')
        }
        this.api.shopGoodList(this.query.toEncode(qry)).then((res) => {
          this.goodList = res.data.list
          this.total = res.page.total
        })


        console.log(val)
      },
      changeActive(item) {
        if (item.checked) {
          item.sort = item.sort == "asc" ? "desc" : "asc"
          this.sortTxt = item.sort
        } else {
          this.filterList.forEach(item => {
            item.checked = false
            item.sort = 'desc'
          })
          item.checked = true
          this.sortTxt = item.sort
          this.filterTxt = item.nm
        }
        this.pageNo = 1
        this.getList()
      },
      handleSizeChange(val) {
        console.log(`每页 ${val} 条`);
      },
      handleCurrentChange(val) {
        this.pageNo = val
        this.getList()
      },
    },
  }
</script>

<style lang="less">
  #home {
    /*卡片图片*/

    .el-carousel__item h3 {
      color: #475669;
      font-size: 14px;
      opacity: 0.75;
      line-height: 200px;
      margin: 0;
    }

    .el-carousel__item:nth-child(2n) {
      /*background-color: #99a9bf;*/
    }

    .el-carousel__item:nth-child(2n+1) {
      /*background-color: #d3dce6;*/
    }

    /*热门设备图片*/

    .ensure {
      .el-image {
        width: 100%;
        height: 100%;
      }
    }
  }
</style>

<style lang="less" scoped>
  @import url("../../assets/css/init.less");

  #home {
    background-color: #F3F3F3;

  }

  .header {

  }

  .content {
    background-color: #F3F3F3;
    display: flex;
    justify-content: center;
    flex-wrap: wrap;

    .top {
      height: 40px;
      margin-top: 30px;
      width: 100%;
      display: flex;
      align-items: center;
      justify-content: space-between;
      background-color: #FFFFFF;
      margin-bottom: 10px;

      .productLeft {
        display: flex;
        align-items: center;
        padding-left: 10px;

        > button {
          background-color: #ffffff;
          color: #666666;
          border: 1px solid #DDDDDD;
          width: 60px;
          height: 26px;
          font-size: 12px;
          cursor: pointer;
        }

        img {
          width: 7px;
          vertical-align: middle;
          margin-left: 8px;
        }

        .active {
          background-color: #E4393C;
          color: #FFFFFF;
          border: 1px solid #E4393C;
        }
      }

      .productRight {
        padding-right: 20px;

      }
    }

    .mid {
      width: 100%;

      .mid-1 {
        margin-top: 30px;
        background: white;
        padding: 30px;
      }
    }

    .bottom {
      display: inline-flex;
      justify-content: center;
      margin-top: 40px;
      background-color: #F3F3F3;
    }
  }

  .footerImg {
    width: 100%;
    margin-top: 53px;
  }

  .banner {
    width: 100%;
    height: 100%;

    img {
      width: 100%;
      height: 100%;
      object-fit: contain;
    }
  }
</style>
