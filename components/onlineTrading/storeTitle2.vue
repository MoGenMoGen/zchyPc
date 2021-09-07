<template>
<!--  用于跳转店铺首页-->
<div id="app" >
  <div class="backgroundByUpperHalf" :style="{width:bWidth+'px'}">
    <!--上部店铺信息栏-->
    <div class="upperHalf" :style="{width:width+'px'}">
      <!--店铺头像-->
      <div class="avatar">
        <img :src="info.shopImg"/>
      </div>
      <!--店铺名称/是否已关注-->
      <div class="name">
        <div class="attention">
          <p style="font-size: 20px">{{info.nm}}<img :src="rz"/></p>
          <p @click="follow" :class="{focus:info.isFollow}"><img :src="gz"/> {{info.isFollow ? '已关注' : '关注'}}</p>
        </div>
      </div>
      <!--搜索-->
      <div class="search">
        <div class="searchMain">
          <input v-model="search" placeholder="店铺内搜索" />
          <button @click="toSeacrh"> <i class="el-icon-search"></i></button>
        </div>
      </div>
      <!--右侧文字-->
      <div class="rightText">
        <div class="textRight">
          <p><img :src="zy"/> 主要营业：<span>{{info.mainGoods && info.mainGoods.length>13 ? info.mainGoods.slice(0,13)+'...' : info.mainGoods}}</span></p>
          <p><img :src="addr"/> 所在地区：<span>{{info.area}}</span></p>
          <p><img :src="time"/> 开店时间：<span>{{info.openTm? info.openTm : ''}}</span></p>
        </div>
      </div>
    </div>
  </div>
  <!--下部店铺Tab栏-->
  <div class="lowerHalf" :style="{width:bWidth+'px'}">
    <div class="tabList" :style="{width:width+'px'}">
      <p v-for="(item,index) in storeTabList" :key="index" @click="toChooseTab(item)" :class="{active:tabId==item.id}">{{item.nm}}</p>
    </div>
  </div>
</div>
</template>

<script>
    import {mapState} from "vuex";
    import rz from '@/assets/img/proDetail/rz.png'
    import addr from '@/assets/img/proDetail/addr.png'
    import time from '@/assets/img/proDetail/time.png'
    import zy from '@/assets/img/proDetail/zy.png'
    import gz from '@/assets/img/proDetail/gz.png'
    export default {
        name: "storeTitle",
      // props:['id','info'],
        props:{
          id:{
              type:Number | String
          }  ,

        },
      data(){
        return{
          info:{},
          rz,
          addr,
          time,
          gz,
          zy,
          search:'',
          storeTabList:[],
            tabId:0,//选中id

        }
      },
      computed:{
        ...mapState([
          'bWidth',
          'width'
        ])
      },
      watch:{
        id:{
          handler(){
            if(this.id){
              this.getInfo()
                this.getCatList()
            }
          },
          immediate:true
        }
      },
      methods:{
        //关注店铺
        follow() {
          //取消关注
          if (this.info.isFollow === 1){
            this.info.isFollow = 0;
            this.api.delFollow(this.info.id)
          }else {
            //关注店铺
            let param = {
              targetId: this.info.id,
              type: 4,
            }
            this.api.followShop(param)
            this.info.isFollow = 1;
          }
        },
          async getInfo(){
             this.info = await this.api.shopIntro(this.id)
          },
          async getCatList(){
              // let qry2 = this.query.new()
              // this.query.toW(qry2,'shopId',this.id,'EQ')
              this.storeTabList.push({
                  nm:'店铺首页',
                  id:0,
              })
              this.storeTabList.push({
                  nm:'全部商品',
                  id:1,
              })
              let param={
                  id:this.id
              }
              let tmp= await this.api.shopCatList(this.id)
              this.storeTabList.push(...tmp)
              this.storeTabList.push({
                  nm:'店铺简介',
                  id:'-1'
              })

          },
        //  切换中间导航栏
          toChooseTab(item){
              // if(item.id!=this.tabId){
              //     this.tabId=item.id
              //     this.$emit('changeTab',item)
              // }
              this.$router.push({
                  path:'/Trade/storeHome',
                  query:{
                      shopId:this.id,
                      tabId:item.id
                  }
              })

          },


        //店铺内搜索
        toSeacrh(){
          let url = '/Trade/storeHome?shopId='+this.id+'&keyWord='+this.search
          this.$router.push(url)
        },
      },
        mounted() {

        }
    }
