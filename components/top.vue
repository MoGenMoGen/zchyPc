<template>
      <div class="topMenu" :style="{width:bWidth+'px'}">
        <div class="main" :style="{width:width+'px'}">
           <p>客服电话： <span>{{tel}}</span></p>
          <div class="menu">
            <div class="loginInfo" v-if="userInfo && userInfo.username">
              <p @click="toPage('../personal/')"><img :src="user"/>{{userInfo.username}}</p>
              <span></span>
              <p @click="toLogout">退出</p>
            </div>
            <div class="loginInfo" v-else>
              <p @click="toPage('../login/login')"><img :src="user"/>登录</p>
              <span></span>
              <p  @click="toPage('../login/register')"><img :src="register"/>注册</p>
            </div>
            <ul>
              <li v-for="(item,index) in menuList" :key="index" @click="toPage(item.href)" :class="{active:item.show}" @mouseenter="changeMenu(index,1)"
                  @mouseleave="changeMenu(index,0)">
                {{item.nm}}
                <i class="el-icon-arrow-down" v-if="item.item && item.item.length>0"></i>
                <div class="menuItem" v-show="item.show">
                  <span></span>
                  <p v-for="(v,i) in item.item" :key="i" @click.stop="toPage(v.href)">{{v.nm}}</p>
                </div>
                <span v-if="index<menuList.length-1"></span>
              </li>
            </ul>
          </div>
          <el-popover
            placement="bottom"
            width="100"
            trigger="click">
            <img :src="wechat"/>
            <el-button slot="reference"><img :src="wechat"/>中创公众号</el-button>
          </el-popover>
        </div>
      </div>
</template>
<script>
  import register from './img/register.png'
  import user from './img/user.png'
  import wechat from './img/wechat.png'
  import {mapState} from "vuex";
  export default {
    props:[],
    data(){
      return{
        wechat,
        register,
        user,
        service:[],
        menuList:[{
          nm:'个人中心',
          href:'../personal/',
          show:false,
          item:[{
            nm:'我的订单',
            href:'../personal/myOrder1?cdType=1'
          },{
            nm:'我的预约',
            href:'/personal/subscribe'
          },{
            nm:'我的船舶',
            href:'/personal/allShipList'
          },{
            nm:'我的收藏',
            href:'../personal/collect'
          },{
            nm:'我的消息',
            href:'../personal/message'
          },{
            nm:'我的优惠券',
            href:'/personal/coupon'
          },{
            nm:'查看更多',
            href:'../personal/'
          }]
        },{
          nm:'客户服务',

          item:[{
            nm:'我的订单',
            href:'../personal/myOrder1?cdType=1'
          },{
            nm:'我的预约',
            href:'/personal/subscribe'
          },{
            nm:'我的船舶',
            href:'/personal/allShipList'
          }]
        },{
          nm:'帮助中心',
          href:'/commonPage?id=5009993740899328'
        },{
          nm:'新手指南',
          href:'/commonPage?id=5009994006057984'
        },{
          nm:'关于我们',
          href:'../about/us'
        }],
      }
    },
    components: {
    },
    watch:{

    },
    computed: {
      ...mapState([
        'ifLogin',
        'bWidth',
        'width',
        'userInfo',
        'tel',
      ])
    },
    mounted(){
      this.getServiceTel()
      // this.store.commit("tel",this.service[0].cont)
      console.log('this.tel',this.tel)
    },
    methods:{
      //退出
      toLogout(){
        this.api.logout().then(()=>{
          this.$message({
            message: '退出成功',
            type: 'success',
            duration:'1500'
          });
          this.store.commit('token',null)
          this.store.commit('userInfo',null)
          this.until.seRemove('token')
          this.until.seRemove('userInfo')
          setTimeout(()=>{
            this.$router.push('../')
          },1500)
        })
      },
      async getServiceTel(){
        let qry = this.query.new()
        let data = await this.api.serviceTel(this.query.toEncode(qry))
        this.service = data.data.list
        this.store.commit("tel",this.service[0].cont)
        console.log('this.tel',this.tel)
        console.log('this.service',this.service)
      },
      //页面跳转
      toPage(item){
        let url =  item.href || item
        console.log(url)
        if(!this.ifLogin && url.indexOf('personal')>-1) {
          this.$confirm('您未登录，立即登录?', '提示', {
            confirmButtonText: '确定',
            cancelButtonText: '取消',
            type: 'warning'
          }).then(() => {
            this.$router.push('../login/login')
          })
        }else {
          this.$router.push(url)
        }
      },
      //鼠标滑动
      changeMenu(index,type){
        if(type && this.menuList[index].item && this.menuList[index].item.length>0){
          this.menuList[index].show = true
        }else {
          this.menuList[index].show = false
        }
        this.$set(this.menuList,index,this.menuList[index])
      }
    }
  }
