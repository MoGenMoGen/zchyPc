<template>
<!--产品交易-->
<div id="home" :style="{width:bWidth+'px'}">
  <!--页头-->
  <div class="header" >
      <el-breadcrumb separator-class="el-icon-arrow-right" :style="{width:width+'px'}">
        <el-breadcrumb-item :to="{ path: './onlinetrading' }">装备在线</el-breadcrumb-item>
        <el-breadcrumb-item ><p class="textRed">产品交易</p></el-breadcrumb-item>
      </el-breadcrumb>
  </div>
  <!--筛选索引-->
  <div class="index" :style="{width:width+'px'}">
      <!--品牌-->
      <div class="partA"  v-show="brandList.length>0">
        <!--左侧标题：品牌-->
        <div class="sectionA">
          <p>品牌:</p>
        </div>
        <!--右侧列表-->
        <div class="sectionB">
          <div class="single" v-for="item in currentBrand" :key="item.id" @click="chooseBrand(item.id)">
            <img :src="item.logo"/>
          </div>
        </div>
        <!--更多-->
        <div class="sectionC" v-show="brandList.length>16 && currentBrand.length<brandList.length">
          <p @click="currentBrand=brandList">更多<i class="el-icon-arrow-right"></i></p>
        </div>
        <div class="sectionC" v-show="brandList.length>16 && currentBrand.length==brandList.length">
          <p @click="currentBrand=brandList.slice(0,16)">收起<i class="el-icon-arrow-right"></i></p>
        </div>
      </div>
      <!--一级-->
      <div class="partB">
        <!--左侧标题：一级-->
        <div class="sectionD">
          <p>一级:</p>
        </div>
        <!--右侧列表-->
        <div class="sectionE">
          <div v-for="(item,index) in classifyList" :key="index" class="singleA" @click="chooseClassify(0,item)" :class="{select:item.id==checked[0]}" >
            <p >{{item.nm}}</p>
          </div>
        </div>
      </div>
      <!--二级-->
      <div class="partB" v-if="twoList && twoList.length>0">
        <!--左侧标题：二级-->
        <div class="sectionD">
          <p>二级:</p>
        </div>
        <!--右侧列表-->
        <div class="sectionE">
          <div v-for="(item,index) in twoList" :key="index" class="singleA" @click="chooseClassify(1,item)" :class="{select:item.id ==checked[1]}">
            <p >{{item.nm}}</p>
          </div>
        </div>
      </div>
      <!--三级-->
      <div class="partB" v-if="threeList && threeList.length>0">
        <!--左侧标题：三级-->
        <div class="sectionD">
          <p>三级:</p>
        </div>
        <!--右侧列表-->
        <div class="sectionE">
          <div v-for="(item,index) in threeList" :key="index" class="singleA" @click="chooseClassify(2,item)" :class="{select:item.id == checked[2]}" >
            <p >{{item.nm}}</p>
          </div>
        </div>
      </div>
  </div>
  <!--商品列表-->
  <div class="productList" :style="{width:width+'px'}" >
      <div class="productHeader">
        <div class="productLeft">
          <button @click="changeActive" :class="{active:isActive}">综合<img v-if="this.isActive === 0" src="../../assets/img/onlineTrade/向下-灰.png" alt=""/><img v-if="this.isActive === 1" src="../../assets/img/onlineTrade/向下.png" alt=""/><img v-if="this.isActive ===2" src="../../assets/img/onlineTrade/向上.png" alt=""/></button>
          <button @click="changeActive2" :class="{active:isActive2}">销量<img v-if="this.isActive2 === 0" src="../../assets/img/onlineTrade/向下-灰.png" alt=""/><img v-if="this.isActive2 === 1" src="../../assets/img/onlineTrade/向下.png" alt=""/><img v-if="this.isActive2 ===2" src="../../assets/img/onlineTrade/向上.png" alt=""/></button>
          <button @click="changeActive3" :class="{active:isActive3}">价格<img v-if="this.isActive3 === 0" src="../../assets/img/onlineTrade/向下-灰.png" alt=""/><img v-if="this.isActive3 === 1" src="../../assets/img/onlineTrade/向下.png" alt=""/><img v-if="this.isActive3 ===2" src="../../assets/img/onlineTrade/向上.png" alt=""/></button>
        </div>
        <div class="productRight">
          <el-pagination
            layout="total, prev, pager, next"
            @current-change="handleCurrentChange"
            :current-page.sync="pageNo"
            :page-size="pageSize"
            :total="total">
          </el-pagination>
        </div>
      </div>
      <div class="productContent">
        <product-list :list = 'list'></product-list>
      </div>
      <div class="productFooter">
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
 <look-and-see></look-and-see>
  <div class="footerImg">
    <img src="../../assets/img/footer.png" alt=""/>
  </div>
