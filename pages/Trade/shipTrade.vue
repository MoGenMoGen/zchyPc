<template>
  <!--船舶交易-->
  <div id="home" :style="{width:bWidth+'px'}">
    <div class="header" >
      <el-breadcrumb separator-class="el-icon-arrow-right" :style="{width:width+'px'}">
        <el-breadcrumb-item :to="{ path: './onlinetrading' }">装备在线</el-breadcrumb-item>
        <el-breadcrumb-item >船舶交易</el-breadcrumb-item>
        <el-breadcrumb-item v-if="catInfo">{{catInfo.nm}}</el-breadcrumb-item>
      </el-breadcrumb>
    </div>
    <!--筛选索引-->
    <div class="index" :style="{width:width+'px'}">
      <!--品牌-->
      <!--<div class="partA">-->
        <!--&lt;!&ndash;左侧标题：品牌&ndash;&gt;-->
        <!--<div class="sectionA">-->
          <!--<p>品牌:</p>-->
        <!--</div>-->
        <!--&lt;!&ndash;右侧列表&ndash;&gt;-->
        <!--<div class="sectionB">-->
          <!--<div class="single" v-for="item in currentBrand" :key="item.id" @click="chooseBrand(item.id)">-->
            <!--<img :src="item.logo"/>-->
          <!--</div>-->
        <!--</div>-->
        <!--&lt;!&ndash;更多&ndash;&gt;-->
        <!--<div class="sectionC" v-show="brandList.length>16 && currentBrand.length<brandList.length">-->
          <!--<p @click="currentBrand=brandList">更多<i class="el-icon-arrow-right"></i></p>-->
        <!--</div>-->
        <!--<div class="sectionC" v-show="brandList.length>16 && currentBrand.length==brandList.length">-->
          <!--<p @click="currentBrand=brandList.slice(0,16)">收起<i class="el-icon-arrow-right"></i></p>-->
        <!--</div>-->
      <!--</div>-->
      <!--船舶航区-->
      <div class="partB">
        <!--左侧标题：船舶航区-->
        <div class="sectionD">
          船舶航区:
        </div>
        <!--右侧列表-->
        <div class="sectionE">
          <div v-for="(item,index) in oneList"
               :key="index" class="singleA"
               @click="changeSelected(0,item.cd)"
               :class="{select:checkId[0] && item.cd==checkId[0]}" >
            <p >{{item.nm}}</p>
          </div>


        </div>
      </div>
      <!--船体材质-->
      <div class="partB">
        <!--左侧标题：船体材质-->
        <div class="sectionD">
          船体材质:
        </div>
        <!--右侧列表-->
        <div class="sectionE">
          <div v-for="(item,index) in twoList" :key="index" class="singleA" @click="changeSelected(1,item.cd)" :class="{select:checkId[1] && item.cd===checkId[1]}">
            <p >{{item.nm}}</p>
          </div>
        </div>
      </div>
      <!--船舶类型-->
      <div class="partB">
        <!--左侧标题：船舶类型-->
        <div class="sectionD">
          船舶类型:
        </div>
        <!--右侧列表-->
        <div class="sectionE">
          <div v-for="(item,index) in threeList" :key="index" class="singleA" @click="changeSelected(2,item.cd)" :class="{select:checkId[2] && item.cd===checkId[2]}">
            <p >{{item.nm}}</p>
          </div>
        </div>
      </div>
      <!--船舶长度-->
      <div class="partB">
        <!--左侧标题：船舶长度-->
        <div class="sectionD">
          公约船长:
        </div>
        <!--右侧列表-->
        <div class="sectionE">
          <div v-for="(item,index) in fourList" :key="index" class="singleA" @click="changeSelected(3,item.cd)" :class="{select:checkId[3] && item.cd===checkId[3]}" >
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
        <div class="productMid">
          <button @click="used=2" :class="{active:used===2}">二手船</button>
          <button @click="used=1" :class="{active:used===1}">新造船</button>
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
        <ship-list :list="list"></ship-list>
      </div>
      <div class="productFooter">
        <el-pagination
          @current-change="handleCurrentChange"
          :current-page.sync="pageNo"
          :page-size="pageSize"
          layout="prev, pager, next, jumper"
          :total="total">
        </el-pagination>
      </div>
    </div>
    <look-and-see></look-and-see>
    <!--产品精选-->
    <!--<div :style="{width:width+'px'}" class="featured">-->
    <!--<div class="featuredHeader">-->
    <!--<p>产品精选</p>-->
    <!--</div>-->
    <!--<div class="featuredMain">-->
    <!--<featured :type='type'></featured>-->
    <!--</div>-->
    <!--</div>-->
    <!--广告栏-->
    <!--<div class="ad" :style="{width:width+'px'}">-->
    <!--<img src="@/assets/img/personal/首页/头像bg.png" alt=""/>-->
    <!--</div>-->
    <!--看了又看-->
    <!--<div :style="{width:width+'px'}" class="featured">-->
    <!--<div class="featuredHeader">-->
    <!--<p>看了又看 </p>-->
    <!--</div>-->
    <!--<div class="featuredMain">-->
    <!--<featured :type='type'></featured>-->
    <!--</div>-->
    <!--</div>-->
    <div class="footerImg" :style="{width:bWidth+'px'}">
      <img src="../../assets/img/footer.png" alt=""/>
    </div>
  </div>
