<template>
  <div id="home" :style="{width:bWidth + 'px '}">
    <div class="header main" >
      <el-breadcrumb class="main" separator-class="el-icon-arrow-right" :style="{width:width + 'px'}">
        <el-breadcrumb-item :to="{ }">全部结果</el-breadcrumb-item>
        <el-breadcrumb-item ><p class="textRed">{{search.keywords}}</p></el-breadcrumb-item>
      </el-breadcrumb>
    </div>
    <div class="content main" :style="{width:width + 'px'}">
      <div class="nav">
        <button @click="changeActive" :class="{active:isActive}">综合<img v-if="this.isActive === 0" src="../../assets/img/onlineTrade/向下-灰.png" alt=""/><img v-if="this.isActive === 1" src="../../assets/img/onlineTrade/向下.png" alt=""/><img v-if="this.isActive ===2" src="../../assets/img/onlineTrade/向上.png" alt=""/></button>
        <button @click="changeActive2" :class="{active:isActive2}">销量<img v-if="this.isActive2 === 0" src="../../assets/img/onlineTrade/向下-灰.png" alt=""/><img v-if="this.isActive2 === 1" src="../../assets/img/onlineTrade/向下.png" alt=""/><img v-if="this.isActive2 ===2" src="../../assets/img/onlineTrade/向上.png" alt=""/></button>
        <button @click="changeActive3" :class="{active:isActive3}">价格<img v-if="this.isActive3 === 0" src="../../assets/img/onlineTrade/向下-灰.png" alt=""/><img v-if="this.isActive3 === 1" src="../../assets/img/onlineTrade/向下.png" alt=""/><img v-if="this.isActive3 ===2" src="../../assets/img/onlineTrade/向上.png" alt=""/></button>
        <div class="mid" >
          <button @click="changeActive4" :class="{active:isActive4}">二手船</button>
          <button @click="changeActive5" :class="{active:isActive5}">新造船</button>
        </div>
        <div class="right">
          <el-pagination
            background
            v-if="this.shipList != ''"
            @current-change="handleCurrentChange"
            :current-page.sync="search.pageNo"
            :page-size="search.search"
            layout="prev, pager, next, jumper"
            :total="total">
          </el-pagination>
        </div>
      </div>
      <div class="body">
        <div class="mainView">
          <div class="ship" v-for="(item,index) in shipList" :key="index" @click="toProDetail(item)">
            <!--左一图片-->
            <div class="left1">
              <img :src="item.img" alt=""/>
            </div>
            <!--左二名称/属性-->
            <div class="left2">
              <p v-html="item.nm"></p>
              <img v-if="item.used == 1" :src="tp1" alt=""/><img v-else :src="tp2" alt=""/>
            </div>
            <!--中部船舶属性-->
            <div class="mid">
              <p><span>船舶总长：</span>{{item.totalLen}}m</p>
              <p><span>公约船长：</span>{{item.pactLen}}m</p>
              <p><span>型宽：</span>{{item.width}}m</p>
              <p><span>型深：</span>{{item.deep}}m</p>
            </div>
            <!--右一价格-->
            <div class="right1">
              <p><span class="textRed">￥</span><span class="textRed">{{item.origPrice}}</span>/艘</p>
            </div>
            <!--右二立即下单-->
            <div class="right2">
              <button @click.stop="toProDetail(item)">立即下单</button>
            </div>
          </div>
        </div>
      </div>
      <div class="foot">
        <el-pagination
          v-if="this.shipList != ''"
          background
          @current-change="handleCurrentChange"
          :current-page.sync="search.pageNo"
          :page-size="search.size"
          layout="prev, pager, next, jumper"
          :total="total">
        </el-pagination>
      </div>
    </div>
    <div class="footer main" :style="{width:width+'px'}">
      <look-and-see></look-and-see>
    </div>
    <div class="footerImg">
      <img src="../../assets/img/footer.png" alt=""/>
    </div>
  </div>
</template>