</div>
</template>

<script>
  import {mapState} from "vuex";
  import ProductList from "../../components/onlineTrading/productList";
  import lookAndSee from "../../components/lookAndSee";

    export default {
      name: "productTrading",
      components: {lookAndSee, ProductList},
      data(){
        return{
          type:'product',
          classifyList:[],
          isActive:1,
          isActive2:0,
          isActive3:0,
          currentPage3: 1,
          list:[],
          twoList:[],
          threeList:[],
          brandList:[],//所有品牌列表
          currentBrand:[],//显示的品牌列表
          checked:[], //选中的分类Id
          brandId:'',//选中的品牌id
          pageNo:1,
          pageSize:15,
          total:0,
        }
      },
      computed:{
        ...mapState([
          'bWidth',
          'width'
        ])
      },
      watch:{
        checked:{
          handler:function () {
            if(this.checked.length>0){
              this.getList()
              this.getBrandList()
              this.changeClassify()
            }
          },
          deep:true,
          immediate:true
        },
        $route:{
          handler:function () {
            this.checked = this.$route.query.id ? JSON.parse(this.$route.query.id) : []
          },
          immediate:true
        }
      },
      async asyncData(context){
        if(process.client){
          return
        }

        let checked = context.query.id ? JSON.parse(context.query.id) : []

        let classifyList = await context.app.api.goodClassify()
        if(checked.length==0){
          checked.push(classifyList[0].id)
        }

        let qry = context.app.query.new()
        if(checked.length==1){
          context.app.query.toW(qry,'tid',checked[0],'EQ')
        }
        if(checked.length==2){
          context.app.query.toW(qry,'sid',checked[1],'EQ')
        }
        if(checked.length==3){
          context.app.query.toW(qry,'catId',checked[2],'EQ')
        }
        context.app.query.toP(qry,1,100)
        let data = await context.app.api.goodList(context.app.query.toEncode(qry))
        return{
          classifyList:classifyList,
          data:data,
        }
      },
      mounted(){
        this.checked = this.$route.query.id ? JSON.parse(this.$route.query.id) : []
        this.getClassify()
      },
      methods:{
        //获取列表
        async getList(){
          let qry = this.query.new()
          if(this.checked.length==1){
            this.query.toW(qry,'tid',this.checked[0],'EQ')
          }
          if(this.checked.length==2){
            this.query.toW(qry,'sid',this.checked[1],'EQ')
          }
          if(this.checked.length==3){
            this.query.toW(qry,'catId',this.checked[2],'EQ')
          }
          if(this.isActive==1){
            this.query.toO(qry,'seq','desc')
          }
          if(this.isActive==2){
            this.query.toO(qry,'seq','asc')
          }
          if(this.isActive2==1){
            this.query.toO(qry,'salesQty','desc')
          }
          if(this.isActive2==2){
            this.query.toO(qry,'salesQty','asc')
          }
          if(this.isActive3==1){
            this.query.toO(qry,'origPrice','desc')
          }
          if(this.isActive3==2){
            this.query.toO(qry,'origPrice','asc')
          }
          this.query.toW(qry,'brandId',this.brandId,'EQ')
          this.query.toP(qry,this.pageNo,this.pageSize)
          let data = await this.api.goodList(this.query.toEncode(qry))
          this.total = data.page.total
          this.list = data.data.list

        },
        handleCurrentChange(val){
          this.pageNo = val
          this.getList()
        },
        //获取全部分类
        async getClassify(){
          this.classifyList = await this.api.goodClassify()
          if(this.checked.length>0){
            this.changeClassify()
          }else {
            this.checked.push(this.classifyList[0].id)
          }
        },
        //品牌列表
        async getBrandList(){
          this.brandList = await this.api.brandList(this.checked[this.checked.length-1])
          this.currentBrand = this.brandList.slice(0,16)
        },
        //品牌选择
        chooseBrand(item){
          this.brandId = item.id
          this.getList()
        },
        //分类选择
        chooseClassify(type,item){
          this.checked[type] = item.id
          this.$set(this.checked,type,item.id)
          this.checked = this.checked.slice(0,type+1)
        },
        //分类切换
        changeClassify(){
          let two = this.classifyList.find(item=> item.id==this.checked[0])
          if(two){
            this.twoList = two.children
          }else {
            this.twoList = []
          }

          if(this.twoList && this.twoList.length>0 && this.checked.length>1){
            this.threeList = this.twoList.find(item=>item.id==this.checked[1]).children
          }else {
            this.threeList = []
          }
        },

        changeActive(){
          if(this.isActive < 2){
            this.isActive += 1;
          }else{
            this.isActive = 1;
          }
          this.isActive2 = 0;
          this.isActive3 = 0;
          this.getList()
        },
        changeActive2(){
          if(this.isActive2 < 2){
            this.isActive2 += 1;
          }else{
            this.isActive2 = 1;
          }
          this.isActive = 0;
          this.isActive3 = 0;
          this.getList()
          // console.log(this.isActive2)
        },
        changeActive3(){
          if(this.isActive3 < 2){
            this.isActive3 += 1;
          }else{
            this.isActive3= 1;
          }
          this.isActive = 0;
          this.isActive2 = 0;
          // console.log(this.isActive3)
          this.getList()
        },


      },

    }