</template>

<script>

  import lookAndSee from "@/components/lookAndSee";
  import {mapState} from "vuex";
  import ProductList from "../../components/onlineTrading/productList";
  import ShipList from "../../components/onlineTrading/shipList";
  import Featured from "../../components/onlineTrading/featured";
  export default {
    name: "shipTrade",
    data(){
      return{
        radio:'',
        type:'ship',
        id:'',
        pageNo:1,
        pageSize:20,
        total:0,
        isActive:1,
        isActive2:0,
        isActive3:0,
        used:null,
        list:[],
        brandId:'',
        brandList:[],
        currentBrand:[],
        checkId:[],
        oneList:[],
        twoList:[],
        threeList:[],
        fourList:[],
        catInfo:null,

        sailingAreaCd:null,
        hullMaterialCd:null,
        typesCd:null,
        lengthCd:null,
        lookAndLook:[],
        proSelect:[],
      }
    },
    components: {
      Featured,
      ShipList,
      ProductList,
      lookAndSee
    },
    computed:{
      ...mapState([
        'bWidth',
        'width'
      ])
    },
    watch:{
      checkId:{
        handler:function () {
          this.pageNo = 1
          this.getList()
        },
        deep:true
      },
      used(){
        this.getList()
      }
    },
    async asyncData(context){
      if(process.client){
        return
      }
      let id = context.query.id ? JSON.parse(context.query.id) : []
      let arr = []
      if(id.length>0){
        arr = await Promise.all([
          // context.app.api.brandList(id[id.length-1]),
          context.app.api.catInfoById(id[id.length-1]),
          context.app.api.shipClassify()
        ])
      }

      let qry = context.app.query.new()
      context.app.query.toP(qry,1,100)
      let data = await context.app.api.shipTradeList(context.app.query.toEncode(qry))
      return{
        arr:arr,
        data:data,
      }
    },
    mounted(){
      // console.log('mounted====================')
      //this.id = this.$route.query.id
      this.id = this.$route.query.id ? JSON.parse(this.$route.query.id) : []
      console.log(this.id)
      this.getData()
      this.getList()
    },
    methods:{
      async getData(){
        //品牌
        if(this.id.length){
          // this.brandList = await this.api.brandList(this.id[this.id.length-1])
          this.catInfo  = await this.api.catInfoById(this.id.length-1?this.id[this.id.length-1]:'')
        }
        // this.currentBrand = this.brandList.slice(0,16)
        let data = await this.api.shipClassify()  //分类
        this.oneList = data.sailingArea
        this.twoList = data.hullMaterial
        this.threeList = data.shipTypes
        // console.log('分类列表')
        // console.log(this.threeList)
        this.fourList = data.shipLength
        // console.log(this.fourList)
        let param = {
          id:this.id[this.id.length-1],
          type:this.id.length
        }
        this.lookAndLook = await this.api.lookAndLook(param)
        this.proSelect = await this.api.proSelect(param)
      },

      handleCurrentChange(val) {
        this.pageNo = val
        this.getList()
      },
      changeSelected(index,cd){

          console.log(this.checkId[index])
          console.log(index)
        //船舶航区
        if (index===0){
            if(this.sailingAreaCd!=cd){
                this.sailingAreaCd = cd;
            }else{
                this.sailingAreaCd = "";
            }

        }
        //船舶航区
        if (index===1){

            if(this.hullMaterialCd!=cd){
                this.hullMaterialCd = cd;
            }else{
                this.hullMaterialCd = "";
            }
        }
        //船舶类型
        if (index===2){
            if(this.typesCd!=cd){
                this.typesCd = cd;
            }else{
                this.typesCd = "";
            }

        }
        //船舶长度:
        if (index===3){
            if(this.lengthCd!=cd){
                this.lengthCd = cd;
            }else{
                this.lengthCd = "";
            }
        }

        this.getList()
        if(this.checkId[index]!=cd){
            this.checkId[index]=cd

        }else{
            this.checkId[index]=""
        }

        // this.$set(this.checkId,index,cd)
      },
      //品牌选择
      chooseBrand(item){
        this.brandId = item.id
        this.getList()
      },
      async getList(){
          console.log("id列表")
          console.log(this.id)
        let qry = this.query.new()
        if(!this.typesCd){
          // if(this.id.length==1){ //只有一个id，是上级id
          //   this.query.toW(qry,'tid',this.id[0],'EQ')
          // }
          // if(this.id.length==2){
          //   this.threeList.forEach(item=>{
          //     if(this.id[1]==item.id){
          //       this.changeSelected(2,item.cd)
          //     }
          //
          //   })
          //   // this.query.toW(qry,'tid',this.id[0],'EQ')
          //   this.query.toW(qry,'sid',this.id[1],'EQ')
          // }
          // if(this.id.length==3){
          //   // this.query.toW(qry,'tid',this.id[0],'EQ')
          //   this.query.toW(qry,'sid',this.id[1],'EQ')
          //   this.query.toW(qry,'catId',this.id[2],'EQ')
          // }
        }else {
          this.query.toW(qry,'typesCd',this.typesCd)
        }

        // this.query.toW(qry,'brandId',this.brandId,'EQ')
        if(this.isActive===1){
          this.query.toO(qry,'seq','desc')
        }
        if(this.isActive===2){
          this.query.toO(qry,'seq','asc')
        }
        if(this.isActive2===1){
          this.query.toO(qry,'salesQty','desc')
        }
        if(this.isActive2===2){
          this.query.toO(qry,'salesQty','asc')
        }
        if(this.isActive3===1){
          this.query.toO(qry,'origPrice','desc')
        }
        if(this.isActive3===2){
          this.query.toO(qry,'origPrice','asc')
        }
        if(this.used){
          this.query.toW(qry,'used',this.used)
        }
        if(this.sailingAreaCd){
          this.query.toW(qry,'sailingAreaCd',this.sailingAreaCd)
        }
        if(this.hullMaterialCd){
          this.query.toW(qry,'hullMaterialCd',this.hullMaterialCd)
        }
        // if(this.typesCd){
        //   this.query.toW(qry,'typesCd',this.typesCd)
        // }
        if(this.lengthCd){
          this.query.toW(qry,'lengthCd',this.lengthCd)
        }
        this.query.toP(qry,this.pageNo,this.pageSize)
        let data = await this.api.shipTradeList(this.query.toEncode(qry))
        this.list = data.data.list
        this.total = data.page.total
      },
      changeActive(){
        if(this.isActive < 2){
          this.isActive += 1;
        }else{
          this.isActive = 1
        }
        this.isActive2 = 0;
        this.isActive3 = 0;
        // console.log(this.isActive)
        this.getList()
      },
      changeActive2(){
        if(this.isActive2 < 2){
          this.isActive2 += 1;
        }else{
          this.isActive2 = 1
        }
        this.isActive = 0;
        this.isActive3 = 0;
        this.getList()
      },
      changeActive3(){
        if(this.isActive3 < 2){
          this.isActive3 += 1;
        }else{
          this.isActive3= 1
        }
        this.isActive = 0;
        this.isActive2 = 0;
        this.getList()
      },
    },
    // async asyncData(context){
    //   if(process.client){
    //     return
    //   }
    //   let id = context.query.id ? JSON.parse(context.query.id) : []
    //
    //   let arr = []
    //   arr = await Promise.all([
    //     context.app.api.brandList(id[id.length-1]),
    //     context.app.api.catInfoById(id[id.length-1]),
    //     context.app.api.shipClassify(),
    //   ])
    //
    //   arr.push(await context.app.api.proDetailLook(id))
    //   arr.push(await context.app.api.shopIntro(info.shopId))
    //   return {
    //     info:info,
    //     arr:arr,
    //   }
    // },
  }
