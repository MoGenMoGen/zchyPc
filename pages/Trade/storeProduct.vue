<template>
<!--店铺商品-->
    <div id="home">
      <div class="header">
        <store-title></store-title>
      </div>
      <div class="content" :style="{width:bWidth+'px'}">
        <div class="top" :style="{width:width+'px'}">
          <div class="productLeft">
            <button @click="changeActive" :class="{active:isActive}">综合<img v-if="this.isActive === 0" src="../../assets/img/onlineTrade/向下-灰.png" alt=""/><img v-if="this.isActive === 1" src="../../assets/img/onlineTrade/向下.png" alt=""/><img v-if="this.isActive ===2" src="../../assets/img/onlineTrade/向上.png" alt=""/></button>
            <button @click="changeActive2" :class="{active:isActive2}">销量<img v-if="this.isActive2 === 0" src="../../assets/img/onlineTrade/向下-灰.png" alt=""/><img v-if="this.isActive2 === 1" src="../../assets/img/onlineTrade/向下.png" alt=""/><img v-if="this.isActive2 ===2" src="../../assets/img/onlineTrade/向上.png" alt=""/></button>
            <button @click="changeActive3" :class="{active:isActive3}">价格<img v-if="this.isActive3 === 0" src="../../assets/img/onlineTrade/向下-灰.png" alt=""/><img v-if="this.isActive3 === 1" src="../../assets/img/onlineTrade/向下.png" alt=""/><img v-if="this.isActive3 ===2" src="../../assets/img/onlineTrade/向上.png" alt=""/></button>
          </div>
          <div class="productRight">
            <el-pagination
              layout="total, prev, pager, next"
              :page-size="54"
              :total="800">
            </el-pagination>
          </div>
        </div>
        <div class="mid" :style="{width:width+'px'}">
          <product-list></product-list>
        </div>
        <div class="bottom" :style="{width:width+'px'}">
          <el-pagination
            @size-change="handleSizeChange"
            @current-change="handleCurrentChange"
            :current-page.sync="currentPage3"
            :page-size="54"
            layout="prev, pager, next, jumper"
            :total="800">
          </el-pagination>
        </div>
      </div>
      <div class="footerImg" :style="{width:bWidth+'px'}">
        <img src="../../assets/img/footer.png" alt=""/>
      </div>
    </div>
</template>

<script>
  import {mapState} from "vuex";
  import StoreTitle from "../../components/onlineTrading/storeTitle";
  import ProductList from "../../components/onlineTrading/productList";
    export default {
        name: "storeProduct",
      data(){
        return{
          isActive:1,
          isActive2:0,
          isActive3:0,
          currentPage3: 5,
        }
      },
      computed:{
        ...mapState([
          'bWidth',
          'width'
        ])
      },
      components: {ProductList, StoreTitle},
      methods:{
        changeActive(){
          if(this.isActive < 2){
            this.isActive += 1;
          }else{
            this.isActive = 1
          }
          this.isActive2 = 0;
          this.isActive3 = 0;
          console.log(this.isActive)
        },
        changeActive2(){
          if(this.isActive2 < 2){
            this.isActive2 += 1;
          }else{
            this.isActive2 = 1
          }
          this.isActive = 0;
          this.isActive3 = 0;
          console.log(this.isActive2)
        },
        changeActive3(){
          if(this.isActive3 < 2){
            this.isActive3 += 1;
          }else{
            this.isActive3= 1
          }
          this.isActive = 0;
          this.isActive2 = 0;
          console.log(this.isActive3)
        },
        handleSizeChange(val) {
          console.log(`每页 ${val} 条`);
        },
        handleCurrentChange(val) {
          console.log(`当前页: ${val}`);
        },
      },
    }
</script>

<style lang="less" scoped>
  @import url("../../assets/css/init.less");
#home{
  background-color: #F3F3F3;

}
  .header{
    margin-bottom: 30px;
  }
  .content{
    background-color: #F3F3F3;
    margin: 0 auto;
    display: flex;
    justify-content: center;
    flex-wrap: wrap;
    .top{
      height: 40px;
      display: flex;
      align-items: center;
      justify-content: space-between;
      background-color: #FFFFFF;
      margin-bottom: 10px;
      .productLeft{
        display: flex;
        align-items: center;
        padding-left: 10px;
        >button{
          background-color: #ffffff;
          color: #666666;
          border: 1px solid #DDDDDD;
          width: 60px;
          height: 26px;
          font-size: 12px;
          cursor: pointer;
        }
        img{
          width: 7px;
          vertical-align: middle;
          margin-left: 8px;
        }
        .active{
          background-color: #E4393C;
          color: #FFFFFF;
          border: 1px solid #E4393C;
        }
      }
      .productRight{
        padding-right: 20px;

      }
    }
    .mid{
      width: 100%;
    }
    .bottom{
      display: inline-flex;
      justify-content: center;
      margin-top: 40px;
      background-color: #F3F3F3;
    }
  }
  .footerImg{
    width: 100%;
    margin-top: 53px;
  }
</style>