</script>

<style lang="less" scoped>
  @import url("../../assets/css/init.less");
  #app {
    background-color: #FFFFFF;
  }
    .backgroundByUpperHalf{
      border-top: 1px solid #E1E6F0;
      height: 140px;
      .upperHalf{
        display: flex;
        display: -webkit-flex;
        align-items: center;
        margin: 0 auto;
        height: 100%;
        .avatar{
          width: 111px;
          height: 111px;
          border: 1px solid #F3F3F3;
          border-radius: 50%;
          display: flex;
          display: -webkit-flex;
          margin-right: 10px;
          overflow: hidden;
          img{
            width: auto;
            max-width: 100%;
            height: auto;
            max-height: 100%;
          }
        }
        .name{
          width: 31.1%;
          .attention{
            display: flex;
            flex-wrap: wrap;
            flex-direction: column;
            justify-content: space-around;
            margin-left: 11px;
            p:nth-child(1){
              font-size: 28px;
              color: #333333;
              /*font-weight: bold;*/
              line-height: 30px;
              margin-bottom: 16px;
              display: flex;
              display: -webkit-flex;
              align-items: center;
              white-space: nowrap;
              text-overflow: ellipsis;
              overflow: hidden;
              img{
                width: 23px;
                margin-left: 8px;
              }
            }
            p:nth-child(2){
              cursor: pointer;
              font-size: 12px;
              color: #FFFFFF;
              background-color: #FF3E3E;
              border-radius: 13px;
              line-height: 27px;
              padding-left: 14px;
              width: 84px;
              display: flex;
              display: -webkit-flex;
              align-items: center;
              img{
                width: 12px;
                margin-right: 5px;
              }
            }
            p.focus{
              color: #FFFFFF;
              background-color: #FF3E3E;
            }
          }
        }
        .search{
          width: 30%;
          .searchMain{
            overflow: hidden;
            height: 38px;
            line-height: 38px;
            border-right: 0;
            border-radius: 2px;
            display: flex;
            display: -webkit-flex;
            align-items: center;
            >input{
              flex: 1;
              height: 100%;
              text-indent: 15px;
              border: 1px solid @themeColor;
              border-right: 0;
              box-sizing: border-box;
              border-radius: 2px 0 0 2px;
            }
            button{
              background: @themeColor;
              border: 1px solid @themeColor;
              color: #ffffff;
              height: 100%;
              padding: 0 20px;
              border-radius: 0 2px 2px 0;
              cursor: pointer;
              box-sizing: border-box;
            }
          }
        }
        .rightText{
          width: 29.7%;
          display: flex;
          padding-left: 59px;
          flex-direction: column;
          justify-content: center;
          height: 100%;
          .textRight{
            display: flex;
            flex-direction: column;
            justify-content: space-around;
            >p{
              line-height: 29px;
              font-size: 12px;
              color: #333333;
              display: flex;
              display: -webkit-flex;
              align-items: center;
              img{
                width: 15px;
                margin-right: 6px;
              }
            }
          }
        }
      }
    }
  .lowerHalf{
    height: 46px;
    background: url("../../assets/img/proDetail/menuBg.png");
    background-repeat: repeat-x;
    background-position: center;
    .tabList{
      margin: 0 auto;
      display: flex;
      >p{
        width: 115px;
        line-height: 46px;
        color: #FFFFFF;
        text-align: center;
        cursor: pointer;
      }
    }
  }

  /*tabId激活*/
  /*.active{*/
  /*   font-size: 18px !important;*/
  /*  !*color: red !important;*!*/
  /*}*/

</style>
