<template>
  <div id="home" :style="{width:bWidth + 'px'}">
    <div class="content main" :style="{width:width + 'px'}">
      <div class="partA">
        <info-menu></info-menu>
      </div>
      <div class="partB">
        <div class="info" v-for="(item,index) in list" :key="index">
          <div class="img">
            <img :src="item.imgUrl" alt=""/>
          </div>
          <div class="right">
            <p v-html="item.nm"></p>
            <p >{{item.cont}}<span @click="toPage(item)">查看更多<i class="el-icon-arrow-right"></i></span></p>
            <p>{{item.releTm}}</p>
          </div>
        </div>
        <div class="foot mian">
          <el-pagination
            v-if="this.list != ''"
            background
            @current-change="handleCurrentChange"
            :current-page.sync="currentPage3"
            :page-size=pageSize
            layout="prev, pager, next, jumper"
            :total= totalNm>
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
  import {mapState} from "vuex";
  import InfoMenu from "../tradeInfo/components/infoMenu";

    export default {
        name: "infoSearch",
      components: {InfoMenu},
      data(){
          return{
            bannerHeight:350,
            currentPage3: 1,
            pageSize: 10,
            list:[],
            search:{
              keywords:'',
              pageNo:1,
              size:5,
            },
            id:'', //筛选id
            totalNm: 0, //当前页码
            NoNow:'', //未使用
            adertList:[],
            time:0,
          }
      },
      computed: {
        ...mapState([
          'bWidth',
          'width',
          'currentRole'
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
        bWidth(){
          if(this.adertList.length>0){
            let myImg = new Image()
            myImg.src = this.adertList[0].imgUrl
            myImg.onload = ()=>{
              // this.bannerHeight = myImg.height/myImg.width*this.bWidth
              if(myImg.width>this.bWidth){
                this.bannerHeight = myImg.height/myImg.width*this.bWidth
              }else {
                this.bannerHeight = myImg.height
              }
            }
          }
        }
      },
      mounted(){
        let arr = this.until.getQueryString('keyWord')
        this.search.keywords = arr
        this.time=arr+this.until.getQueryString('time')
        // this.getData();
      },
      methods:{

        async getAdert(){
          this.adertList = await this.api.getAdert('PChyxx')
          if(this.adertList.length>0){
            let myImg = new Image()
            myImg.src = this.adertList[0].imgUrl
            myImg.onload = ()=>{
              // this.bannerHeight = myImg.height/myImg.width*this.bWidth
              if(myImg.width>this.bWidth){
                this.bannerHeight = myImg.height/myImg.width*this.bWidth
              }else {
                this.bannerHeight = myImg.height
              }
            }
          }
        },
        handleCurrentChange(val) {
          this.search.pageNo = val
          this.getData()
        },
        //跳转至资讯详情
        toPage(item){
          this.$router.push({
            path:'../tradeInfo/infoDetail',
            query:{
              id:item.id,
              cid:item.cid
            }
          });
        },
        //获取数据，若不存在关键词，进入全部资讯页面
        async getData(){
          if(this.search.keywords){
            let qry = this.query.new()
            this.query.toW(qry,'nm',this.search.keywords,'LK')
            this.query.toP(qry,this.search.currentPage3,this.pageSize)
            let data = await this.api.tradeInfo(this.query.toEncode(qry))
            data.data.list.forEach(item => {
              item.nm = item.nm.replace(new RegExp(this.search.keywords,'g'),`<span style='color:#dd4b39'>${this.search.keywords}</span>`)
            })
            this.list = data.data.list
            this.totalNm=data.page.total
            this.list.forEach(item =>{
              item.cont = item.cont.replace(/<\/?[^>]*>/g, "").slice(0,150) + '...';
              item.cont = item.cont.replace(/(&nbsp;)/g, "");
            })
          }else if(!this.search.keywords){
            this.$router.push('../tradeInfo/tradeInfo')
          }
        }
      },
    }
</script>

<style lang="less" scoped>
  @import url("../../assets/css/init.less");
  #home{
    background-color: #FFFFFF;
    display: flex;
    flex-wrap: wrap;
  }
  .header{
    width: 100%;
    .adert{
      text-align: center;
      /*max-height: 350px;*/
    }
  }
  .content{
    padding-top: 50px;
    display: inline-flex;
    justify-content: space-between;
    .partA{

    }
    .partB{
      padding: 0 40px;
      .info{
        display: flex;
        flex: 1;
        padding: 30px 0;
        border-bottom: 1px solid #E8E8E8;
        &:first-of-type{
          padding-top: 0;
        }
        .img{
          height: 160px;
          width: 240px;
          display: flex;
          >img{
            margin: auto;

          }
        }
        .right{
          margin-left: 39px;
          display: flex;
          flex-direction: column;
          flex-wrap: wrap;
          justify-content: space-between;
          width: 730px;
          p:nth-child(1){
            font-size: 20px;
            color: #333333;
            line-height: 40px;
          }
          p:nth-child(2){
            color: #666666;
            font-size: 14px;
            width: 100%;
            line-height: 25px;
            >span{
              margin-left: 10px;
              color: @themeColor;
              cursor: pointer;
            }
          }
          p:nth-child(3){
            font-size: 12px;
            line-height: 30px;
            color: #999999;
          }
        }
      }
      .foot{
        margin-top: 40px;
        width: 100%;
        display: flex;
        justify-content: center;
      }
    }
  }
  .footerImg{
    width: 100%;
    margin-top: 53px;
  }
</style>
