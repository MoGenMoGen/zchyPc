<template>
  <div id="featured">
    <div class="mainView2">
      <div class="productQ2" v-for="(item,index) in featuredList" :key="index"
           :style="{height:width*0.196*0.827+150+'px'}"
           :class="{seen:item.isSeen}"
           @click="toProDetail(item.id)"
           @mouseenter="enter(index)"
           @mouseleave="leave(index)">
        <div class="img" :style="{height:width*0.196*0.827+'px'}">
          <img :src="item.fmImgUrl" :alt="item.nm"/>
        </div>
        <div class="overall2">


          <el-tooltip  effect="light" :content="item.nm" placement="top">
            <p class="child1">{{item.nm}}</p>
          </el-tooltip>

          <div class="space2">
            <p class="child2" v-if="item.goodsMinPrice!=price && item.goodsMinPrice !== item.goodsMaxPrice">￥<span class="child3">{{item.goodsMinPrice}}-{{item.goodsMaxPrice}}</span></p>
            <p class="child2" v-if="item.goodsMinPrice!=price && item.goodsMinPrice === item.goodsMaxPrice">￥<span class="child3">{{item.goodsMinPrice}}</span></p>
            <p class="child2" v-if="item.goodsMinPrice==price">价格面议</p>
            <!--<button>满减</button>-->
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
    import {mapState} from 'vuex'
    export default {
        name: "featured",
      props:['list','type'],
      data(){
        return{
          featuredList:[],
        }
      },
      computed:{
        ...mapState([
          'width',
          'prosize',
          'price'
        ])
      },
      watch:{
        list:{
          handler:function () {
            if(this.list){
              this.featuredList = JSON.parse(JSON.stringify(this.list))
              // console.log(this.featuredList)
              this.featuredList.forEach((item,index)=>{
                if(item.origPrice){
                  item.price1 =(item.origPrice+'').split('.')[0]
                  item.price2 = (item.origPrice+'').split('.')[1] ? (item.origPrice+'').split('.')[1] : ''
                }else {
                  item.price1 = 0
                }
                this.$set(this.featuredList,index,item)
              })
            }

          },
          deep:true,
          immediate:true
        }
      },
      methods:{
        //跳转产品详情
        toProDetail(id){
          this.until.href('/Trade/productDetail?id='+id+'&type='+this.type)
          // this.$router.push({
          //   path:'../Trade/productDetail',
          //   query:{
          //     id:id,
          //     type:this.type
          //   }
          // })
        },
        enter(index){
          this.featuredList[index].isSeen = true
          this.$set(this.featuredList,index,this.featuredList[index])
        },
        leave(index){
          this.featuredList[index].isSeen = false
          this.$set(this.featuredList,index,this.featuredList[index])
        }
      }
    }
</script>

<style lang="less" scoped>
  @import url("../../assets/css/init.less");
#app{

}
.mainView2{
  display: flex;
  width: 100%;
  background-color: #FFFFFF;
  margin-top: 10px;
  flex-wrap: wrap;
  padding: 0 3px;
  .seen{
    border: 1px solid #ECECEC;
    box-shadow:0 0 7px #ECECEC;
  }
//商品具体的div
  .productQ2{
    display: flex;
    flex-direction: column;
    width: 19.6%;
    background-color: #FFFFFF;
    margin-bottom: 10px;
    cursor: pointer;
    margin-right:0.5% ;
    &:nth-of-type(5n+5){
      margin-right: 0;
    }
    .img{
      width: 82.7%;
      margin: 20px auto;
      display: flex;
      display: -webkit-flex;
      >img{
        margin: auto;
      }
    }
    .overall2{
      display: flex;
      flex-wrap: wrap;
      justify-content: flex-start;
      text-align: left;
      width: 190px;
      margin: 0 auto;
      >p{
        padding-bottom: 20px;
        color: #666666;

        overflow: hidden;
        text-overflow:ellipsis;
        white-space: nowrap;


      }
      .space2{
        display: inline-flex;
        justify-content: space-between;
        width: 100%;
      //￥符号
        .child2{
          color: #FF3E3E;
        }
      //小数点前数字
        .child3{
          color: #FF3E3E;
          font-size: 20px;
        }
      //小数点后
        .child4{
          color: #FF3E3E;
          font-size: 18px;
        }
        button{
          width: 42px;
          height: 20px;
          border: 1px solid #FF3E3E;
          color: #FF3E3E;
          background-color: #FFFFFF;
          border-radius: 2px;
          font-size: 14px;
        }
      }
    }
  }
}
</style>
