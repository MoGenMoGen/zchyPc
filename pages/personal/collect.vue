<template>
<!--  我的优惠券-->
    <div class="main">
      <div style="padding:0 30px">
        <tobbar :title="title" :show-all="false"></tobbar>
      </div>
      <p style="border-bottom: 1px solid rgba(0,0,0,0.1);"></p>


      <div class="line1">

        <div class="search">
          <p>
            <input placeholder="搜索名称" v-model="nm" >
          </p>

          <p @click="toSearch"><img :src="searchIc"></p>
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
      <div class="line4">
        <div class="item" v-for="item in list" :key="item.id">
          <div class="card">
            <cltCard :item="item" :type="type" v-show="tabId!=5 && tabId!=4 && tabId!=6"></cltCard>
            <!--学习和直播-->
            <learning :item="item" :type="tabId" v-show="tabId==5 || tabId==6"></learning>
            <shop :item="item" v-show="tabId==4"></shop>
          </div>
        </div>
      </div>
      <div class="line5">
        <el-pagination
          @current-change="handleCurrentChange"
          :current-page="pageNo"
          :page-size="pageSize"
          prev-text="上一页"
          next-text="下一页"
          layout="total, sizes, prev, pager, next, jumper"
          :total="total">
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
    // import card from "../../components/person/card";
    import shop from '@/components/person/shop'
    import learning from '@/components/person/learning'
    // import featured from "../../components/onlineTrading/featured";
    import cltCard from "../../components/person/cltCard";
    export default {
        name: "allShip",
        components:{
            tobbar,
            // card,
            // featured,
            cltCard,
          shop,
          learning
        },
        mounted(){
          this.getList()
        },
        methods:{
          async getList(){
            let qry = this.query.new()
            if(this.tabId!=5){
              this.query.toW(qry,'type',this.tabId,'EQ')
            }
            this.query.toP(qry,this.pageNo,this.pageSize)
            let data = null
            if(this.tabId==1){
              data = await this.api.collect1(this.query.toEncode(qry),this.nm)
            }else if(this.tabId==2 || this.tabId==3){
              data = await this.api.collect2(this.query.toEncode(qry),this.nm)
            }else if(this.tabId==4){
              data = await this.api.collect3(this.query.toEncode(qry),this.nm)
            }else if(this.tabId==5){
              data = await this.api.collect4(this.query.toEncode(qry),this.nm)
            }else {
              data = await this.api.collect5(this.query.toEncode(qry),this.nm)
            }
            this.list = data.data.list
            this.total = data.page.total
          },
            handleCurrentChange(val) {
               this.pageSize = val
              this.getList()
            },
            chooseTab(item,index){
                if(item.id!=this.tabId){
                    this.tabId=item.id
                    this.tabIndex=index
                    this.pageNo = 1
                    this.getList()
                }
            },
          toSearch(){
            this.pageNo = 1
            this.getList()
          }
        },
      watch:{
        tabId(){
          if(this.tabId==1){
            this.type = 'manage'
          }else if(this.tabId==2){
            this.type ='ship'
          }else if(this.tabId==3){
            this.type = 'product'
          }else if(this.tabId==4){
            this.type='study'
          }else if(this.tabId==5){
            this.type = 'shop'
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
                nm:'',
                title:'我的收藏',
                type:'manage',
                tabList:[
                    {
                        id:1,
                        nm:'船舶方案',
                    },
                  {
                    id:2,
                    nm:'船舶',
                  },
                    {
                        id:3,
                        nm:'产品',
                    },
                    {
                        id:5,
                        nm:'学习',
                    },
                  {
                    id:4,
                    nm:'店铺',
                  },
                  {
                    id:6,
                    nm:'直播',
                  },
                ],
                pageNo:1,
                pageSize:12,
                total:0,
                tabId:1,
                tabIndex:0,
                list:[]

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
      justify-content: flex-end;
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
          height: 300px;


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