</script>
<style lang="less">

</style>

<style lang="less" scoped>
  @import url("../../assets/css/init.less");
#home{
  background-color: #FFFFFF;
}
  //首部标题部分
.header{
  background-color: #F3F3F3;
  height: 40px;
  display: flex;
  justify-content: center;
  align-items: center;
    .textRed{
      color: #FF3E3E;
    }
}
//索引部分
.index{
  background-color: #FFFFFF;
  padding-top: 20px;
  margin: 0 auto;
    >div:last-of-type{
      border-bottom: 1px solid #DDDDDD;
    }
    .partA{
      height: 125px;
      width: 100%;
      border-top: 1px solid #DDDDDD;
      display: flex;
      justify-content: space-between;
      .sectionA{
        width: 10.4%;
        background-color: #F3F3F3;
        >p{
          font-weight: bold;
          font-size: 16px;
          padding-left: 22px;
          padding-top: 21px;
        }
      }
      .sectionB{
        flex: 1;
        padding: 25px 0 25px 30px;
        overflow: hidden;
        .single{
          width: 12%;
          height: 50px;
          border-right:1px solid #DDDDDD ;
          border-bottom: 1px solid #DDDDDD;
          cursor: pointer;
          &:nth-of-type(8n+1){
            border-left: 1px solid #DDDDDD;
          }
          &:nth-of-type(-n+8){
            border-top: 1px solid #DDDDDD;
          }
          display: flex;
          display: -webkit-flex;
          float: left;
          img{
            margin: auto;
          }
        }
      }
      .sectionC{

        >p{
          margin-top: 11px;
          border: 1px solid #DDDDDD;
          padding: 7px;
        }
      }
    }
    .partB{
      border-top: 1px solid #DDDDDD;
      width: 100%;
      display: flex;

      .sectionD{
        width: 10.4%;
        background-color: #F3F3F3;
        >p{
          font-weight: bold;
          font-size: 16px;
          padding-left: 22px;
          padding-top: 30px;
        }
      }
      .sectionE{
        flex: 1;
        padding: 20px 0;
        .singleA{
          display: inline-block;
          padding-left: 30px;
          text-align: left;
          cursor: pointer;
          line-height: 40px;
          &:hover{
            color: #FF3E3E;
          }
        }
      }
      .select{
        color: #FF3E3E;
      }
    }
}
.productList{
  background-color: #FFFFFF;
  margin: 20px auto 0;
    .productHeader{
      background-color: #F3F3F3;
      border-top: 1px solid #DDDDDD;
      border-bottom: 1px solid #DDDDDD;
      width: 100%;
      height: 40px;
      display: flex;
      align-items: center;
      justify-content: space-between;
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
    .productContent{
      background-color: #FFFFFF;
    }
    .productFooter{
      width: 100%;
      margin-top: 52px;
      display: inline-flex;
      justify-content: center;
      margin-bottom: 76px;
    }
}
.featured{
  background-color: #FFFFFF;
  margin: 0 auto;
  /*height: 394px;*/
  border: 1px solid #DEDEDE;
    .featuredHeader{
      >p{
        color: #343434;
        font-size: 20px;
        padding-left: 20px;
        padding-top: 20px;
        line-height: 19px;
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
.footerImg{
  width: 100%;
  margin-top: 53px;
}
</style>
