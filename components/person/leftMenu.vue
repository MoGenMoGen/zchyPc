<!--<template>-->
  <!--<div id="app">-->
    <!--&lt;!&ndash;个人中心页眉&ndash;&gt;-->
    <!--<div class="header">-->
      <!--<p @click="toPage('./')">个人中心</p>-->
    <!--</div>-->
    <!--<div class="myShip"  v-for="(item,index) in navList" :key="index">-->
<!--&lt;!&ndash;      <p :class="{active:item.isActive}" @click="toPage(item.href)" v-for="(item,index) in navList[0]" :key="index">{{item.nm}}</p>&ndash;&gt;-->
      <!--<p><img :src="item.img" alt=""/>{{item.nm}}</p>-->
      <!--<p v-for="(item2,index2) in item.title" :key="index2" :class="{active:item2.cd == cd}" @click="toPage(item2.href)">{{item2.nm}}</p>-->
    <!--</div>-->
  <!--</div>-->
<!--</template>-->

<!--<script>-->
  <!--import mS from "../../assets/img/personal/我的船舶.png"-->
  <!--import mO from "../../assets/img/personal/我的订单.png"-->
  <!--import mC from "../../assets/img/personal/我的合同.png"-->
  <!--import mD from "../../assets/img/personal/我的资料.png"-->
  <!--import mF from "../../assets/img/personal/资金管理.png"-->
    <!--export default {-->
        <!--name: "leftMenu",-->
      <!--data(){-->
          <!--return{-->
            <!--cd:'',-->
            <!--navList:[{-->
              <!--nm:'我的船舶',-->
              <!--img:mS,-->
              <!--title:[{-->
                <!--nm:'全部船舶',-->
                <!--cd:'allShipList',-->
                <!--href:'./allShipList'-->
              <!--},{-->
                <!--nm:'我的预约',-->
                <!--cd:'subscribe',-->
                <!--href:'./subscribe'-->
              <!--},{-->
                <!--nm:'维保服务',-->
                <!--cd:'repair',-->
                <!--href:'./repair'-->
              <!--}],-->
            <!--},{-->
              <!--nm:'我的合同',-->
              <!--img:mC,-->
              <!--title:[{-->
                <!--nm:'船舶合同',-->
                <!--cd:'myContract1',-->
                <!--href:'./myContract?myCd=1'-->
              <!--},{-->
                <!--nm:'产品合同',-->
                <!--cd:'myContract2',-->
                <!--href:'./myContract?myCd=2'-->
              <!--}]-->
            <!--}, {-->
              <!--nm:'我的订单',-->
              <!--img:mO,-->
              <!--title:[{-->
                <!--nm:'船舶订单',-->
                <!--cd:'myOrder2',-->
                <!--href:'./myOrder?cdType=2'-->
              <!--},{-->
                <!--nm:'产品订单',-->
                <!--cd:'myOrder1',-->
                <!--href:'./myOrder?cdType=1'-->
              <!--}]-->
            <!--},{-->
              <!--nm:'资金管理',-->
              <!--img:mF,-->
              <!--title:[{-->
                <!--nm:'付款记录',-->
                <!--cd:'payRecord',-->
                <!--href:'./payRecord'-->
              <!--}],-->
            <!--},{-->
              <!--nm:'我的资料',-->
              <!--img:mD,-->
              <!--title:[{-->
                <!--nm:'个人资料',-->
                <!--cd:'persInfo',-->
                <!--href:'./persInfo'-->
              <!--},{-->
                <!--nm:'我的消息',-->
                <!--cd:'message',-->
                <!--href:'./message'-->
              <!--},{-->
                <!--nm:'我的地址',-->
                <!--cd:'addrList',-->
                <!--href:'./addrList'-->
              <!--},{-->
                <!--nm:'修改密码',-->
                <!--cd:'pswAlter',-->
                <!--href:'./pswAlter'-->
              <!--}],-->

            <!--}]-->
          <!--}-->
      <!--},-->
      <!--watch:{-->
        <!--$route(e){-->
          <!--let arr = this.$route.path.split('/');-->
          <!--this.cd = arr[2] ? arr[2] : 'index'-->
          <!--let arr2 = this.until.getQueryString('myCd')-->
          <!--if(arr2 != null){-->
            <!--this.cd = this.cd + arr2-->
          <!--}-->
          <!--let arr3 = this.until.getQueryString('cdType')-->
          <!--if(arr3 != null){-->
            <!--this.cd = this.cd + arr3-->
          <!--}-->
          <!--console.log("A:",this.cd)-->
        <!--},-->
      <!--},-->
      <!--mounted() {-->
        <!--let arr = this.$route.path.split('/');-->
        <!--this.cd = arr[2] ? arr[2] : 'index';-->
        <!--let arr2 = this.until.getQueryString('myCd')-->
        <!--if(arr2 != null){-->
          <!--this.cd = this.cd + arr2-->
        <!--}-->
        <!--let arr3 = this.until.getQueryString('cdType')-->
        <!--if(arr3 != null){-->
          <!--this.cd = this.cd + arr3-->
        <!--}-->
        <!--console.log('',this.cd)-->
      <!--},-->
      <!--methods:{-->
          <!--toPage(url){-->
              <!--this.$router.push(url)-->
            <!--},-->
      <!--},-->
    <!--}-->
<!--</script>-->

<!--<style lang="less" scoped>-->
  <!--@import url("../../assets/css/init.less");-->
  <!--#app{-->
    <!--height: 100%;-->
    <!--width: 100%;-->
    <!--background-color: #FFFFFF;-->
    <!--.header{-->
      <!--width: 100%;-->
      <!--height: 60px;-->
      <!--background-color: #2778BE;-->
      <!--&gt;p{-->
        <!--color: #FFFFFF;-->
        <!--line-height: 60px;-->
        <!--font-size: 20px;-->
        <!--font-weight: bold;-->
        <!--text-align: center;-->
        <!--cursor: pointer;-->
      <!--}-->
    <!--}-->

    <!--.myShip{-->
      <!--width: 100%;-->
      <!--background-color: #FFFFFF;-->
      <!--border-bottom: 1px solid #F3F3F3;-->
      <!--display: flex;-->
      <!--flex-direction: column;-->
      <!--align-items: center;-->
      <!--padding-bottom: 30px;-->
      <!--&:last-of-type{-->
        <!--border: 0;-->
      <!--}-->
      <!--&gt;p{-->
        <!--line-height: 33px;-->
        <!--color: #666666;-->
        <!--font-size: 14px;-->
        <!--cursor: pointer;-->
      <!--}-->
      <!--.active{-->
        <!--color: #2778BE;-->
      <!--}-->
      <!--p:nth-child(1){-->
        <!--color: #333333;-->
        <!--font-size: 16px;-->
        <!--font-weight: bold;-->
        <!--padding-top: 30px;-->
        <!--text-align: center;-->
        <!--display: flex;-->
        <!--display: -webkit-flex;-->
        <!--align-items: center;-->
        <!--&gt;img{-->
          <!--width: 26px;-->
          <!--padding-right: 10px;-->
        <!--}-->
      <!--}-->
      <!--p:not(:nth-child(1)){-->
        <!--padding-left: 18px;-->
      <!--}-->
      <!--p:hover{-->
        <!--color: @themeColor;-->
      <!--}-->
    <!--}-->

  <!--}-->
<!--</style>-->
