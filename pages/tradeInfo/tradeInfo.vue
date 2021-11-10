<template>
<div id="home" :style="{width:bWidth + 'px'}">
  <div class="header">
    <el-carousel :height="bannerHeight+'px'" class="adert">
      <el-carousel-item v-for="item in adertList" :key="item.aiid">
        <div class="bannerImg" :style="{backgroundImage:'url('+item.imgUrl+')'}"  @click="toUrl(item.locUrl)"/>
      </el-carousel-item>
    </el-carousel>
  </div>
  <div class="content main" :style="{width:width + 'px'}">
    <div class="partA">
      <info-menu @changeActive="getData"></info-menu>
    </div>
    <div class="partB">
      <div class="info" v-for="(item,index) in list" :key="index" @click="toPage(item)" v-if="id!=1">
        <div class="img">
<!--          <img :src="item.imgUrl" alt=""/>-->
          <img :src="item.imgUrl"
               class="el-images"
               :ref="'img'+index"
          />
          <!--< el-image-->
            <!--style="width: 100%; height: 100%"-->
            <!--:src="item.imgUrl"-->
            <!--fit="cover"></el-image>-->
        </div>
        <div class="right">
          <p>{{item.nm}}</p>
          <p >{{item.cont}}<span >查看更多<i class="el-icon-arrow-right"></i></span></p>
          <p>{{item.releTm}}</p>
        </div>
      </div>
      <div class="info" style="padding-bottom: 0;border-bottom: none;display: flex;flex-direction: column;" v-if="id==1">
        <div class="info-top">
          <div class="info-top-item">
            <span>项目名称：</span>
            <el-input placeholder="请输入项目名称" v-model="nm" clearable></el-input>
          </div>
          <div class="info-top-item">
            <span>报名截止日期：</span>
            <el-date-picker v-model="completeTm" type="daterange" range-separator="至" start-placeholder="开始日期" end-placeholder="结束日期"></el-date-picker>
          </div>
          <div class="info-top-item">
            <el-button type="primary" @click="search()">搜索</el-button>
          </div>
        </div>
        <el-table :data="tableData" max-height="577" style="width: 100%" :cell-style="{
        	    'text-align': 'center',
        	    color: '#333',
        	    'font-weight': '500',
        	  }" :header-cell-style="{
        	    color: '#606060',
        	    background: '#f8f8f8',
        		'text-align': 'center',
        	  }" @row-click="toDetail">
        	<el-table-column type="index" label="序号" min-width="50">
        		<template slot-scope="scope">
        			<span>{{(currentPage3 - 1) * pageSize + scope.$index + 1}}</span>
        		</template>
        	</el-table-column>
        	<el-table-column prop="nm" label="项目名称" min-width="250"></el-table-column>
          <el-table-column prop="publishTm" label="发布日期" min-width="150"></el-table-column>
          <el-table-column prop="completeTm" label="报名截止日期" min-width="150"></el-table-column>
          <el-table-column label="项目状态" min-width="150">
            <template slot-scope="scope">
              <p v-if="returnDate(scope.row.completeTm)" style="color: #2778BE;">进行中</p>
              <p v-else>已结束</p>
            </template>
          </el-table-column>
        </el-table>
      </div>
      <div class="foot mian" v-if="id!=1">
        <el-pagination
          v-if="this.list != ''"
          background
          @current-change="handleCurrentChange"
          :current-page.sync="currentPage3"
          :page-size='pageSize'
          layout="prev, pager, next, jumper"
          :total= 'totalNm'>
        </el-pagination>
      </div>
      <div class="foot mian" v-if="id==1">
        <el-pagination
            background
          @current-change="handleCurrentChange"
          :current-page.sync="currentPage3"
          :page-size='pageSize'
          layout="prev, pager, next, jumper"
          :total= 'totalNm'>
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
    import InfoMenu from "./components/infoMenu";
    export default {
        name: "tradeInfo",
      components: {InfoMenu},
      data(){
          return{
            bannerHeight:350,
            currentPage3: 1,
            pageSize: 10,
            list:[],
            tableData: [],
            id:'', //筛选id
            totalNm: 0, //当前页码
            NoNow:'', //未使用
            adertList:[],
            nowDate: '',
            nm: '',
            completeTm: ''
          }
        },
      computed: {
        ...mapState([
          'bWidth',
          'width',
          'currentRole'
        ]),
        returnDate() {
          return (date) => {
            if((new Date(date+' 23:59:59')).getTime()>this.nowDate){
              return true
            } else {
              return false
            }
          }
        }
      },
      watch:{
        $route(e){
          let arr = this.until.getQueryString('cid')
          console.log('此时arr',arr)
          this.id=arr
          this.currentPage3=1
          this.getData()
        },

      },
      async asyncData(context){
        if(process.client){
          return
        }
        let qry = context.app.query.new()
        let list = await context.app.api.infoList(context.app.query.toEncode(qry))
        let id = context.query.cid ? context.query.cid : list[0].id

        let qry2 = context.app.query.new()
        context.app.query.toW(qry2,'cid',id,'EQ')
        context.app.query.toP(qry2,1,100)
        let data = await context.app.api.tradeInfo(context.app.query.toEncode(qry2))
        return{
          list:list,
          data:data
        }
      },
      mounted(){
        let arr = this.until.getQueryString('cid')
        console.log('此时arr2',arr)
        this.getAdert()
        if(arr){
          this.id=arr
        }
        this.getData()
        // this.changeActive()
        this.nowDate = (new Date()).getTime()
      },
      methods:{
        toUrl(url){
          if(url){
            window.location.href = url
          }
        },
        async getAdert(){
          this.adertList = await this.api.getAdert('PChyxx')
          if(this.adertList.length>0){
            let myImg = new Image()
            myImg.src = this.adertList[0].imgUrl
            myImg.onload = ()=>{
              // this.bannerHeight = myImg.height/myImg.width*this.bWidth
              this.bannerHeight = myImg.height
            }
          }
        },
        handleCurrentChange(val) {
          this.currentPage3 = val
          this.getData()
        },
        toPage(item){
          this.$router.push({
            path:'./infoDetail',
            query:{
              id:item.id,
              cid:item.cid
            }
          });
        },
        async getData(val){
          let qry = this.query.new()
          if(!this.id && val){
            this.id =val
          }
          if(this.id!=1) {
            this.query.toW(qry,'cid',this.id,'EQ')
            // this.query.toO(qry,'seq','asc')
            // this.query.toO(qry,'releTm','desc')
            this.query.toP(qry,this.currentPage3,this.pageSize)
            let data = await this.api.tradeInfo(this.query.toEncode(qry))
            this.list = data.data.list
            this.totalNm =  data.page.total
            this.list.forEach((item,index) =>{
              item.cont = item.cont.replace(/<\/?[^>]*>/g, "").slice(0,150) + '...';
              item.cont = item.cont.replace(/(&nbsp;)/g, "");
              this.objectFitImages(this.$refs['img'+index])
            // console.log(item.id)
            })
          } else {
            if(this.nm) {
              this.query.toW(qry,'nm',this.nm,'LK')
            }
            if(this.completeTm) {
              let timeSearch = this.until.formatTime(this.completeTm[0])+','+this.until.formatTime(this.completeTm[1])
              this.query.toW(qry,'completeTm',timeSearch,'BT')
            }
            this.query.toO(qry,'publishTm','desc')
            this.query.toP(qry,this.currentPage3,this.pageSize)
            this.query.toW(qry,'audit','2','EQ')
            this.api.getBidList(this.query.toEncode(qry)).then(res => {
              res.data.list.forEach(item => {
                item.completeTm = item.completeTm.substring(0,10)
                item.publishTm = item.publishTm.substring(0,10)
              })
              this.tableData = res.data.list
              this.totalNm = res.page.total
            })
          }
        },
        search() {
          this.getData()
        },
        toDetail(row,col,e) {
          this.$router.push({
            path: '../tradeInfo/zbDetail',
            query: {
              id: row.id,
              cid:1
            }
          })
        }
      },

    }
