<template>
<div id="home" :style="{width: bWidth+'px'}">
  <!--头部-->
  <design-appo :show="designShow" @close="close" @submit="submit"></design-appo>
  <div class="header main" :style="{width:width + 'px'}">
    <el-breadcrumb separator-class="el-icon-arrow-right" :style="{width:width+'px'}" class="bc main">
      <el-breadcrumb-item :to="{ path: './index' }">首页</el-breadcrumb-item>
      <el-breadcrumb-item :to="{ path: '/shipDisplay/shipMan' }">船舶管理</el-breadcrumb-item>
      <el-breadcrumb-item ><p class="textRed">监理单位详情</p></el-breadcrumb-item>
    </el-breadcrumb>
  </div>
  <!--内容-->
  <div class="body main" :style="{width:width + 'px'}">
    <div class="left">
      <span>
              <img style="width: 100%;"  v-if="info.imgUrl" :src="info.imgUrl" alt=""/>
      </span>
    </div>

    <div class="right">
      <p>{{info.company}}</p>
      <p>{{info.summery}}</p>
      <p>公司等级：{{info.evaluateNm}}</p>
      <ul>
        <p><img src="@/assets/img/shipDisplay/地址.png" alt=""/><span>公司地址：</span>{{info.addrNm}}{{info.address}}</p>
        <p><img src="@/assets/img/shipDisplay/联系邮箱.png" alt=""/><span>联系邮箱：</span>{{info.email}}</p>
        <p><img src="@/assets/img/shipDisplay/联系人.png" alt=""/><span>联系人：</span>{{info.linkMan}}</p>
        <p><img src="@/assets/img/shipDisplay/联系电话.png" alt=""/><span>联系电话：</span>{{info.mob}}</p>
      </ul>
    </div>
  </div>
  <!-- 内容-->
  <div class="content main" :style="{width:width + 'px'}">
    <div class="head">
      <p>监理单位详情</p>
    </div>
    <div v-html="info.intro" class="cont">
    </div>
  </div>
  <!--脚部-->
  <div class="footer" :style="{width:bWidth+'px'}">

    <img src="../../assets/img/bg.png" alt=""/>

  </div>

</div>
</template>

<script>
    import {mapState} from "vuex";
    import DesignAppo from "@/components/designAppo";
    export default {
        name: "design",
        components: {DesignAppo},
      data(){
          return{
            designShow:false,
            id:'',
            info:{},
            list2:[],
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
      async asyncData(context){
        if(process.client){
          return
        }

        let info = await context.app.api.getRoleDetail2(context.query.id)

        let qry = context.app.query.new()
        context.app.query.toW(qry,'designId',context.query.id,'EQ')
        let data = await context.app.api.shipList2(context.app.query.toEncode(qry))
        return{
          info:info,
          data:data,
        }
      },
      async mounted(){
        this.id = this.until.getQueryString('id')
        this.info = await this.api.getRoleDetail2(this.id)
        this.getShipList()
      },
      methods:{
        async getShipList(){
          let qry = this.query.new()
          this.query.toW(qry,'designId',this.id,'EQ')
          this.list2 = await this.api.shipList2(this.query.toEncode(qry))
          this.list2.forEach((item,index)=>{
            this.objectFitImages(this.$refs['img'+index])
          })
          console.log(this.list2)
          // this.list2.push(...this.list2)
        },
        close() {
          // 取消弹窗回调
          this.designShow = false
        },
        submit() {
          // 确认弹窗回调
          this.designShow = false
        },
        toDetail(item){
          this.$router.push({
            path:'./showDetail',
            query:{
              id:item.id,
            }
          })
        },

      }
    }
</script>

<style lang="less" scoped>
  @import url("../../assets/css/init.less");
#home{
  background-color: #F3F3F3;
}
  .header{
    height: 54px;
    align-items: center;
    display: flex;
    .textRed{
      color: @themeColor;
    }
  }
  .body{
    padding: 30px;
    display: flex;
    background-color: #FFFFFF;
    .left{
      width: 28%;
      display: flex;
      display: -webkit-flex;
      img{
        /*margin: auto;*/
      }
    }
    .right{
      flex: 1;
      margin-left: 3%;
      position: relative;
      button{
        position: absolute;
        bottom: 13px;
        right: 6px;
        width: 157px;
        height: 42px;
        color: #FFFFFF;
        background-color: #2778BE;
        border: none;
        border-radius: 2px;
        cursor: pointer;
      }
      i{
        margin-right: 9px;
      }
      p{
        line-height: 30px;
        color: #666666;
        span{
          text-align: justify;
          text-align-last: justify;
          display: inline-block;
          width: 75px;
        }
      }
      img{
        width: 16px;
        height: 16px;
        vertical-align: middle;
        margin-right: 9px;
      }
      //标题
      >p:nth-child(1){
        font-size: 20px;
        color: #303030;
        line-height: 40px;
        font-weight: bold;
      }
      //公司等级
      >p:nth-child(3){
        font-size: 16px;
        color: #666666;
        line-height: 55px;
      }
      //内容
      >p:nth-child(2){
        font-size: 14px;
        color: #999999;
        line-height: 25px;
      }
      ul{
        p{
          display: inline-block;
          width: 49%;
        }
      }
    }
  }
  .content{
    margin-top: 30px;
    background-color: #FFFFFF;
    padding: 21px;
    .head{
      background-color: #ECECEC;
      height: 40px;
      p{
        line-height: 40px;
        padding-left: 28px;
        color: #666666;
      }
    }
    .cont{
      padding: 20px 25px;
    }
  }
  .footer{
    margin-top: 40px;
  }
</style>
