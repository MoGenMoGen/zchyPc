<template>
<!--  我的优惠券-->
    <div class="main">
      <div style="padding:0 30px">
        <tobbar :title="title" :show-all="false"></tobbar>
      </div>
      <p style="border-bottom: 1px solid rgba(0,0,0,0.1);"></p>

      <div class="line2">

        <div>
          <p v-for="(item,index) in tabList" :key="item.id"  :class="{active2: tabId==item.id}" @click="chooseTab(item,index)">
            <span>{{item.nm}}</span>
          </p>
          <!--          30*(tabIndex+1)加上一个间距-->
          <span class="btm" :style="{left:tabIndex*64+30*(tabIndex+1)+'px'}"></span>
        </div>
      </div>


      <div class="line4">
        <div class="item" v-for="item in list" :key="item.id">
          <div class="card">

            <card :item="item" :type="tabId"></card>

          </div>
        </div>

      </div>

<!--      <div class="line5">-->
<!--        <el-pagination-->
<!--          @size-change="handleSizeChange"-->
<!--          @current-change="handleCurrentChange"-->
<!--          :current-page="currentPage"-->
<!--          :page-sizes="[100, 200, 300, 400]"-->
<!--          :page-size="100"-->
<!--          prev-text="上一页"-->
<!--          next-text="下一页"-->
<!--          layout="total, sizes, prev, pager, next, jumper"-->
<!--          :total="400">-->
<!--        </el-pagination>-->
<!--      </div>-->


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
    export default {
        name: "allShip",
        components:{
            tobbar,
            card,
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
                title:'我的优惠券',

                tabList:[
                    {
                        id:1,
                        nm:'未使用',
                    },
                    {
                        id:2,
                        nm:'已使用',
                    },
                    {
                        id:3,
                        nm:'已过期',
                    },

                ],
                currentPage:1,
                tabId:1,
                tabIndex:0,
                list:[
                    {
                        id:'1',
                        price:'1320',
                        desc:'满5000元可用',
                        time1:'2020-02-02 - 2020-02-02',
                        limit1:'仅可购买船舶设备部分产品',
                        limit2:'全平台使用',
                    },
                    {
                        id:'2',
                        price:'13200',
                        desc:'满5000元可用',
                        time1:'2020-02-02 - 2020-02-02',
                        limit1:'仅可购买船舶设备部分产品',
                        limit2:'全平台使用',
                    },
                    {
                        id:'3',
                        price:'13200',
                        desc:'满5000元可用',
                        time1:'2020-02-02 - 2020-02-02',
                        limit1:'仅可购买船舶设备部分产品',
                        limit2:'全平台使用',
                    },
                    {
                        id:'4',
                        price:'13200',
                        desc:'满5000元可用',
                        time1:'2020-02-02 - 2020-02-02',
                        limit1:'仅可购买船舶设备部分产品',
                        limit2:'全平台使用',
                    },{
                        id:'5',
                        price:'13200',
                        desc:'满5000元可用',
                        time1:'2020-02-02 - 2020-02-02',
                        limit1:'仅可购买船舶设备部分产品',
                        limit2:'全平台使用',
                    },{
                        id:'6',
                        price:'13200',
                        desc:'满5000元可用',
                        time1:'2020-02-02 - 2020-02-02',
                        limit1:'仅可购买船舶设备部分产品',
                        limit2:'全平台使用',
                    },{
                        id:'7',
                        price:'13200',
                        desc:'满5000元可用',
                        time1:'2020-02-02 - 2020-02-02',
                        limit1:'仅可购买船舶设备部分产品',
                        limit2:'全平台使用',
                    },
                    {
                        id:'8',
                        price:'320',
                        desc:'满5000元可用',
                        time1:'2020-02-02 - 2020-02-02',
                        limit1:'仅可购买船舶设备部分产品',
                        limit2:'全平台使用',
                    },{
                        id:'9',
                        price:'1320000',
                        desc:'满5000元可用',
                        time1:'2020-02-02 - 2020-02-02',
                        limit1:'仅可购买船舶设备部分产品',
                        limit2:'全平台使用',
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


    .line4{

      padding:0 15px;
      .item{
        vertical-align: top;
        width: 25%;
        display: inline-block;
        padding: 0 15px;
        color: #333333;
        font-size: 15px;
        margin-bottom: 30px;
        .card{

          width: 100%;
          height: 333px;


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
