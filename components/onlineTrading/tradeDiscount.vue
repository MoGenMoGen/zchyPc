<template>
    <div class="home">
      <div class="specific">
        <div class="leftBanner">
          <img src="../../assets/img/homeBX.png" alt="左边大图"/>
          <p>金属管接头</p>
          <p class="textRed">￥55.9</p>
        </div>
        <div class="rightBanner">
          <div class="productMain">
            <div class="leftPart">
              <img class="pImg" src="../../assets/img/homeBX.png" alt=""/>
            </div>
            <div class="rightPart">
              <p>中山耐腐金属软管</p>
              <p class="textRed">￥8.9</p>
              <p>热销中</p>
            </div>
          </div>
          <div class="productMain">
            <div class="leftPart">
              <img class="pImg" src="../../assets/img/homeBX.png" alt=""/>
            </div>
            <div class="rightPart">
              <p>金属板</p>
              <p class="textRed">￥158.9</p>
              <p>热销中</p>
            </div>
          </div>
          <div class="productMain">
            <div class="leftPart">
              <img class="pImg" src="../../assets/img/homeBX.png" alt=""/>
            </div>
            <div class="rightPart">
              <p>双金属片型蒸汽疏水阀</p>
              <p class="textRed">￥15.9</p>
              <p class="flexBottom">热销中</p>
            </div>
          </div>
          <div class="productMain">
            <div class="leftPart">
              <img class="pImg" src="../../assets/img/homeBX.png" alt="右一"/>
            </div>
            <div class="rightPart">
              <p>电接点双金属温度计</p>
              <p class="textRed">￥8.9</p>
              <p>热销中</p>
            </div>
          </div>
        </div>
      </div>
    </div>
</template>

<script>
    export default {
        name: "tradeDiscount",
      props:[],
      data(){
        return{
        }
      },
    }
</script>

<style lang="less" scoped>
  @import url("../../assets/css/init.less");
  .home{
    height: 100%;
    width: 100%;
    .specific{
      display: inline-flex;
      flex-wrap: nowrap;
      width: 100%;
    //左部大图
      .leftBanner{
        display: flex;
        flex-wrap: wrap;
        justify-content: space-around;
        width: 33.33%;
        cursor: pointer;

        >img{
          height: 170px;
          width: 170px;
          border: 1px solid #F3F3F3;
        }
        >p{
          width: 100%;
          text-align: center;
          font-size: 14px;
          display: -webkit-box;
          -webkit-box-orient: vertical;
          overflow: hidden;
        }
      //红色字体
        .textRed{
          color: #FF3E3E;
        }
      }
    //右部四个小图
      .rightBanner{
        width: 66.6%;
        display: flex;
        flex-wrap: wrap;
        cursor: pointer;

      //具体商品栏
        .productMain{
          width: 50%;
          display: flex;
        //商品栏左部分
          .leftPart{
            >img{
              width: 85px;
              height: 85px;
              border: 1px solid #F3F3F3;
            }
          }
        //商品栏右部分
          .rightPart{
            display: flex;
            justify-content: space-between;
            flex-direction: column;
            padding-left: 10px;
            padding-bottom: 15px;
            >p{
              width: 84px;
              color: #666666;
              padding-bottom: 12px;
              display: -webkit-box;
            }
            .textRed{
              color: #FF3E3E;
            }
          }
        }
      }
    }
  }


</style>
