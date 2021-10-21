<template>
<div id="home" :style="{width:bWidth + 'px'}">
  <div class="header">
    <el-breadcrumb class="main" separator-class="el-icon-arrow-right" :style="{width:width + 'px'}">
      <el-breadcrumb-item :to="{ }">全部结果</el-breadcrumb-item>
      <el-breadcrumb-item ><p class="textRed">{{search.keywords}}</p></el-breadcrumb-item>
    </el-breadcrumb>
  </div>
  <div class="content main" :style="{width:width + 'px'}">
    <div class="left">
      <div class="nav">
        <button @click="changeActive" :class="{active:isActive}">综合<img v-if="this.isActive === 0" src="../../assets/img/onlineTrade/向下-灰.png" alt=""/><img v-if="this.isActive === 1" src="../../assets/img/onlineTrade/向下.png" alt=""/><img v-if="this.isActive ===2" src="../../assets/img/onlineTrade/向上.png" alt=""/></button>
        <button @click="changeActive2" :class="{active:isActive2}">销量<img v-if="this.isActive2 === 0" src="../../assets/img/onlineTrade/向下-灰.png" alt=""/><img v-if="this.isActive2 === 1" src="../../assets/img/onlineTrade/向下.png" alt=""/><img v-if="this.isActive2 ===2" src="../../assets/img/onlineTrade/向上.png" alt=""/></button>
      </div>
      <div class="body" >
        <div class="part" v-for="(item,index) in list" :key="index">
          <div class="lf" @click="storeDetail(item)">
            <img :src="item.shopImg" :style="{height:width*0.83*0.54*0.205 + 'px'}"/>
            <div class="ct">
              <p v-html="item.nm"></p>
              <p>所在地区：<span v-html="item.area"></span></p>
              <p>主营：<span v-html="item.mainGoods"></span></p>
            </div>
          </div>
          <div class="rt">
            <div class="pt" v-for="(v,t) in item.goodsVos" :key="t" @click="toProDetail(v)">
              <div class="img">
                <span>
                    <img :src="v.fmImgUrl"/>
                </span>
              </div>
              <p v-if="v.origPrice!=price">￥{{v.origPrice}}</p>
              <p v-else>价格面议</p>
            </div>
          </div>
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
    </div>
    <div class="right">
      <div class="nav">
        <p>掌柜热卖</p>
      </div>
      <div class="body">
        <div class="part" v-for="(i,o) in list2" :key="o" @click="toProDetail(i)">
          <img :src="i.fmImgUrl"/>
          <p>{{i.nm}}</p>
        </div>
      </div>
    </div>
  </div>
  <div class="footerImg">
    <img src="../../assets/img/footer.png" alt=""/>
  </div>
</div>
</template>

<script>
  import {mapState} from "vuex";
    export default {
        name: "storeSearch",
      data(){
          return{
            isActive:1,
            isActive2:0,
            search:{
              keywords:'',
              pageNo:1,
              size:6,
              overall:'DESC',
              salesqty:'',
            },
            total:0,
            list:[],
            list2:[],
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
        // console.log('keyWord',this.keyWord)
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
              let data = await this.api.shopList(this.query.toEncode(qry))
              console.log(data)
              data.data.list.forEach(item => {
                item.nm = item.nm.replace(new RegExp(this.search.keywords,'g'),`<span style='color:#dd4b39'>${this.search.keywords}</span>`)
              })
              this.list = data.data.list
              this.total=data.page.total
              this.list.forEach(item => {
                item.synopsis =item.synopsis ?  item.synopsis.replace(/<\/?[^>]*>/g, "").slice(0,15) + '...' : ''
                item.synopsis = item.synopsis ? item.synopsis.replace(/(&nbsp;)/g, "") : '';
              })
              this.list2 = await this.api.personLook()
            }else if(!this.search.keywords){
              this.$router.push('../Trade/qualityStore')
            }
          },
        toProDetail(item){
          let url = '../sinovat/Trade/productDetail?id='+item.id + "&type=" + (item.ship ? 'ship' : 'product')
          window.open(url)
        },

        storeDetail(item){
          window.open('./Trade/storeHome?shopId='+item.id)
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
          this.search.salesqty = ''
          this.isActive2 = 0;
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
          this.search.overall = ''
          this.isActive = 0;
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
  height: 100%;
  .header{
    background-color: #F3F3F3;
    display: flex;
    align-items: center;
    height: 40px;
    .textRed{
      color: #E4393C;
      font-weight: bold;
    }
  }
  .content{
    padding-top: 30px;
    border: none;
    display: flex;
    flex-direction: row;
    justify-content: space-between;

    .left{
      width: 83%;
      .nav{
        background-color: #F3F3F3;
        border-top: 1px solid #DDDDDD ;
        border-bottom: 1px solid #DDDDDD;
        padding: 7px 9px;
        height: 40px;
        display: flex;
        flex-direction: row;
        width: 100%;
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
      }
      .body{
        width: 100%;
        .part{
          display: flex;
          flex-direction: row;
          padding: 30px 0 30px 21px;
          flex: 1;
          border-bottom: 1px solid #F3F3F3;
          .lf{
            display: flex;
            flex-direction: row;
            max-height: 111px;
            width: 54%;
            cursor: pointer;
            >img{
              width: 20.5%;
              border-radius:50%;
            }
            .ct{
              display: flex;
              flex-direction: column;
              justify-content: center;
              margin-left: 27px;
              width: 70%;
              >p{
                color: #666666;
                line-height: 35px;
              }
              p:nth-child(1){
                font-size: 16px;
                color: #2778BE;
              }
            }
          }
          .rt{
            display: flex;
            flex-direction: row;
            justify-content: space-between;
            flex: 1;
            width: 46%;
            .pt{
              width: calc((100% - 60px)/3);
              flex-direction: column;
              border: 1px solid #E1E1E1;
              cursor: pointer;
              margin: 0 10px;
              .img{
                border-bottom: 1px solid #E1E1E1;
                height: 80%;
                display: flex;
                display: -webkit-flex;
                align-items: center;
                justify-content: center;

              }

              >p{
                line-height: 30px;
                font-size: 12px;
                color: #E4393C;
                padding-left: 10px;
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
    .right{
      width: 15%;
      border: 1px solid #DDDDDD;
      .nav{
        background-color: #F3F3F3;
        color: #666666;
        font-size: 16px;
        height: 40px;
        line-height: 40px;
        padding-left: 20px;
      }
      .body{
        width: 100%;
        .part{
          display: flex;
          flex-direction: column;
          align-items: center;
          margin-top: 10px;
          cursor: pointer;
          >img{
            width: 100%;
            margin: auto;
          }
          >p{
            line-height: 32px;
            color: #666666;
          }
        }
      }
    }
  }
  .footerImg{
    width: 100%;
  }
}

</style>
