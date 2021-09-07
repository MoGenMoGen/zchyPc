<template>
  <ul class="main scaleList" :style="{width:width+'px'}" >
    <li v-for="(item,index) in myList" @click="toPage(item.cd)" :style="{width:liWidth+'px',height:liWidth*2/3+'px'}" @mouseenter="changeLiTrans(index,1)" @mouseleave="changeLiTrans(index,0)">
      <div class="showContent" :class="{active:item.ifTrans}">
          <img :src="item.arg1"/>
          <p>{{item.nm}}</p>
      </div>
    </li>
  </ul>
<!--  <ul class="main scaleList" :style="{width:width+'px'}" >-->
<!--    <li v-for="(item,index) in myList" @click="toPage(item.cd)" @mouseenter="changeLiTrans(index,1)" @mouseleave="changeLiTrans(index,0)" :style="{width:liWidth+'px',height:liWidth+'px'}">-->
<!--      <div class="transContent toScale" v-show="item.ifTrans" :style="{width:width*0.25+8+'px',background:'rgba('+item.arg3+',1)',boxShadow:'0px 5px 13px 0px rgba('+item.arg3+',0.48)'}">-->
<!--        <p><img :src="item.arg2"/>-->
<!--          {{item.nm}}</p>-->

<!--        {{item.arg4}}-->
<!--      </div>-->
<!--      <div class="showContent" >-->
<!--        <img :src="item.arg1"/>-->
<!--        <p>{{item.nm}}</p>-->
<!--      </div>-->
<!--    </li>-->
<!--  </ul>-->
</template>

<script>

import {mapState} from "vuex";
export default {
  props:['list'],
  data(){
    return{
      myList:[],
      liWidth:100,
    }
  },
  computed: {
    ...mapState([
      'bWidth',
      'width'
    ])
  },

  watch:{
    width:{
        handler:function(){
           this.liWidth = (this.width-20*4)/5
        },
        immediate:true
    },
    list:{
      handler:function () {
        this.myList = JSON.parse(JSON.stringify(this.list))
      },
      immediate:true
    }
  },
  components: {
  },
  mounted(){



  },
  methods:{

    //鼠标滑动切换放大效果
    changeLiTrans(index,type){
      this.myList[index].ifTrans = type ? true : false
      this.$set(this.myList,index,this.myList[index])
    },

    //页面跳转
    toPage(cd){
        this.$router.push('../settleIn/settleIn?type='+cd)
    }
  }
}
</script>

<style lang="less" scoped>
  @import url("../assets/css/init.less");
    .scaleList{
      box-sizing: border-box;
      margin-bottom: 20px;
      li{
        box-shadow:0px 0px 5px 0px rgba(13,4,8,0.10);
        box-sizing: border-box;
        margin-right: 20px;
        display: inline-block;
        cursor: pointer;
        position: relative;
        font-size: 16px;
        margin-bottom: 20px;
        &:nth-of-type(5n){
          margin-right: 0;
        }
        .showContent{
          width: 100%;
          height: 100%;
          display: flex;
          display: -webkit-flex;
          align-items: center;
          justify-content: center;
          flex-direction: column;
          img{
            width: 40px;
            height: auto;
            margin-bottom: 15px;
          }
          p{
            text-align: center;
            font-size: 15px;
            margin-top: 5px;
          }
        }
        .active{
          box-shadow:0px 5px 13px 0px rgba(86,86,86,0.33);

        }
        .transContent{
          width:100%;
          height:300px;
          box-shadow:0px 5px 13px 0px rgba(86,86,86,0.33);
          position: absolute;
          top:-1px;
          left: -1px;
          padding: 61px 21px 21px;
          box-sizing: border-box;
          color: rgba(255,255,255,0.7);
          font-size: 12px;
          z-index: 9;

          p{
            display: flex;
            display: -webkit-flex;
            align-items: center;
            justify-content: center;
            margin-bottom: 30px;
            font-size: 16px;
            color: rgba(255,255,255,1);
            img{
              width: 30px;
              margin-right: 8px;
            }
          }
        }

      }
    }


</style>
