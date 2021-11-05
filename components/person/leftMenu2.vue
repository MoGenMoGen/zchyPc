<template>
  <!-- 设计院 -->
  <div id="app">
    <!--个人中心页眉-->
    <div class="header">
      <p @click="toPage('./')">个人中心</p>
    </div>
    <div class="myShip"  v-for="(item,index) in navList" :key="index" >
<!--      <p :class="{active:item.isActive}" @click="toPage(item.href)" v-for="(item,index) in navList[0]" :key="index">{{item.nm}}</p>-->
      <p @click="toPage(item.href)" :class="{active:cd && item.cd == cd}"><img :src="item.img" alt=""/>{{item.nm}}</p>
      <p v-for="(item2,index2) in item.title" :key="index2" :class="{active:item2.cd == cd}" @click="toPage(item2.href)">
        <span v-if="item2.cd=='message' && msgNum">{{msgNum}}</span>
        {{item2.nm}}</p>
    </div>
  </div>
</template>

<script>
  import bM from "../../assets/img/personal/投标管理.png"
  import sQ from "../../assets/img/personal/选型报价.png"
  import mS from "../../assets/img/personal/我的船舶.png"
  import mO from "../../assets/img/personal/我的订单.png"
  import mC from "../../assets/img/personal/我的合同.png"
  import mD from "../../assets/img/personal/我的资料.png"
  import mF from "../../assets/img/personal/资金管理.png"
  import aM from "../../assets/img/personal/子账户维护.png"
  import qO from "../../assets/img/personal/快速下单.png"
  import bill from "../../assets/img/personal/发票.png"
    export default {
        name: "leftMenu2",
      props:['cd','msgNum'],
      data(){
          return{
            navList:[
				// {
    //           nm:'投标管理',
    //           img:bM,
    //           href:'bid',
    //           cd:'bid'
    //         },
			{
              nm:'选型报价',
              img:sQ,
              cd:'seleQuo',
              href:'seleQuo',
            },{
              nm:'我的船舶',
              img:mS,
              title:[{
                nm:'全部船舶',
                cd:'allShipList',
                href:'./allShipList'
              },{
                nm:'船舶档案',
                cd:'shipFile',
                href:'./shipFile',
              },{
                nm:'船舶展示',
                cd:'shipShow',
                href:'./shipShow'
              }],
            },{
              nm:'我的合同',
              img:mC,
              title:[{
                nm:'设计销售合同',
                cd:'myContract31',
                href:'./myContract?myCd=31'
              },{
                nm:'设计采购合同',
                cd:'myContract32',
                href:'./myContract?myCd=32'
              },{
                nm:'采购合同',
                cd:'myContract4',
                href:'./myContract?myCd=4'
              }]
            }, {
              nm:'我的订单',
              img:mO,
              title:[{
                nm:'采购订单',
                cd:'myOrder21',
                href:'./myOrder2?cdType=1'
              },{
                nm:'设计销售订单',
                cd:'myOrder221',
                href:'./myOrder2?cdType=21'
              },{
                nm:'设计采购订单',
                cd:'myOrder222',
                href:'./myOrder2?cdType=22'
              }]
            },{
              nm:'资金管理',
              img:mF,
              title:[{
                nm:'采购付款',
                cd:'purchase21',
                href:'./purchase2?myCd=1'
              },{
                nm:'设计收款',
                cd:'purchase22',
                href:'./purchase2?myCd=2'
              }],
            },{
              nm:'我的资料',
              img:mD,
              title:[{
                nm:'个人资料',
                cd:'persInfo',
                href:'./persInfo'
              },{
                nm:'我的消息',
                cd:'message',
                href:'./message'
              },{
                nm:'我的地址',
                cd:'addrList',
                href:'./addrList'
              },{
                nm:'修改密码',
                cd:'pswAlter',
                href:'./pswAlter'
              }],
            },
            {
              nm:'我的发票',
              img:bill,
              title:[{
                nm:'申请发票',
                cd:'myBill1',
                href:'./myBill?cdType=1'
              },{
                nm:'增票资质',
                cd:'myBill2',
                href:'./myBill?cdType=2'
              },{
                nm:'发票帮助中心',
                cd:'myBill3',
                href:'./myBill?cdType=3'
              }],
            },
			{
			  nm:'我的投标',
			  img:bM,
			  title:[{
			    nm:'投标报名',
			    cd:'bidRegistration',
			    href:'./bidRegistration'
			  },{
			    nm:'全部投标',
			    cd:'tender1',
			    href:'./tender?cdType=1'
			  },{
			    nm:'投标结果',
			    cd:'tender2',
			    href:'./tender?cdType=2'
			  }],
			},
            {
              nm:'快速下单',
              img:qO,
              cd:'quicklyOrder',
              href:'./quicklyOrder'
            },{
              nm:'子账户维护',
              img:aM,
              cd:'subAccount',
              href:'./subAccount'
            }],
            userInfo:{},
            authz:[],
            juriList:[],
          }
      },
      watch:{
        $route(e){

        },
      },
      mounted() {
        this.userInfo=JSON.parse(this.until.seGet('userInfo'))
        this.getAuthz()
      },
      methods:{
        async getAuthz(){
          if(this.userInfo.pid){  //有pid就是子账号
            let nowList=[]
            let qry = this.query.new()
            this.query.toW(qry,'pid','5052299821765632','EQ')
            this.juriList=await this.api.getjuri(this.query.toEncode(qry))//获取所有的权限列表
            let data1=await this.api.getAuthz()//获取当前子账号有的权限
            this.authz=data1.stringPermissions
            console.log(this.juriList)

            this.juriList.forEach(item=>{
              this.authz.forEach(i=>{
                if(item.perm==i){
                  nowList.push(item.nm)//将当前子账号拥有的权限名保存
                }
              })
            })
            // 将列表标记
            this.navList.forEach((item,index)=>{
              if(item.title){
                item.title.forEach(i=>{
                  nowList.forEach(j=>{
                    if(j==i.nm){
                      i.flag=1
                    }
                  })
                })
              }else{
                nowList.forEach(j=>{
                  if(j==item.nm){
                    item.flag=1
                  }
                })
              }
              this.$set(this.navList,index,this.navList[index])
            })
            console.log(nowList)
            this.navList.forEach((item,index)=>{
              if(item.title && item.nm!='我的资料'){
                item.title.forEach(i=>{
                  if(!i.flag){
                    i.href=''
                  }
                })
              }else{
                if(!item.flag && item.nm!='快速下单'){
                  item.href=''
                }
              }
              this.$set(this.navList,index,this.navList[index])
            })
            console.log(this.navList)
          }
        },
          toPage(url){
              if(url){
                this.$router.push(url)
              }else if(this.userInfo.pid){
              this.$message({
                message: '当前子账号没有权限',
                duration:'1500'
              });
            }
          },
      },
    }
