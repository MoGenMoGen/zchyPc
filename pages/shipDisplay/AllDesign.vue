<template>
<div id="home" :style="{width: bWidth+'px'}">
  <div class="header main" :style="{width:width + 'px'}">
    <el-breadcrumb separator-class="el-icon-arrow-right">
      <el-breadcrumb-item :to="{ path: './shipMan' }">船舶管理</el-breadcrumb-item>
      <el-breadcrumb-item :to="{ path: './shipDesign' }">船舶设计</el-breadcrumb-item>
      <el-breadcrumb-item ><p class="textRed">设计院所总览</p></el-breadcrumb-item>
    </el-breadcrumb>
  </div>
  <div class="content main" :style="{width:width + 'px'}">
    <!--<ul>-->
      <!--<li v-for="(item,index) in list" :key="index">-->
        <!--<img :src="item.logo" :alt="item.company"/>-->
      <!--</li>-->
    <!--</ul>-->
    <div class="body">
      <div class="part" v-for="(item,index) in list" :key="index">
        <div class="left">
          <img :src="item.imgUrl" :alt="item.company"/>
        </div>
        <div class="right">
          <p>{{item.company}}</p>
          <p><span>设计优势：</span>{{item.business}}</p>
          <p v-html="item.intro"></p>
          <button @click="toPage(item)">查看详情</button>
        </div>
      </div>
    </div>
  </div>
  <div class="footer">
    <img src="@/assets/img/footer.png" alt=""/>
  </div>
</div>
</template>

<script>
    import {mapState} from "vuex";
    import bg from '@/assets/img/shipDisplay/lunbot.png';
    export default {
        name: "AllDesign",
      data(){
          return{
            list:[],
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
        let qry = context.app.query.new()
        context.app.query.toW(qry,'identityCd','identity20','EQ')
        let data = await context.app.api.getRoleAll(context.app.query.toEncode(qry))
        return{
          data:data,
        }
      },
      mounted() {
          this.getDesign()
      },
      methods:{
          async getDesign(){
            let qry = this.query.new()
            this.query.toW(qry,'identityCd','identity20','EQ')
            this.list = await this.api.getRoleAll(this.query.toEncode(qry))
            this.list.forEach(item => {
              item.intro = item.intro.replace(/<\/?[^>]*>/g, "").slice(0, 30) + '...';
			  item.intro = item.intro.replace(/(&nbsp;)/g, "");
            })

          },
        toPage(item){
          this.$router.push({
            path:'./design',
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
  .content{
    padding: 20px;
    background-color: #FFFFFF;
    ul{
      display: flex;
      flex-direction: row;
      flex-wrap: wrap;
      li{
        display: flex;
        width: 11.1%;
        height: 51px;
        align-items: center;
        /*border-top: 1px solid #DDDDDD;*/
        /*border-left: 1px solid #DDDDDD;*/
        border: 1px solid #DDD;
        padding: 5px;
        >img{
          margin: auto;
        }
        /*&:nth-child(9n){*/
        /*  border-right: 1px solid #DDDDDD;*/
        /*}*/
        /*&:nth-child(n+10):nth-child(-n+18){*/
        /*  border-bottom: 1px solid #DDDDDD;*/
        /*}*/
      }
    }
    .body{
      width: 100%;
      /*margin-top: 60px;*/
      flex: 1;
      display: flex;
      flex-wrap: wrap;
      .part{
        width: 50%;
        height: 300px;
        display: flex;
        padding: 40px 0;
        justify-content: space-between;
        /*margin-top: 40px;*/
        border-bottom: 1px solid #E9E9E9;
        &:nth-child(n+5):nth-child(-n+6){
          border-bottom: none;
        }
        .left{
          width: 48%;
          display: flex;
          flex-shrink: 0;
          img{
            width: 100%;
            margin: auto;
          }
        }
        .right{
          display: flex;
          flex-direction: column;
          justify-content: space-between;
          margin-left: 3%;
          flex: 1;
          p{
            max-width: 83%;

          }
          p:nth-child(1){
            color: #333333;
            font-size: 18px;
            font-weight: bold;
            line-height: 25px;
            white-space: nowrap;
            text-overflow: ellipsis;
            overflow: hidden;
          }
          p:nth-child(2){
            color: #666666;
            line-height: 40px;
            white-space: nowrap;
            text-overflow: ellipsis;
            overflow: hidden;
          }
          p:nth-child(3){
            color: #999999;
            line-height: 25px;
          }
          button{
            border-radius: 2px;
            background-color: #2778BE;
            color: #FFFFFF;
            width: 107px;
            height: 33px;
            border: none;
            margin-top: 20px;
            cursor:pointer;
          }
        }
      }
    }
  }
  .footer{
    width: 100%;
  }
</style>
