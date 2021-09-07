<template>
  <!-- 补贴在线查询 -->
<div id="home" :style="{width:bWidth + 'px'}">
  <div class="header">
    <el-carousel :height="bannerHeight+'px'">
      <el-carousel-item v-for="item in adertList" :key="item.id">
        <img :src="item.imgUrl" style="width:100%">
      </el-carousel-item>
    </el-carousel>
    <el-breadcrumb separator-class="el-icon-arrow-right" :style="{width:width+'px'}" class="bc main">
      <el-breadcrumb-item :to="{ path: './pilotService' }">试点服务</el-breadcrumb-item>
      <el-breadcrumb-item ><p class="textRed">补贴申请列表</p></el-breadcrumb-item>
    </el-breadcrumb>
  </div>
  <div class="ad main" :style="{width:width+'px'}" style="background-color: #f3f3f3;margin-top: 35px;">
      <el-table
       :data="inquirtList"
       style="width: 100%"
       :header-cell-style="{'background-color':'#f7f7f7'}"
       @row-click="toDetail">
         <el-table-column
            prop="no"
            width="300"
            label="序号"
            align="center">
         </el-table-column>
         <el-table-column
           prop="projNm"
           width="300"
           label="项目名称"
           align="center">
         </el-table-column>
         <el-table-column
           width="300"
           prop="crtTm"
           label="申请日期"
           align="center">
         </el-table-column>
         <el-table-column
           width="300"
           prop="auditNm"
           label="申请状态"
           align="center">
         </el-table-column>
      </el-table>
  </div>
  <div class="footPage">
       <el-pagination
         background
         @current-change="handleCurrentChange"
         :current-page.sync="currentPage1"
         :page-size="pageSize"
         layout="prev, pager, next, jumper"
         :total="total">
       </el-pagination>
  </div>
  <div class="footerImg" :style="{width:bWidth+'px'}">
    <img src="@/assets/img/footer.png" alt=""/>
  </div>
</div>
</template>

<script>
  import {mapState} from "vuex";
  export default {
        name: "vision",
      data(){
          return{
            bannerHeight:'300',
            navImgHeight:'160',
            tabImgHeight:'285',
            adertList:[],
            list:[],
            inquirtList:[{no:1,ddd:'ddd'},{no:1,ddd:'ddd'},{no:1,ddd:'ddd'},{no:1,ddd:'ddd'}],
            currentPage1: 1,      //前往哪页
            pageSize:10,       //每页数量
            total:0,         //总数
          }
      },
      computed: {
        ...mapState([
          'bWidth',
          'width',
          'currentRole'
        ])
      },
    mounted() {
      this.getAdert()
      this.getList()
    },
    methods:{
      async getList(){
        this.inquirtList=[]
        let qry = this.query.new()
        this.query.toP(qry,this.currentPage1,this.pageSize)
        let data=await this.api.subsidiesList(this.query.toEncode(qry))
        this.total=data.page.total
        console.log('补贴列表')
        console.log(data)
        if(this.total>0){
          data.data.list.forEach((item,index)=>{
            item.crtTm=item.crtTm.split(' ')[0]
            item.no=index+1
            this.inquirtList.push(item)
          })
        }
      },
      async getAdert(){
        this.adertList = await this.api.getAdert('subsidies')
        if(this.adertList.length>0){
          let myImg = new Image()
          myImg.src = this.adertList[0].imgUrl
          myImg.onload = ()=>{
            if(myImg.width>this.bWidth){
              this.bannerHeight = myImg.height/myImg.width*this.bWidth
            }else {
              this.bannerHeight = myImg.height
            }
          }
        }
        this.list = await this.api.getAdert('JLJZ')
        this.getHeight()
      },
      toPage(url){
        this.$router.push(url)
      },
      toDetail(item){
        console.log(item)
          this.$router.push('/pilotService/inquiryDetail?id='+item.id)
        // if(item.auditCd=='PILOT_STATUS.10'){  //待审核
        //   this.$router.push('/pilotService/inquiryDetail?type=1&id='+item.id)
        // }else if(item.auditCd=='PILOT_STATUS.20'){  //审核中
        //   this.$router.push('/pilotService/inquiryDetail?type=2&id='+item.id)
        // }else if(item.auditCd=='PILOT_STATUS.30'){  //审核通过
        //   this.$router.push('/pilotService/inquiryDetail?type=3&id='+item.id)
        // }else if(item.auditCd=='PILOT_STATUS.40'){  //审核未通过
        //   this.$router.push('/pilotService/inquiryDetail?type=4&id='+item.id)
        // }
      },
      handleCurrentChange(val){
        this.currentPage1=val
        this.inquirtList=[]
        this.getList()()
      },
      getHeight(){
        this.navImgHeight=this.width*0.24*9/16;
        this.tabImgHeight=this.width*0.24;
        console.log('nav : '+this.navImgHeight+'tab : '+this.tabImgHeight)
      },
    }
  }
</script>

<style lang="less" scoped>
  @import url("../../assets/css/init.less");
#home{
  background-color: #FFFFFF;
  .header{
    .bc{
      height: 52px;
      display: flex;
      align-items: center;
      .textRed{
        color: #0064B2;
      }
    }
    .ad{
      margin-top: 11px;
    }
  }
  .footerImg{
    margin-top: 50px;
  }
  .footPage{
    display: flex;
    justify-content: center;
    margin: 44px 0 20px;
    .btn_confirm{
      margin-left: 43px;
    }
  }
}
</style>