<script>
  import {mapState} from "vuex";
  import img1 from "../../assets/img/homeSZ1.png";
  import tp1 from '@/assets/img/onlineTrade/新造船.png'
  import tp2 from '@/assets/img/onlineTrade/二手船.png'
  import lookAndSee from "@/components/lookAndSee";
  export default {
        name: "shipSearch",
    components:{lookAndSee},
    data(){
      return{
        type:'product',
        tp1,
        tp2,
        isActive:1,
        isActive2:0,
        isActive3:0,
        isActive4: false,
        isActive5: false,
        search:{
          pageNo:1,
          size:6,
          ship:1,
          keywords:'',
          overall:'DESC',//综合排序
          salesqty:'',//销量排序
          origprice:'',//价格排序
          used: ''
        },
        total:0,
        shipList:[],
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
        let arr = this.until.getQueryString('keyWord')
        this.search.keywords = arr
        setTimeout(()=>{
          this.time=arr+this.until.getQueryString('time')
        },200)
        // this.getData()
      },
      time(){
        this.getData()
      },
    },
    mounted() {
      let arr = this.until.getQueryString('keyWord')
      this.search.keywords = arr
      this.time=arr+this.until.getQueryString('time')
      // this.getData()
    },
    methods:{
      async getData(){
        if(this.search.keywords){
          let qry = this.query.new()
          this.query.toP(qry,this.search.pageNo,this.search.size)
          this.query.toW(qry,'nm',this.search.keywords,'LK')
          if(this.search.overal) {
            this.query.toO(qry,'seq',this.search.overall)
          }
          if(this.search.salesqty) {
            this.query.toO(qry,'salesQty',this.search.salesqty)
          }
          if(this.search.origprice) {
            this.query.toO(qry,'origPrice',this.search.origprice)
          }
          if(this.search.used) {
            this.query.toW(qry,'used',this.search.used,'EQ')
          }
          let data = await this.api.shipTradeList(this.query.toEncode(qry))
          data.data.list.forEach(item => {
            item.nm = item.nm.replace(new RegExp(this.search.keywords,'g'),`<span style='color:#dd4b39'>${this.search.keywords}</span>`)
          })
          this.shipList = data.data.list
          this.total = data.page.total
        }else{
          this.$router.push('../Trade/shipTrade')
        }
      },
      toProDetail(item){
        this.$router.push({
          path:'../Trade/productDetail',
          query:{
            id:item.goodsId,
            type:item.ship?'ship':'product'
          }
        })
      },
      enter(index){
        this.productList[index].isSeen = true
        this.$set(this.productList,index,this.productList[index])
      },
      leave(index){
        this.productList[index].isSeen = false
        this.$set(this.productList,index,this.productList[index])
      },
      changeActive(){
        if(this.isActive < 2){
          this.isActive += 1;
        }else{
          this.isActive = 1;
        }
        if(this.isActive == 0){
          this.search.overall = ''
        }else if(this.isActive == 1){
          this.search.overall = 'DESC'
        }else if(this.isActive == 2){
          this.search.overall = 'asc'
        }
        this.search.origprice = ''
        this.search.salesqty = ''
        this.isActive2 = 0;
        this.isActive3 = 0;
        this.getData()
      },
      changeActive2(){
        if(this.isActive2 < 2){
          this.isActive2 += 1;
        }else{
          this.isActive2 = 1;
        }
        if(this.isActive2 == 0){
          this.search.salesqty = ''
        }else if(this.isActive2 == 1){
          this.search.salesqty = 'DESC'
        }else if(this.isActive2 == 2){
          this.search.salesqty = 'asc'
        }
        this.search.origprice = ''
        this.search.overall = ''
        this.isActive = 0;
        this.isActive3 = 0;
        this.getData()
      },
      changeActive3(){
        if(this.isActive3 < 2){
          this.isActive3 += 1;
        }else{
          this.isActive3= 1;
        }
        if(this.isActive3 == 0){
          this.search.origprice = ''
        }else if(this.isActive3 == 1){
          this.search.origprice = 'DESC'
        }else if(this.isActive3 == 2){
          this.search.origprice = 'asc'
        }
        console.log('1',this.isActive3,'2',this.search.origprice)
        this.isActive = 0;
        this.isActive2 = 0;
        this.search.salesqty = ''
        this.search.overall = ''
        this.getData()
      },
      changeActive4(){
        this.isActive4 = true
        this.isActive5 = false
        this.search.used = 2
        this.getData()

      },
      changeActive5(){
        this.isActive5 = true
        this.isActive4 = false
        this.search.used = 1
        this.getData()
      },
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
    background-color: #F3F3F3;
    .textRed{
      color: #E4393C;
      font-weight: bold;
    }
  }
  .content{
    padding-top: 30px;
    color: #F3F3F3;
    width: 100%;
    .nav{
      border-top: 1px solid #DDDDDD ;
      border-bottom: 1px solid #DDDDDD;
      padding: 7px 9px;
      display: flex;
      flex-direction: row;
      width: 100%;
      align-items: center;
      white-space: nowrap;
      >button{
        width: 80px;
        height: 26px;
        background-color: #FFFFFF;
        color: #666666;
        font-size: 12px;
        cursor: pointer;
        border: 1px solid #DDDDDD;

        flex-shrink: 0;
        img{
          width: 7px;
          vertical-align: middle;
          margin-left: 8px;
        }
      }
      .active{
        background-color: #E4393C;
        color: #FFFFFF;
        border: none;
      }
      .mid{
        display: flex;
        margin-left: 5%;
        >button{
          background-color: #ffffff;
          color: #666666;
          border: 1px solid #DDDDDD;
          width: 60px;
          height: 26px;
          font-size: 12px;
          cursor: pointer;
        }
        .active{
          background-color: #E4393C;
          color: #FFFFFF;
          border: 1px solid #E4393C;
        }
      }
      .right{
        /*float: right;*/
        margin-left: 48%;
        flex: 1;
        height: 100%;
        display: flex;
        align-items: center;
      }
    }
    .body{
      width: 100%;
      flex: 1;
      display: flex;
      .mainView{
        display: flex;
        margin-top: 10px;
        width: 100%;
        flex-wrap: wrap;
        border: 1px solid #F3F3F3;
        .ship{
          height: 211px;
          width: 100%;
          border-bottom: 1px solid #F3F3F3;
          padding: 20px 30px;
          display: inline-flex;
          .left1{
            width: 24.1%;
            >img{
              max-width: 100%;
              margin: auto;
            }
          }
          .left2{
            display: flex;
            flex-wrap: wrap;
            /*justify-content: center;*/
            flex-direction: column;
            justify-content: center;
            width: 25%;
              margin-left: 1.5%;
            p:nth-child(1){
              // line-height: 60px;
              font-size: 16px;
              color: #333333;
              margin-bottom: 10px;
            }
            >img{
              width: 80px;
            }
          }
          .mid{
            width: 18%;
            display: flex;
            flex-wrap: wrap;
            justify-content: center;
            flex-direction: column;
            >p{
              height: 27px;
              color: #999999;
              display: inline-flex;
              justify-content: space-between;
              width: 135px;
            }
          }
          .right1{
            width: 18%;
            display: flex;
            align-items: center;
            .textRed{
              color: #FF3E3E;
            }
            >p{
              color: #999999;
              font-size: 14px;
              line-height: 21px;
              >span:nth-child(1){
                font-size: 8px;
              }
            }
          }
          .right2{
            width: 11.8%;
            display: flex;
            justify-content: center;
            align-items: center;
            >button{
              background-color: #2778BE;
              color: #FFFFFF;
              font-size: 16px;
              border-radius: 5px;
              width: 120px;
              height: 40px;
              border: 0;
              cursor: pointer;
            }
          }
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
  }
  .footer{
    .featured{
      background-color: #FFFFFF;
      margin: 0 auto;
      height: 394px;
      border: 1px solid #DEDEDE;
      .featuredHeader{
        display: flex;
        flex: 1;
        flex-direction: row;
        justify-content: space-between;
        >p{
          color: #343434;
          font-size: 20px;
          padding-left: 20px;
          padding-top: 20px;
          line-height: 19px;
        }
        p:nth-child(2){
          float: right;
          color: #333333;
          font-size: 14px;
          line-height: 19px;
          margin-right: 22px;
          >i{
            color: #AAAAAA;
            margin-right: 9px;
          }
        }
      }
      .featuredMain{

      }
    }
    .ad{
      margin: 20px auto 21px auto ;
      background-color: #FFFFFF;
      height: 102px;
      img{
        width: 100%;
        height: 100%;
      }
    }
  }
  .footerImg{
    margin-top: 40px;
  }
</style>
