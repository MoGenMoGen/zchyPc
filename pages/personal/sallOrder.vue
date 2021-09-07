<template>
<!--  供应商销售订单不用-->
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
          <span class="btm" :style="{left:tabIndex*131+30*(tabIndex+1)+'px'}"></span>
        </div>
      </div>



      <div class="line3">
        <p>
          序号
        </p>
        <p>
          订单编号
        </p>
        <p>
          产品名称
        </p>
        <p>
          规格型号
        </p>
        <p>
          数量
        </p>

        <p>
          实付款
        </p>
        <p>
          交易状态
        </p>
        <p>
          交易操作
        </p>
      </div>
      <div class="line4">
        <div class="line4-item" v-for="item in list" :key="item.id">
          <p>{{item.id}}</p>
          <p>{{item.number}}</p>
          <p>{{item.nm}}</p>
          <p>{{item.model}}</p>
          <p>{{item.count}}</p>
          <p>￥{{item.pay}}</p>
          <p>{{item.status}}</p>

<!--          操作按钮,待买家付款-->
          <div>
            <p v-if="item.status=='待付款'" class="daifukuan">
              <span>提醒买家付款</span>
              <span>取消订单</span>
            </p>
            <p v-if="item.status=='已付款'"  class="yifukuan">
              <span>发货</span>
              <span>查看详情</span>
            </p>
            <p v-if="item.status=='待收货'"  class="daishouhuo">
              <span>填写物流单号</span>
              <span>提醒买家收货</span>
              <span>查看详情</span>
            </p>
            <p v-if="item.status=='已完成'"  class="yiwancheng">
              <span>查看详情</span>
              <span>删除订单</span>
            </p>

          </div>
        </div>

      </div>

      <div class="line5">
        <el-pagination
          @current-change="handleCurrentChange"
          :current-page="currentPage"
          :page-sizes="[100, 200, 300, 400]"
          :page-size="pageSize"
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
      data(){
        return{
          searchIc,
          deleteIc,
          successIc,
          jifen,
          title:'销售订单',

          tabList:[
            {
              id:1,
              nm:'全部',
            },
            {
              id:2,
              nm:'待收款',
            },
            {
              id:3,
              nm:'待发货',
            },
            {
              id:4,
              nm:'待收货',
            },
            {
              id:5,
              nm:'已完成',
            },


          ],
          currentPage:1,
          pageSize:5,
          tabId:1,
          tabIndex:0,
          list:[
            {
              id:'1',
              number:'1234567890',
              nm:'船舶设备名称',
              model:'BL112',
              count:2,
              pay:200,
              status:"待付款",
            },
            {
              id:'2',
              number:'1234567890',
              nm:'船舶设备名称',
              model:'BL112',
              count:2,
              pay:200,
              status:"已付款",
            },
            {
              id:'3',
              number:'1234567890',
              nm:'船舶设备名称',
              model:'BL112',
              count:2,
              pay:200,
              status:"待收货",
            },
            {
              id:'4',
              number:'1234567890',
              nm:'船舶设备名称',
              model:'BL112',
              count:2,
              pay:200,
              status:"已完成",
            },
            {
              id:'5',
              number:'1234567890',
              nm:'船舶设备名称',
              model:'BL112',
              count:2,
              pay:200,
              status:"已完成",
            },


          ]

        }
      },
      layout:'person',
        components:{
            tobbar,
        },
        watch:{
          $route(){
            this.tabId = this.until.getQueryString('tabId') ? this.until.getQueryString('tabId') : 0
          }
        },
        mounted(){
          this.tabId = this.until.getQueryString('tabId') ? this.until.getQueryString('tabId') : 0
        },
        methods:{
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
          width: 131px;
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
          width: 131px;
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
        flex:0.8;
        text-align: center;

      }
      p:nth-of-type(2){
        flex:1;

      }
      p:nth-of-type(3){
        flex:1;

      }
      p:nth-of-type(4){
        flex:1;

      }
      p:nth-of-type(5){
        flex:1;
        text-align: center;
      }
      p:nth-of-type(6){
        flex:1;
        text-align: center;
      }
      p:nth-of-type(7){
        flex:1;
        text-align: center;
      }
      p:nth-of-type(8){
        flex:1;
        text-align: center;
      }

    }
    .line4{

      .line4-item{
        display: flex;
        min-height: 76px;
        align-items: center;
        border-bottom: 1px solid #DFDFDF;
        padding: 0 31px;
        color: #333333;
        font-size: 15px;
        p:nth-of-type(1){
          flex:0.8;
          text-align: center;

        }
        p:nth-of-type(2){
          flex:1;


        }
        p:nth-of-type(3){
          flex:1;

        }

        p:nth-of-type(4){
          flex:1;


        }
        p:nth-of-type(5){
          flex:1;
          text-align: center;

        }
        p:nth-of-type(6){
          flex:1;
          color: #E4393C;
          text-align: center;
        }

        p:nth-of-type(7){
          flex:1;
          text-align: center;

        }
        div{
          flex:1;
          text-align: center;
          display: flex;
          align-items: center;
          justify-content: center;
          p{
            padding: 21px 0;
          }
          span{
            display: flex;
            align-items: center;
            justify-content: center;
            &:hover{
              cursor: pointer;
            }
            &:active{
              opacity: 0.5;
            }
          }
          /*代付款*/
          .daifukuan{

            span:nth-of-type(1){
              width:104px;
              height:28px;
              background:rgba(255,255,255,1);
              border:1px solid rgba(39, 120, 190, 1);
              border-radius:3px 3px 3px 3px;
              color: #2778BE;
              font-size: 14px;

            }
            span:nth-of-type(2){
              margin-top: 10px;
              font-size:15px;
              font-family:Microsoft YaHei;
              font-weight:400;
              color:rgba(153,153,153,1);
              line-height:24px;

            }
          }
          /*已发货*/
          .yifukuan{
            span:nth-of-type(1){
              width:104px;
              height:28px;
              background:#2778BE;
              border-radius:3px 3px 3px 3px;
              color: white;
              font-size: 14px;

            }
            span:nth-of-type(2){
              margin-top: 10px;
              font-size:15px;
              font-family:Microsoft YaHei;
              font-weight:400;
              color:#666666;
              line-height:24px;

            }
          }
        /*  待收货*/
          .daishouhuo{
            span:nth-of-type(1){
              width:104px;
              height:28px;
              background:rgba(255,255,255,1);
              border:1px solid rgba(39, 120, 190, 1);
              border-radius:3px 3px 3px 3px;
              color: #2778BE;
              font-size: 14px;

            }

            span:nth-of-type(2){
              margin-top: 15px;
              width:104px;
              height:28px;
              background:#2778BE;
              border-radius:3px 3px 3px 3px;
              color: white;
              font-size: 14px;

            }
            span:nth-of-type(3){
              margin-top: 10px;
              font-size:15px;
              font-family:Microsoft YaHei;
              font-weight:400;
              color:#666666;
              line-height:24px;

            }
          }
        /*  已完成*/
          .yiwancheng{
            span:nth-of-type(1){
              font-family:Microsoft YaHei;
              font-weight:400;
              color:rgba(39,120,190,1);
              line-height:24px;

            }

            span:nth-of-type(2){
              margin-top: 15px;
              font-size:15px;
              font-family:Microsoft YaHei;
              font-weight:400;
              color:rgba(153,153,153,1);
              line-height:24px;

            }
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
    color: #2572B5;
    /*border-bottom: 2px solid #2572B5;*/
  }

</style>
