<template>
  <div class="person" id="default" :style="{width:bWidth+'px'}" v-loading="loading">
    <!--右边的悬浮菜单-->
    <!--<top v-show="topShow"></top>-->
    <my-menu v-show="headerShow"  @toTop="toTop"></my-menu>
    <my-header v-show="headerShow"></my-header>
    <el-scrollbar class="container">
      <div class="personMain2 main"  id="defaultScroll" :style="{width:width+'px'}">
        <div class="personMain" >
          <div id="left" :style="{width:(width-20)*1/5.9+'px'}">
            <left-menu1 v-if="!currentRole || !currentRole.identityCd" :cd="cd" :msgNum="msgNum"></left-menu1>  <!-- 没有入驻 -->
            <left-menu1 v-if="currentRole &&currentRole.identityCd=='identity10'" :cd="cd" :msgNum="msgNum"></left-menu1>  <!-- 船东 -->
            <left-menu2 v-if="currentRole &&currentRole.identityCd=='identity20'" :cd="cd" :msgNum="msgNum"></left-menu2>  <!-- 设计院 -->
            <left-menu3 v-if="currentRole &&currentRole.identityCd=='identity70'" :cd="cd" :msgNum="msgNum"></left-menu3>  <!-- 经销商 -->
            <left-menu4 v-if="currentRole &&currentRole.identityCd=='identity50'" :cd="cd" :msgNum="msgNum"></left-menu4>  <!-- 检验检测机构 -->
            <left-menu5 v-if="currentRole &&currentRole.identityCd=='identity40'" :cd="cd" :msgNum="msgNum"></left-menu5>  <!-- 监理监造机构 -->
            <left-menu6 v-if="currentRole &&currentRole.identityCd=='identity60'" :cd="cd" :msgNum="msgNum"></left-menu6>  <!-- 供应商 -->
            <left-menu7 v-if="currentRole &&currentRole.identityCd=='identity80'" :cd="cd" :msgNum="msgNum"></left-menu7>  <!-- 代理商 -->
            <left-menu8 v-if="currentRole &&currentRole.identityCd=='identity30'" :cd="cd" :msgNum="msgNum"></left-menu8>  <!-- 船厂 -->
            <left-menu9 v-if="currentRole &&currentRole.identityCd=='identity90'" :cd="cd" :msgNum="msgNum"></left-menu9>  <!-- 服务商 -->
          </div>
          <div id="right" :style="{width:(width-20)*4.9/5.9+'px'}">
            <nuxt />
          </div>
        </div>

      </div>

      <my-footer v-show="footerShow"></my-footer>
    </el-scrollbar>

  </div>