</script>

<style lang="less" scoped>
  @import url("../../assets/css/init.less");
  #home{
    background-color: #FFFFFF;
  }
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
    margin: 0 auto;
    padding-top: 20px;
    .partA{
      width: 100%;
      border-bottom: 1px solid #DDDDDD;
      border-top: 1px solid #DDDDDD;
      display: flex;
      justify-content: space-between;
      .sectionA{
        width: 10.4%;
        background-color: #F3F3F3;
        padding: 25px 0 25px 22px;
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
        width: 5.2%;
        >p{
          margin-top: 11px;
          border: 1px solid #DDDDDD;
          padding: 7px;
        }
      }
    }
    .partB{
      width: 100%;
      border-bottom: 1px solid #DDDDDD;
      display: flex;
      .sectionD{
        width: 10.4%;
        background-color: #F3F3F3;
        display: flex;
        display: -webkit-flex;
        align-items: center;
        font-weight: bold;
        font-size: 16px;
        padding: 25px 0 25px 22px;
      }
      .sectionE{
        flex: 1;
        padding: 25px 0;
        .singleA{

          text-align: center;
          cursor: pointer;
          display: inline-block;
          margin: 10px 0;
          margin-left: 30px;
          p:hover{
            color: #FF3E3E;
          }
        }
      }
      .select{
        color: #FF3E3E;
        border-bottom: 1px solid #FF3E3E !important;
      }
    }
  }
  .productList{
    background-color: #FFFFFF;
    margin: 20px auto 0 auto;
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
      .productMid{
        display: flex;
        flex: 1;
        margin-left: 60px;
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
      .productRight{
        padding-right: 20px;

      }
    }
    .productContent{
      background-color: #FFFFFF;
    }
    .productFooter{
      width: 100%;
      padding-top: 50px;
      display: inline-flex;
      justify-content: center;
      padding-bottom: 50px;
      border-bottom: 1px solid #F3F3F3;
      border-left: 1px solid #F3F3F3;
      border-right: 1px solid #F3F3F3;
    }
  }
  .featured{
    background-color: #FFFFFF;
    margin: 30px auto 0 auto;
    height: 394px;
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
    margin: 25px auto 0 auto ;
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
