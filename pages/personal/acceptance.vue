<template>
<!--  资金管理-检测收款-检验检测机构-->
    <div class="main">
      <div style="padding:0 30px">
        <tobbar :title="title" :show-all="false"></tobbar>
      </div>
      <p style="border-bottom: 1px solid rgba(0,0,0,0.1);"></p>


      <div class="line1">

        <div class="search">
          <p>
            <input placeholder="搜索订单编号" >
          </p>

          <p><img :src="searchIc"></p>
        </div>
      </div>


      <div class="line2">

        <div>
          <p v-for="(item,index) in tabList" :key="item.id"  :class="{active2: tabId==item.id}" @click="chooseTab(item,index)">
            <span>{{item.nm}}</span>
          </p>
          <!--          30*(tabIndex+1)加上一个间距-->
<!--          <span class="btm" :style="{left:tabIndex*64+30*(tabIndex+1)+'px'}"></span>-->
        </div>
      </div>


      <div class="line3">
        <p>
          订单编号
        </p>
        <p>
          付款单位
        </p>
        <p>
          付款时间
        </p>
        <p>
          订单总额
        </p>
        <p>
          已收
        </p>
        <p>
          未收
        </p>
        <p>
          操作
        </p>
      </div>
      <div class="line4">
        <div v-for="item in list" :key="item.id">
          <p>{{item.id}}</p>
          <p>{{item.nm}}</p>
          <p>{{item.time}}</p>
          <p>￥{{item.price}}</p>
          <p>￥{{item.yishou}}</p>
          <p>￥{{item.weishou}}</p>
          <p>
            <span class="sail" @click="showPop(item)">我要出售</span>
          </p>

        </div>

      </div>


      <div class="line5">
        <el-pagination
          @size-change="handleSizeChange"
          @current-change="handleCurrentChange"
          :current-page="currentPage"
          :page-sizes="[100, 200, 300, 400]"
          :page-size="100"
          prev-text="上一页"
          next-text="下一页"
          layout="total, sizes, prev, pager, next, jumper"
          :total="400">
        </el-pagination>
      </div>


    </div>
</template>

<script>
    import tobbar from "../../components/person/tobbar";
    import searchIc from '../../assets/img/personal/search.png'
    import deleteIc from '../../assets/img/personal/delete.png'
    import jifen from '../../assets/img/personal/jifen.png'
    import test1 from '../../assets/img/personal/test1.png'
    import successIc from '../../assets/img/success.png'
    import card from "../../components/person/card";
    import featured from "../../components/onlineTrading/featured";
    import cltCard from "../../components/person/cltCard";
    export default {
        name: "allShip",
        components:{
            tobbar,
            card,
            featured,
            cltCard
        },
        methods:{
            handleSizeChange(val) {
                console.log(`每页 ${val} 条`);
            },
            handleCurrentChange(val) {
                console.log(`当前页: ${val}`);
            },

            chooseTab(item,index){
                if(item.id!=this.tabId){
                    this.tabId=item.id
                    this.tabIndex=index

                }
            }
        },
        data(){
            return{
                searchIc,
                deleteIc,
                successIc,
                jifen,
                test1,
                title:'资金管理',

                tabList:[
                    {
                        id:1,
                        nm:'检测收款',
                    },
                    {
                        id:2,
                        nm:'订单付款',
                    },


                ],
                currentPage:1,
                tabId:1,
                tabIndex:0,
                list:[
                    {
                        id:'12345689525522',
                        nm:'新型中型休闲渔船',
                        time:'2020-11-11 12:00:00',
                        price:'1320000',
                        status:'设计中',
                        yishou:1000,
                        weishou:1000,
                    },
                    {
                        id:'12345689525522',
                        nm:'新型40w中型休闲渔船',
                        time:'2020-11-11 12:00:00',
                        price:'1320000',
                        status:'设计中',
                        yishou:1000,
                        weishou:1000,
                    },
                    {
                        id:'12345689525522',
                        nm:'新型中型休闲渔船',
                        time:'2020-11-11 12:00:00',
                        price:'1320000',
                        status:'设计中',
                        yishou:1000,
                        weishou:1000,
                    }, {
                        id:'12345689525522',
                        nm:'新型中型休闲渔船',
                        time:'2020-11-11 12:00:00',
                        price:'1320000',
                        status:'设计中',
                        yishou:1000,
                        weishou:1000,
                    }, {
                        id:'12345689525522',
                        nm:'新型4w中型休闲渔船',
                        time:'2020-11-11 12:00:00',
                        price:'1320000',
                        status:'设计中',
                        yishou:1000,
                        weishou:1000,
                    },




                ]

            }
        },
        layout:'person',

    }