</script>

<style lang="less" scoped>
  @import url("../../assets/css/init.less");
  #app{
    height: 100%;
    width: 100%;
    background-color: #FFFFFF;
    .header{
      width: 100%;
      height: 60px;
      background-color: #2778BE;
      >p{
        color: #FFFFFF;
        line-height: 60px;
        font-size: 20px;
        font-weight: bold;
        text-align: center;
        cursor: pointer;
      }
    }

    .myShip{
      width: 100%;
      background-color: #FFFFFF;
      border-bottom: 1px solid #F3F3F3;
      display: flex;
      flex-direction: column;
      align-items: center;
      padding-bottom: 20px;
      &:last-of-type{
        border: 0;
      }
      >p{
        line-height: 33px;
        color: #666666;
        font-size: 14px;
        cursor: pointer;
        position: relative;
        span{
          position: absolute;
          font-size: 12px;
          background: @redColor;
          color: #ffffff;
          border-radius: 5px;
          right: -2px;
          top:5px;
          transform: translateX(100%);
          line-height: normal;
          padding: 0 2px;
        }
      }
      p:nth-child(1){
        color: #333333;
        font-size: 16px;
        font-weight: bold;
        padding-top: 20px;
        text-align: center;
        display: flex;
        display: -webkit-flex;
        align-items: center;
        >img{
          width: 26px;
          padding-right: 10px;
        }
      }
      p:not(:nth-child(1)){
        padding-left: 18px;
      }
      p:hover{
        color: @themeColor;
      }
    }
    .active{
      color: #2778BE!important;
    }
  }
</style>
