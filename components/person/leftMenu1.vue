<template>
  <!-- 船东 -->
  <div id="app">
    <!--个人中心页眉-->
    <div class="header">
      <p @click="toPage('./')">个人中心</p>
    </div>
    <div class="myShip"  v-for="(item,index) in navList" :key="index">
      <p @click="toPage(item.href)" :class="{active:cd && item.cd == cd}"><img :src="item.img" alt=""/>{{item.nm}}</p>
      <p v-for="(item2,index2) in item.title" :key="index2" :class="{active:item2.cd == cd}" @click="toPage(item2.href)">
        <span v-if="item2.cd=='message' && msgNum">{{msgNum}}</span>
        {{item2.nm}}</p>
    </div>
  </div>
</template>

<script>
  import mS from "../../assets/img/personal/我的船舶.png"
  import mO from "../../assets/img/personal/我的订单.png"
  import mC from "../../assets/img/personal/我的合同.png"
  import mD from "../../assets/img/personal/我的资料.png"
  import mF from "../../assets/img/personal/资金管理.png"
  import qO from "../../assets/img/personal/快速下单.png"
  import aM from "../../assets/img/personal/子账户维护.png"
    export default {
        name: "leftMenu1",
      props:['cd','msgNum'],
      data(){
          return{
            navList:[{
              nm:'我的船舶',
              img:mS,
              title:[{
                nm:'全部船舶',
                cd:'allShipList',
                href:'./allShipList'
              },{
                nm:'我的预约',
                cd:'subscribe',
                href:'./subscribe'
              },{
                nm:'维保服务',
                cd:'repair',
                href:'./repair'
              }],
            },{
              nm:'我的合同',
              img:mC,
              title:[{
                nm:'船舶合同',
                cd:'myContract1',
                href:'./myContract?myCd=1'
              },{
                nm:'产品合同',
                cd:'myContract2',
                href:'./myContract?myCd=2'
              }]
            }, {
              nm:'我的订单',
              img:mO,
              title:[{
                nm:'船舶订单',
                cd:'myOrder12',
                href:'./myOrder1?cdType=2'
              },{
                nm:'产品订单',
                cd:'myOrder11',
                href:'./myOrder1?cdType=1'
              }]
            },
              {
              nm:'资金管理',
              img:mF,
              title:[{
                nm:'船舶付款记录',
                cd:'payRecord11',
                href:'./payRecord?myCd2=11'
              },{
                nm:'商城付款记录',
                cd:'payRecord22',
                href:'./payRecord?myCd2=22'
              }],
            },

              {
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
            },{
              nm:'快速下单',
              img:qO,
              cd:'quicklyOrder',
              href:'./quicklyOrder'
              },{
              nm:'子账户维护',
              img:aM,
              cd:'subAccount',
              href:'./subAccount'
            }
            ],
            userInfo:{},
            authz:[],
            juriList:[],
          }
      },
      watch:{

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

            let data1=await this.api.getAuthz()
            this.authz=data1.stringPermissions
            console.log('权限:',this.authz)
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
          }
        },
          toPage(url){
            if(url){
              this.$router.push(url)
            }else if(this.userInfo.pid) {
              this.$message({
                message: '当前子账号没有权限',
                duration: '1500'
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
      .active{
        color: #2778BE;
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