</template>
<script>
  import top from '~/components/top.vue'
  import myHeader from '~/components/myHeader.vue'
  import myFooter from '~/components/myFooter.vue'
  import myMenu from '~/components/myMenu.vue'
  import leftMenu1 from '~/components/person/leftMenu1.vue'
  import leftMenu2 from '~/components/person/leftMenu2.vue'
  import leftMenu3 from '~/components/person/leftMenu3.vue'
  import leftMenu4 from '~/components/person/leftMenu4.vue'
  import leftMenu5 from '~/components/person/leftMenu5.vue'
  import leftMenu6 from '~/components/person/leftMenu6.vue'
  import leftMenu7 from '~/components/person/leftMenu7.vue'
  import leftMenu8 from '~/components/person/leftMenu8.vue'
  // const leftMenu1 = resolve => require(['~/components/person/leftMenu1'],resolve); //船东
  // const leftMenu2 = resolve => require(['@/components/person/leftMenu2'],resolve); //设计院
  // const leftMenu3 = resolve => require(['@/components/person/leftMenu3'],resolve); //经销商
  // const leftMenu4 = resolve => require(['@/components/person/leftMenu4'],resolve); //检验检测机构
  // const leftMenu5 = resolve => require(['@/components/person/leftMenu5'],resolve); //监理监造机构
  // const leftMenu6 = resolve => require(['@/components/person/leftMenu6'],resolve); //供应商
  // const leftMenu7 = resolve => require(['@/components/person/leftMenu7'],resolve); //代理商
  // const leftMenu8 = resolve => require(['@/components/person/leftMenu8'],resolve); //船厂
  // const leftMenu9 = resolve => require(['@/components/person/leftMenu9'],resolve); //船东
  import {mapState} from "vuex";
  import LeftMenu9 from "~/components/person/leftMenu9.vue";
  export default {
    data(){
      return{
        bWidth:1000,
        width:1000,
        headerShow:true,
        topShow:true,
        footerShow:true,
        currentCd:'',
        cd:'',
      }
    },
    components: {
      LeftMenu9,
      leftMenu1,leftMenu2,leftMenu3,leftMenu4,leftMenu5,leftMenu6,leftMenu7,leftMenu8,
      top,
      myHeader,
      myFooter,
      myMenu
    },
    computed: {
      ...mapState([
        'loading',
        'currentRole',
        'ifLogin',
        'msgNum'
      ])
    },
    watch:{
      $route(e){
        this.changeDevice()
        document.getElementById('defaultScroll').scrollIntoView()
        let arr = e.path.split('/')
        this.currentCd = arr[1] ? arr[1] : 'index'
        this.getCd()
      },
      currentRole:{
        handler:function (v){
          if(!v && process.client){
            //获取上次登录最后的角色
            this.api.toGet('currentInfo').then(data=>{
              if(data){
                this.store.dispatch('save',data)
              }else {
                this.api.roleList()
              }
            })
          }
        },
        immediate:true
      },
      currentCd:{
        handler:function (e) {

          if(e=='login' || e=='settleIn'){
            this.headerShow = false
          }else {
            this.headerShow = true
          }
          if(e=='login'){
            this.footerShow = false
          }else {
            this.footerShow = true
          }
          if(e=='login'){
            this.topShow = false
          }else {
            this.topShow = true
          }
        },
        immediate:true
      }
    },
    async mounted(){
      this.getWidth()
      this.changeDevice()
      window.onresize = () => {
        this.changeDevice()
        this.getWidth()
      }
      if(!this.until.seGet('token')){
        this.$router.push('/login/login')
        return
      }
      let arr = window.location.pathname.split('/')
      this.currentCd = arr[1] ? arr[1] : 'index'
      this.until.saveToStore()
      this.getCd()
      await this.api.msgUnRead()
      // console.log('未读消息：'+this.msgNum)

    },
    methods:{
      //切换设备
      changeDevice() {
        // console.log("=========== "+window.location.pathname+" ===========" )
        let isPC=this.App.IsPC()
        if(!isPC){
          if(window.location.pathname==='/sinovat/personal/'){  //个人中心首页
            window.location.href = window.location.origin+'/mob/views/person/index.html'
          }
          //船东
          if(window.location.pathname==='/sinovat/personal/allShipList'){  //全部船舶
            window.location.href = window.location.origin+'/mob/views/person/shipList.html'
          }
          if(window.location.pathname==='/sinovat/personal/subscribe'){  //我的预约
            window.location.href = window.location.origin+'/mob/views/person/subscribe.html'
          }
          if(window.location.pathname==='/sinovat/personal/repair'){  //维保服务
            window.location.href = window.location.origin+'/mob/views/person/repair.html'
          }
          if(window.location.pathname==='/sinovat/personal/myContract'){  //合同
            //  船舶合同1 产品合同2 设计销售合同31 设计采购合同32 采购合同4 建造合同5 监理监造合同6 检验检测合同7
            // let myCd= this.until.getQueryString('myCd')
            window.location.href = window.location.origin+'/mob/views/person/myContract.html'
          }
          if(window.location.pathname==='/sinovat/personal/myOrder1'){  //船舶订单2产品订单1
          let cdType = this.until.getQueryString('cdType')
            window.location.href = window.location.origin+'/mob/views/person/myOrder.html?orderType='+cdType+'&item={"id":0,"nm":"全部","statusCd":"01"}'
          }
          if(window.location.pathname==='/sinovat/personal/payRecord'){  //资金管理
            //收款记录1 付款记录2
            window.location.href = window.location.origin+'/mob/views/person/payRecord2.html'
          }
          if(window.location.pathname==='/sinovat/personal/persInfo'){  //个人资料
            window.location.href = window.location.origin+'/mob/views/person/persInfo.html'
          }
          if(window.location.pathname==='/sinovat/personal/message'){  //我的消息
            window.location.href = window.location.origin+'/mob/views/person/message.html'
          }
          if(window.location.pathname==='/sinovat/personal/addrList'){  //我的地址
            window.location.href = window.location.origin+'/mob/views/person/addrList.html'
          }
          if(window.location.pathname==='/sinovat/personal/pswAlter'){  //修改密码
            window.location.href = window.location.origin+'/mob/views/person/pswAlter.html'
          }
          if(window.location.pathname==='/sinovat/personal/quicklyOrder'){  //快速下单
            window.location.href = window.location.origin+'/mob/views/person/quickOrder.html'
          }
          if(window.location.pathname==='/sinovat/personal/myRecommend'){  //我的推荐
            window.location.href = window.location.origin+'/mob/views/person/myRecommend.html'
          }
          //设计院
          if(window.location.pathname==='/sinovat/personal/bid'){  //投标管理
            window.location.href = window.location.origin+'/mob/views/bid/bidList.html'
          }
          if(window.location.pathname==='/sinovat/personal/seleQuo'){  //选型报价
            window.location.href = window.location.origin+'/mob/views/shipDesign/seleQuo.html?type=add'
          }
          if(window.location.pathname==='/sinovat/personal/recordDetail'){  //选型报价详情
            let id = this.until.getQueryString('id')
            window.location.href = window.location.origin+'/mob/views/shipDesign/seleDtl.html?id='+id
          }
          if(window.location.pathname==='/sinovat/personal/shipFile'){  //船舶档案
            window.location.href = window.location.origin+'/mob/views/shipDesign/shipFile.html'
          }
          if(window.location.pathname==='/sinovat/personal/shipShow'){  //船舶展示
            window.location.href = window.location.origin+'/mob/views/shipDesign/shipShow.html'
          }
          if(window.location.pathname==='/sinovat/personal/myOrder2'){  //我的订单
            //采购订单1 设计销售订单21 设计采购订单22 建造订单3
            let cdType = this.until.getQueryString('cdType')
            window.location.href = window.location.origin+'/mob/views/person/myOrder.html?orderType='+cdType+'&item={"id":0,"nm":"全部","statusCd":"01"}'
          }
          if(window.location.pathname==='/sinovat/personal/purchase2'){  //资金管理
          //采购付款1 设计收款、建造收款、监理收款、检验检测、销售收款2
            window.location.href = window.location.origin+'/mob/views/person/payRecord2.html'
          }
          if(window.location.pathname==='/sinovat/personal/subAccount'){  //子账号维护
            window.location.href = window.location.origin+'/mob/views/person/accountList.html'
          }
          //船厂
          if(window.location.pathname==='/sinovat/personal/shipbuild'){  //船舶建造
            window.location.href = window.location.origin+'/mob/views/person/shipBuild.html'
          }
          //监理监造
          if(window.location.pathname==='/sinovat/personal/shipControl'){  //管控船舶
            window.location.href = window.location.origin+'/mob/views/shipControl/controlList.html'
          }
          //检验检测
          if(window.location.pathname==='/sinovat/personal/shipDetec'){  //检测船舶
            window.location.href = window.location.origin+'/mob/views/'
          }
          //供应商
          if(window.location.pathname==='/sinovat/personal/myOrder3'){  //销售订单
            window.location.href = window.location.origin+'/mob/views/person/myOrder.html?orderType=6&item={"id":0,"nm":"全部","statusCd":"01"}'
          }
          if(window.location.pathname==='/sinovat/personal/withMoney'){  //来往款项
            // window.location.href = window.location.origin+'/mob/views/'
          }
          //代理商
          if(window.location.pathname==='/sinovat/personal/profile'){  //客户档案
            window.location.href = window.location.origin+'/mob/views/follow/profileList.html'
          }
          if(window.location.pathname==='/sinovat/personal/profileDetail'){  //客户档案详情
            let id = this.until.getQueryString('id')
            window.location.href = window.location.origin+'/mob/views/follow/profileDtl.html?id='+id
          }
          if(window.location.pathname==='/sinovat/personal/opportunity'){  //我的商机
            window.location.href = window.location.origin+'/mob/views/follow/opportunity.html'
          }
          if(window.location.pathname==='/sinovat/personal/follow'){  //跟进记录
            window.location.href = window.location.origin+'/mob/views/follow/followList.html'
          }
          //服务商
          if(window.location.pathname==='/sinovat/personal/repairService'){  //维保服务
            window.location.href = window.location.origin+'/mob/views/person/repair.html'
          }
          if(window.location.pathname==='/sinovat/personal/contractDetail'){  //合同详情
            let id = this.until.getQueryString('id')
            window.location.href = window.location.origin+'/mob/views/person/contractDetail.html?id='+id
          }
          if(window.location.pathname==='/sinovat/order/orderDetail'){  //订单详情
            let id = this.until.getQueryString('id')
            window.location.href = window.location.origin+'/mob/views/trading/orderDetail.html?id='+id
          }
          if(window.location.pathname==='/sinovat/personal/msgDtl'){  //消息详情
            let id = this.until.getQueryString('id')
            window.location.href = window.location.origin+'/mob/views/person/msgDtl.html?id='+id
          }
          if(window.location.pathname==='/sinovat/personal/addAddr'){  //编辑地址
            let type = this.until.getQueryString('type')
            let info = this.until.getQueryString('info')
            window.location.href = window.location.origin+'/mob/views/person/addAddr.html?type='+type+'&info='+info
          }
          if(window.location.pathname==='/sinovat/personal/archives'){  //全部船舶——详情
            let id = this.until.getQueryString('id')
            let shipCd = this.until.getQueryString('shipCd')
            let shipStatus = this.until.getQueryString('shipStatus')
            window.location.href = window.location.origin+'/mob/views/person/archives.html?id='+id+'&shipCd='+shipCd+'&shipStatus='+shipStatus
          }
          if(window.location.pathname==='/sinovat/personal/shipFileDetail'){  //船舶档案——详情
            let id = this.until.getQueryString('id')
            let shipCd = this.until.getQueryString('shipCd')
            let shipStatus = this.until.getQueryString('shipStatus')
            if(this.until.getQueryString('formW')=='design'){
              window.location.href = window.location.origin+'/mob/views/person/archives.html?form=2&id='+id+'&shipCd='+shipCd+'&shipStatus='+shipStatus
            }else if(this.until.getQueryString('formW')=='build'){
              window.location.href = window.location.origin+'/mob/views/person/archives.html?buildW=2&id='+id+'&shipCd='+shipCd+'&shipStatus='+shipStatus
            }else if(this.until.getQueryString('formW')=='jiance'){
              window.location.href = window.location.origin+'/mob/views/shipControl/detecAdd.html?id='+id
            }else if(this.until.getQueryString('formW')=='jianli'){
              window.location.href = window.location.origin+'/mob/views/shipControl/controlAdd.html?id='+id
            }else{
              window.location.href = window.location.origin+'/mob/views/person/archives.html?id='+id+'&shipCd='+shipCd+'&shipStatus='+shipStatus
            }
          }
          if(window.location.pathname==='/sinovat/personal/addShip'){  //新增或修改船舶展示
            if(this.until.getQueryString('id')){
              let id=this.until.getQueryString('id')
              window.location.href = window.location.origin+'/mob/views/shipDesign/shipAdd.html?type=2&id='+id
            }else{
              window.location.href = window.location.origin+'/mob/views/shipDesign/shipAdd.html?type=1'
            }
          }

          if(window.location.pathname==='/sinovat/personal/collect'){  //我的收藏
            window.location.href = window.location.origin+'/mob/views/person/collect.html'
          }
          if(window.location.pathname==='/sinovat/personal/record'){  //选型记录
            window.location.href = window.location.origin+'/mob/views/person/record.html'
          }
          if(window.location.pathname==='/sinovat/personal/recordDetail'){  //选型记录详情
            let id=this.until.getQueryString('id')
            window.location.href = window.location.origin+'/mob/views/person/recordDtl.html?id='+id
          }
          if(window.location.pathname==='/sinovat/commonPage'){  //底部的详情
            let id=this.until.getQueryString('id')
            window.location.href = window.location.origin+'/mob/views/person/ruleDtl.html?id='+id
          }
          if(window.location.pathname==='sinovat/personal/signIn'){  //签到
            window.location.href = window.location.origin+'/mob/views/person/signIn.html'
          }

          // if(window.location.pathname===''){  //
          //   window.location.href = window.location.origin+'/mob/views/'
          // }
        }
      },
      toTop(){
        document.getElementById('default').scrollIntoView()
      },
      getCd(){
        let arr = this.$route.path.split('/');
        this.cd = arr[2] ? arr[2] : 'index';
        let arr2 = this.until.getQueryString('myCd')
        if(arr2 != null){
          this.cd = this.cd + arr2
        }
        let arr3 = this.until.getQueryString('cdType')
        if(arr3 != null){
          this.cd = this.cd + arr3
        }
        let arr4 = this.until.getQueryString('myCd2')
        if(arr4 != null){
          this.cd = this.cd + arr4
        }
        if(this.cd=='shipFileDetail'){
          this.cd='shipFile'
        }
        if(this.cd=='msgDtl'){
          this.cd = 'message'
        }
      },
      //获取宽度
      getWidth(){
        // console.log('获取宽度')
        let data = this.until.getWidth()
        this.bWidth = data.bWidth
        this.width = data.width
        this.store.commit('changeWidth',data)
      },
    }
  }
</script>
<style lang="less">
  html {
    font-family: 'Source Sans Pro', -apple-system, BlinkMacSystemFont, 'Segoe UI',
    Roboto, 'Helvetica Neue', Arial, sans-serif;
    font-size: 16px;
    -ms-text-size-adjust: 100%;
    -webkit-text-size-adjust: 100%;
    -moz-osx-font-smoothing: grayscale;
    -webkit-font-smoothing: antialiased;
    box-sizing: border-box;
    background: #F3F3F3;
  }
  .el-scrollbar__wrap {
    overflow-x: hidden;
  }
  *,
  *:before,
  *:after {
    box-sizing: border-box;
    margin: 0;
  }
  .person{
    min-height: 100%;
    /*display: flex;*/
    /*display: -webkit-flex;*/
    /*flex-direction: column;*/
    .container{
      /*flex: 1;*/

      .personMain{
        display: flex;
        display: -webkit-flex;
        padding-top: 20px;
        padding-bottom: 121px;
        background-image: url("../assets/img/bg.png");
        background-position: center bottom;
        background-repeat: repeat-x;
      }
      #left{

        flex-shrink:0;
        margin-right: 20px;
        background: #ffffff;
      }
      #right{
        background: #ffffff;

      }
    }
  }
</style>