</script>
<style lang="less">
  @import url("../assets/css/init.less");
  .el-popover{
    width: 100px!important;
    img{
      width: 100%;
    }
  }
  /*.el-popper[x-placement^=bottom]{*/
    /*margin-top: 2px;*/
  /*}*/
  .topMenu{
    .el-button{
      padding: 0;
      border: none;
      background: none;
      font-size: 12px;
      span{
        display: flex;
        display: -webkit-flex;
        align-items: center;
        height: 38px;
        img{
          width: 18px;
          margin-left: 20px;
          margin-right: 5px;
        }
      }

    }
  }
</style>
<style lang="less" scoped>
  @import url("../assets/css/init.less");
  .topMenu{
    background: #F3F3F3;
    height: 35px;
    line-height: 35px;
    .main{
      display: flex;
      display: -webkit-flex;
      align-items: center;
      >p{
        flex: 1;
        font-size: 14px;
        color: #666666;
        span{
          color: @themeColor;
          font-size: 18px;
        }
      }
      .menu{
        display: flex;
        display: -webkit-flex;
        align-items: center;
        span{
          margin: 0 19px;
          width: 1px;
          background: rgba(0,0,0,0.1);
          display: inline-block;
          height: 11px;
        }
        .loginInfo{
          display: flex;
          display: -webkit-flex;
          align-items: center;
          p{
            vertical-align: middle;
            cursor: pointer;
            display: inline-block;
            font-size: 12px;
            img{
              width: 12px;
              margin-right: 7px;
            }
          }
        }
        ul{
          padding-left: 40px;
          font-size: 12px;
          color: #666666;
          li{
            display: inline-block;
            position: relative;
            cursor: pointer;
            padding-left: 8px;
            border: 1px solid transparent;
            span{
              margin: 0;
              margin-left: 5px;
            }
            i{
              font-size: 6px;
            }
            .menuItem{
              position: absolute;
              top:35px;
              line-height: 26px;
              width: 250%;
              z-index: 99;
              padding: 8px 0;
              left: -1px;
              background:rgba(255,255,255,1);
              border:1px solid rgba(204, 204, 204, 1);
              box-shadow:0px 1px 7px 0px rgba(3, 46, 26, 0.35);
              span{
                position: absolute;
                z-index: 100;
                background: #ffffff;
                width: 40%;
                display: block;
                height: 10px;
                top: -8px;
                margin: 0;
                left: 0;
              }
              p{
                display: inline-block;
                width: 50%;
                text-indent: 8px;
                color: #666666!important;
              }
              p:hover{
                color: @themeColor!important;
              }
            }
          }
          li:hover{
            color: @themeColor
          }
          .active{
            background:rgba(255,255,255,1);
            border:1px solid rgba(204, 204, 204, 1);
            box-shadow:0px 1px 7px 0px rgba(3, 46, 26, 0.35);
            color: @themeColor;
            span{
              background: none;
            }
          }
        }

      }
    }
  }
</style>

