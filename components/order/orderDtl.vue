<template>
<!--  订单详情组件-->
  <div style="width: 100%">
    <div class="line2">
      <div class="line2-l">
        <p >订单号：159856856655</p>

        <p class="red">待发货</p>
        <p>取消订单</p>

      </div>
      <div class="line2-r">
        <!--            <el-steps :active="1">-->
        <!--              <el-step title="提交订单" icon="el-icon-edit" description="2020-06-28 12:00:25"></el-step>-->
        <!--              <el-step title="付款成功" icon="el-icon-upload" description="2020-06-28 12:00:25"></el-step>-->
        <!--              <el-step title="待收货" icon="el-icon-picture" description="2020-06-28 12:00:25"></el-step>-->
        <!--              <el-step title="交易完成" icon="el-icon-picture" description="2020-06-28 12:00:25"></el-step>-->
        <!--            </el-steps>-->
        <steps></steps>
      </div>


    </div>
    <!--        商品信息-->
    <div class="line3">
      <div class="line3-l">
        <img src="https://fuss10.elemecdn.com/8/27/f01c15bb73e1ef3793e64e6b7bbccjpeg.jpeg">
        <p>水下推进器 LEFEET S1R 工具潜水器工具潜水器</p>
      </div>
      <div class="line3-r">
        <el-timeline >
          <el-timeline-item
            v-for="(activity, index) in activities"
            :key="index"
            :timestamp="activity.timestamp">
            {{activity.content}}
          </el-timeline-item>
        </el-timeline>




      </div>
    </div>
    <!--        收件人信息-->
    <div class="line4">
      <div>
        <p>收件人信息</p>
        <p><span>收货人：</span><span>王小二</span></p>
        <p><span>收货地址：</span><span>浙江省宁波市镇海区329创业社区211-212室</span></p>
        <p><span>手机号码：</span><span>188****8888</span></p>
      </div>
      <div>
        <p>配送信息</p>
        <p><span>配送方式：</span><span>普通快递</span></p>
        <p><span>运费：</span><span>￥0.00</span></p>
      </div>
      <div>
        <p>付款信息</p>
        <p><span>付款方式：</span><span>在线支付</span></p>
        <p><span>付款时间：</span><span>2020-06-18 12:00:20</span></p>
        <p><span>商品总额：</span><span>￥10.00</span></p>
      </div>
    </div>


  </div>

</template>

<script>
    import steps from "../steps/steps";
    export default {
        name: "orderDtl",
        components:{
            steps,
        },
        data(){
            return{
                activities: [{
                    content: '活动按期开始活动按期开始666666666666666666666666666666666',
                    timestamp: '2018-04-15'
                }, {
                    content: '通过审核',
                    timestamp: '2018-04-13'
                }, {
                    content: '创建成功',
                    timestamp: '2018-04-11'
                }],
                goodList:[
                    {
                        img:'https://fuss10.elemecdn.com/8/27/f01c15bb73e1ef3793e64e6b7bbccjpeg.jpeg',
                        nm:'商品名称商品名称商品名称商品名称商品名称',
                        rule:'颜色：红色，规格：s',
                        price:800,
                        count:1,
                        total:800,
                    },
                    {
                        img:'https://fuss10.elemecdn.com/8/27/f01c15bb73e1ef3793e64e6b7bbccjpeg.jpeg',
                        nm:'商品名称商品名称商品名称商品名称商品名称',
                        rule:'颜色：红色，规格：s',
                        price:800,
                        count:1,
                        total:800,
                    },
                    {
                        img:'https://fuss10.elemecdn.com/8/27/f01c15bb73e1ef3793e64e6b7bbccjpeg.jpeg',
                        nm:'商品名称商品名称商品名称商品名称商品名称',
                        rule:'颜色：红色，规格：s',
                        price:800,
                        count:1,
                        total:800,
                    }
                ]
            }
        }
    }
</script>

<style scoped lang="less">


  .line2{
    width: 100%;
    display: flex;
    height: 198px;
    background: white;
    .line2-l{
      width: 325px;
      height: 100%;
      border-right: 1px solid #F1F1F1;
      p{
        margin: 0 auto;
        text-align: center;
        width: fit-content;

        &:nth-child(1){
          color: #999999;
          margin-top: 30px;

        }
        &:nth-child(2){
          font-size: 28px;
          margin-top: 37px;

        }
        &:nth-child(3){
          font-size: 14px;
          color: #666666;
          margin-top: 30px;
          border-radius: 3px;
          border: 1px solid #DDDDDD;
          padding:9px 22px;
          background: #F5F5F5;

        }
      }
    }
    .line2-r{
      width: 874px;
      height: 100%;
      padding:54px 1px 54px 84px;


    }
  }

  .line3{
    margin-top: 20px;
    width: 100%;
    background: white;
    display: flex;
    .line3-l{
      display: flex;
      padding:31px;
      width: 325px;
      border-right: 1px solid #F1F1F1;
      img{
        width: 110px;
        height: 110px;
        margin-right: 20px;
      }
      p{
        margin-top: 13px;

      }
    }
    .line3-r{
      width: 874px;
      display: flex;
      align-items: center;
      padding: 30px 80px;
    }

  }
  .line4{
    margin-top: 20px;
    width: 100%;
    padding: 30px 0;
    display: flex;
    background: white;
    div{
      padding: 0 30px;
      flex:1;
      border-right: 1px solid #F1F1F1;
      /*标题*/
      p:first-of-type{
        font-size: 16px;
        color: #303030;
        font-weight: bold;
        margin-bottom: 24px;
      }
      p:not(first-of-type){
        display: flex;
        margin-bottom: 20px;
        span:nth-of-type(1){
          flex: 1;
        }
        span:nth-of-type(2){
          flex:3;
        }
      }
      p:last-of-type{
        margin-bottom: 0;
      }
      &:last-child{
        border-right: none;
      }
    }

  }



</style>
