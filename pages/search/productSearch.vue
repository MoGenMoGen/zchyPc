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
      <div class="right">
        <el-pagination
          background
          v-if="this.productList != ''"
          @current-change="handleCurrentChange"
          :current-page.sync="search.pageNo"
          :page-size="search.size"
          layout="prev, pager, next, jumper"
          :total="total">
        </el-pagination>
      </div>
    </div>
    <div class="body">
      <div class="mainView">
        <div class="productQ" v-for="(item,index) in productList" :key="index" :class="{seen:item.isSeen}" @click="toProDetail(item)" @mouseenter="enter(index)" @mouseleave="leave(index)">
          <div class="img" :style="{height:width*0.196*0.827+'px'}">
            <img :src="item.fmimgurl" alt=" "/>
          </div>
          <div class="overall">
            <p class="child1" v-html="item.nm"></p>
            <div class="space">
              <p class="child2" v-if="item.origprice!=price">
                ￥<span class="child3" v-show="item.origprice">{{item.origprice}}</span>
                <span class="child4" v-show="item.price2">{{item.price2}}</span>
              </p>
              <p class="child2" v-else>
                价格面议
                <span class="child3" ></span>
              </p>
<!--              <button>满减</button>-->
            </div>
          </div>
        </div>
      </div>
    </div>
    <div class="foot">
      <el-pagination
        v-if="this.productList != ''"
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
  import lookAndSee from "@/components/lookAndSee";
  export default {
        name: "productSearch",
        components:{lookAndSee},
        data(){
            return{
              isActive:1,
              isActive2:0,
              isActive3:0,
              search:{
                pageNo:1,
                size:15,
                ship:0,
                keywords:'',
                overall:'DESC',//综合排序
                salesqty:'',//销量排序
                origprice:'',//价格排序
              },
              total:0,
              productList:[],
              time:0,
            }
        },
      computed: {
        ...mapState([
          'bWidth',
          'width',
          'currentRole',
          'typeSize',
          'price'
        ])
      },
    watch:{
      $route(e){
        let arr = this.until.getQueryString('keyWord')
        this.search.keywords = arr
        console.log('改变')
        setTimeout(()=>{
          this.time=arr+this.until.getQueryString('time')
        },200)

        // this.getData()
      },
      time(){
		console.log(this.time)
        this.getData()
      },
      // 'search.keywords'(){
      //   this.getData()
      // },
    },
    mounted() {
      let arr = this.until.getQueryString('keyWord')
      this.search.keywords = arr
      console.log('ddd')
      this.time=arr+this.until.getQueryString('time')
      // this.getData()
    },
    methods:{
          //获取数据，若测试search.keywords为空则跳至全部产品
      async getData(){
        if(this.search.keywords){
          let data = await this.api.goodsSearch(this.search)
          // console.log(data)
          this.total=data.data.total

          this.productList = data.data.result
        }else {
          this.$router.push('../Trade/productTrading')
        }
      },
      //鼠标移入
      enter(index){
        this.productList[index].isSeen = true
        this.$set(this.productList,index,this.productList[index])
      },
      //鼠标移出
      leave(index){
        this.productList[index].isSeen = false
        this.$set(this.productList,index,this.productList[index])
      },
      //去产品详情，判断是船舶或者产品
      toProDetail(item){
        this.$router.push({
          path:'../Trade/productDetail',
          query:{
            id:item.id,
            type:item.ship?'ship':'product'
          }
        })
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
      height: 40px;
      >button{
        width: 60px;
        height: 26px;
        background-color: #FFFFFF;
        color: #666666;
        font-size: 12px;
        cursor: pointer;
        border: 1px solid #DDDDDD;
        float: left;
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
      .right{
        float: right;
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
        margin-top: 10px;
        width: 100%;
        .seen{
          border: 1px solid #ECECEC;
          box-shadow: 0 0 3px 3px rgba(0,0,0,0.08);
        }
        //商品具体的div
        .productQ{
          float: left;
          display: flex;
          flex-direction: column;
          /*justify-content: space-around;*/
          width: 19.6%;
          background-color: #ffffff;
          height: 343px;
          margin-bottom: 10px;
          cursor: pointer;
          margin-right:0.5% ;
          &:nth-of-type(5n+5){
            margin-right: 0;
          }
          .img{
            width: 82.7%;
            margin: 20px auto;
            display: flex;
            display: -webkit-flex;
            >img{
              margin: auto;
            }
          }

          .overall{
            display: flex;
            flex-wrap: wrap;
            justify-content: flex-start;
            text-align: left;
            width: 190px;
            margin: 0 auto;
            >p{
              padding-bottom: 20px;
              color: #666666;
            }
            .space{
              display: inline-flex;
              justify-content: space-between;
              width: 100%;
              //￥符号
              .child2{
                color: #FF3E3E;
              }
              //小数点前数字
              .child3{
                color: #FF3E3E;
                font-size: 24px;
              }
              //小数点后
              .child4{
                color: #FF3E3E;
                font-size: 18px;
              }
              button{
                width: 42px;
                height: 20px;
                border: 1px solid #FF3E3E;
                color: #FF3E3E;
                background-color: #FFFFFF;
                border-radius: 2px;
                font-size: 14px;
              }
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