</script>

<style lang="less">
  #home{
    /*卡片图片*/
    .el-carousel__item:nth-child(2n) {
      /*background-color: #99a9bf;*/
    }
    .el-carousel__item:nth-child(2n+1) {
      /*background-color: #d3dce6;*/
    }
  }
</style>
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
      width: 100%;
      text-align: center;
      /*max-height: 350px;*/
      .bannerImg{
        height: 100%;
        background-position: center;
      }
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
      flex:1;
      .info{
        display: flex;
        flex: 1;
        padding: 30px 0;
        border-bottom: 1px solid #E8E8E8;
        cursor: pointer;
        &:first-of-type{
          padding-top: 0;
        }
        .img{
          height: 160px;
          width: 240px;
          display: flex;
          flex-shrink: 0;
          >img{
            /*margin: auto;*/
            object-fit: cover;

          }
        }
        .right{
          margin-left: 39px;
          display: flex;
          flex-direction: column;
          flex-wrap: wrap;
          justify-content: space-between;
          flex: 1;
          p:nth-child(1){
            font-size: 18px;
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
        .info-top {
          display: flex;
          align-items: center;
          margin-bottom: 30px;
          .info-top-item {
            display: flex;
            align-items: center;
            margin-right: 40px;
            .el-input {
              width: 240px;
            }
            .el-range-editor {
              width: 400px;
            }
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
