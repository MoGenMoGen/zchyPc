<template>
  <div id="home" :style="{width:bWidth + 'px'}">
    <div class="header">
      <img :height="bannerHeight+'px'" class="adert" alt="" src="../../assets/img/shipDisplay/建造规范.png" :style="{width:bWidth + 'px'}"/>
      <el-breadcrumb separator-class="el-icon-arrow-right" :style="{width:width+'px'}" class="bc main">
        <el-breadcrumb-item :to="{ path: './shipCat' }">船舶管理</el-breadcrumb-item>
        <el-breadcrumb-item :to="{ path: './shipBuild' }">船舶建造</el-breadcrumb-item>
        <el-breadcrumb-item ><p class="textRed">建造规范</p></el-breadcrumb-item>
      </el-breadcrumb>
    </div>
    <div class="content main" :style="{width:width + 'px'}">
      <div class="partA">
        <rule-menu  type="2"></rule-menu>
      </div>
      <div class="partB" >
        <div class="head" v-bind="infoDetail">
            <span @click="back"><i class="el-icon-arrow-left"></i>返回</span>
          <p>
            {{infoDetail.nm}}
          </p>
        </div>
        <div class="body" v-html="infoDetail.cont">
        </div>
      </div>

    </div>
    <div class="footerImg" :style="{width:bWidth+'px'}">
      <img src="../../assets/img/footer.png" alt=""/>
    </div>
  </div>
</template>

<script>
    import InfoMenu from "../tradeInfo/components/infoMenu";
    import {mapState} from "vuex";
    import RuleMenu from "./components/ruleMenu";

    export default {
        name: "ruleDetail",
      components: {RuleMenu, InfoMenu},
      data(){
        return{
          id:'',
          bannerHeight:346,
          infoDetail:{
          },
        }
      },
      computed: {
        ...mapState([
          'bWidth',
          'width'
        ])
      },
      watch:{

        $route(e){
          this.id = this.until.getQueryString('id')
          this.getInfo()
        },
      },
      async asyncData(context){
        if(process.client){
          return
        }
        let data = await context.app.api.standardDetail(context.query.id)
        return{
          data:data,
        }
      },
      mounted(){
        this.id = this.until.getQueryString('id')
        this.getInfo()
      },
      methods:{
          async getInfo(){
             this.infoDetail = await this.api.standardDetail(this.id)
          },

        back(){
          this.$router.go(-1)
        },
      },
    }
</script>

<style lang="less" scoped>
  @import url("../../assets/css/init.less");
  #home{
    background-color: #FFFFFF;
    display: flex;
    flex-wrap: wrap;
    .textRed{
      color: @themeColor;
    }
    .bc{
      height: 54px;
      display: flex;
      align-items: center;
    }
  }
  .header{
    width: 100%;
  }
  .content{
    padding-top: 50px;
    display: flex;
    .partA{

    }
    .partB{
      flex: 1;
      padding: 0 78px;
      .head{
        span{
          color: #2778BE;
          cursor: pointer;

        }
        i{
          color: #2778BE;
          cursor: pointer;

        }
        p{
          font-size: 26px;
          text-align: center;
          color: #333333;
          width: 100%;
          line-height: 70px;
        }
      }
    }
  }
  .footerImg{
    width: 100%;
    margin-top: 53px;
  }

</style>