</script>

<style scoped lang="less">
  .main{
    width: 100%;
    /*搜索框*/
    .line1{
      width: 100%;
      display: flex;
      align-items: center;
      height: 78px;
      padding-left: 29px;
      padding-right: 16px;

      .search{

        display: flex;
        align-items: center;
        p:nth-of-type(1){
          width: 250px;
          border-top: 1px solid #DFDFDF;
          border-bottom: 1px solid #DFDFDF;
          border-left: 1px solid #DFDFDF;
          border-top-left-radius: 5px;
          border-bottom-left-radius: 5px;
          height: 38px;
          padding: 0 12px;
          align-items: center;
          display: flex;
          input{
            border: none;
            outline: none;
            color: #666666;
          }
          ::-webkit-input-placeholder { /* Chrome/Opera/Safari */
            color: #999999;
          }
          ::-moz-placeholder { /* Firefox 19+ */
            color: #999999;
          }
          :-ms-input-placeholder { /* IE 10+ */
            color: #999999;
          }
          :-moz-placeholder { /* Firefox 18- */
            color: #999999;
          }
        }
        p:nth-of-type(2){
          flex-shrink: 0;
          background:#2778BE;
          height: 38px;
          display: flex;
          align-items: center;
          justify-content: center;
          width: 55px;
          border-top-right-radius: 5px;
          border-bottom-right-radius: 5px;
          &:hover{
            cursor: pointer;
          }
          img{
            width: 18px;
          }
        }

      }
    }


    .line2{
      padding: 0 30px;
      margin-top: 15px;
      margin-bottom: 30px;
      width: 100%;

      div{
        /*padding: 0 30px;*/
        border-bottom:1px solid #2778BE;
        display: flex;
        height: 42px;
        align-items: center;
        position: relative;

        p{

          width: 129px;
          display: flex;
          align-items: center;
          justify-content: center;
          height: 100%;
          /*对所有属性过渡0.2s*/
          transition: all 0.2s;
          &:hover{
            cursor: pointer;
          }
        }
        .btm{
          position: absolute;
          width: 64px;
          height: 100%;
          border-bottom: 2px solid #2778BE;
          /*对left属性过渡5秒*/
          transition: left 0.5s;
        }
      }


    }


    .line3{
      display: flex;
      align-items: center;
      height: 60px;
      padding: 0 31px;
      background:#F7F7F7;
      p:nth-of-type(1){
        flex:1;
      }
      p:nth-of-type(2){
        flex:1;
      }
      p:nth-of-type(3){
        flex:1;
      }
      p:nth-of-type(4){
        flex:0.5;
        text-align: center;
      }
      p:nth-of-type(5){
        flex:0.5;
        text-align: center;
      }
      p:nth-of-type(6){
        flex:0.5;
        text-align: center;
      }
      p:nth-of-type(7){
        flex:0.5;
        text-align: center;
      }

    }
    .line4{

      div{
        display: flex;
        height: 76px;
        align-items: center;
        border-bottom: 1px solid #DFDFDF;
        padding: 0 31px;
        color: #333333;
        font-size: 15px;
        p:nth-of-type(1){
          flex:1;
        }
        p:nth-of-type(2){
          flex:1;
        }
        p:nth-of-type(3){
          flex:1;
        }
        p:nth-of-type(4){
          flex:0.5;
          text-align: center;
        }
        p:nth-of-type(5){
          flex:0.5;
          text-align: center;
          color: #2778BE;
        }
        p:nth-of-type(6){
          flex:0.5;
          text-align: center;
          color: #E3393C;
        }
        p:nth-of-type(7){
          flex:0.5;
          display: flex;
          justify-content: center;
          flex-direction: column;
          span:hover{
            cursor: pointer;
          }
          .sail{
            margin: 5px auto;
            display: flex;
            color: #FFFFFF;
            background:#2778BE;
            width: 83px;
            height: 28px;
            align-items: center;
            justify-content: center;
            border-radius: 3px;
          }

        }
      }


    }
    .line5{
      padding: 30px 40px;
      width: 100%;
      display: flex;
      justify-content: flex-end;
    }

  }

  .active2{
    color: white;
    background: #2778BE;
    /*border-bottom: 2px solid #2572B5;*/
  }

</style>
