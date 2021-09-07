<template>
<!--  我的积分-->
    <div class="main">
      <div style="padding:0 30px">
        <tobbar :title="title" :show-all="false"></tobbar>
      </div>
      <p style="border-bottom: 1px solid rgba(0,0,0,0.1);"></p>


      <div class="line1">
        <p>

          <img :src="jifen">
          <span>8000</span>
          <span>当前积分</span>
        </p>

        <p>新增地址</p>

      </div>



      <div class="line2">

        <div>
          <p v-for="(item,index) in tabList" :key="item.id"  :class="{active2: tabId==item.id}" @click="chooseTab(item,index)">
            <span>{{item.nm}}</span>
          </p>
          <!--          30*(tabIndex+1)加上一个间距-->
          <span class="btm" :style="{left:tabIndex*64+30*(tabIndex+1)+'px'}"></span>
        </div>
      </div>



      <div class="line3">
        <p>
          事件
        </p>
        <p>
          时间
        </p>
        <p>
          积分
        </p>

      </div>
      <div class="line4">
        <div v-for="item in list" :key="item.id">
          <p>{{item.id}}</p>
          <p>{{item.nm}}</p>

          <p v-if="tabId==1">{{item.price}}</p>
          <p v-else style="color: #2778BE">{{item.price}}</p>
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
    import successIc from '../../assets/img/success.png'
    export default {
        name: "allShip",
        components:{
            tobbar,
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
                title:'我的积分',

                tabList:[
                    {
                        id:1,
                        nm:'收入明细',
                    },
                    {
                        id:2,
                        nm:'支出明细',
                    },

                ],
                currentPage:1,
                tabId:1,
                tabIndex:0,
                list:[
                    {
                        id:'12345689525522',
                        nm:'新型4000kw中型休闲渔船',
                        price:'1320000',
                        status:'设计中',
                        time:'2020-11-11',
                    },
                    {
                        id:'12345689325522',
                        nm:'新型4000kw中型休闲渔船',
                        price:'1320000',
                        status:'设计中',
                        time:'2020-11-11',
                    },
                    {
                        id:'1234489525522',
                        nm:'新型4000kw中型休闲渔船',
                        price:'1320000',
                        status:'设计中',
                        time:'2020-11-11',
                    },
                    {
                        id:'12345589525522',
                        nm:'新型4000kw中型休闲渔船',
                        price:'1320000',
                        status:'已交船',
                        time:'2020-11-11',
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


    .line1{
      padding:0 18px 0 30px;
      width: 100%;
      height: 74px;
      position: relative;
      display: flex;
      align-items: center;
      justify-content: space-between;
      p:nth-of-type(1){
        display: flex;
        align-items: center;
        img{
          width:43px;
          height:43px;
        }
        span:nth-of-type(1){
          font-size:36px;
          font-family:Microsoft YaHei;
          font-weight:bold;
          color:rgba(229,66,69,1);
          margin-left: 20px;
        }
        span:nth-of-type(2){
          font-size:16px;
          font-family:Microsoft YaHei;
          font-weight:400;
          color:rgba(102,102,102,1);
          margin-left: 20px;
        }
      }
      p:nth-of-type(2){
        width:106px;
        height:34px;
        background:rgba(39,120,190,1);
        border-radius:4px;
        color: white;
        display: flex;
        align-items: center;
        justify-content: center;
        font-size:16px;
        &:hover{
          cursor: pointer;
        }
        &:active{
          opacity: 0.5;
        }
      }
    }

    .line2{

      margin-top: 15px;
      margin-bottom: 30px;
      width: 100%;

      div{
        /*padding: 0 30px;*/
        border-bottom:1px solid rgba(0,0,0,0.1);
        display: flex;
        height: 42px;
        align-items: center;
        position: relative;
        p{
          margin-left: 30px;
          width: 64px;
          display: flex;
          align-items: center;
          justify-content: center;
          height: 100%;
          /*对所有属性过渡0.2s*/
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
      font-size: 15px;
      p:nth-of-type(1){
        flex:1;
        text-align: center;
      }
      p:nth-of-type(2){
        flex:1.5;
        text-align: center;
      }
      p:nth-of-type(3){
        flex:1;
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
          text-align: center;

        }
        p:nth-of-type(2){
          flex:1.5;
          text-align: center;

        }
        p:nth-of-type(3){
          flex:1;
          color: #E4393C;
          text-align: center;
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
    color: #2572B5;
    /*border-bottom: 2px solid #2572B5;*/
  }

</style>
