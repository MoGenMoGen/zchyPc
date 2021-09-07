<template>
  <ul class="main scaleList" :style="{width:width+'px'}" >
    <li v-for="(item,index) in myList" @mouseenter="changeLiTrans(index,1)" @mouseleave="changeLiTrans(index,0)"  @click="toPage(item.href)">
      <div class="transContent toScale" v-show="item.ifTrans" :style="{width:width*0.25+8+'px',background:item.color}">
        <p><img :src="item.imgH"/>
          {{item.nm}}</p>
        {{item.cont}}
      </div>
      <div class="showContent" >
        <img :src="item.img"/>
        <p>{{item.nm}}</p>
      </div>
    </li>
  </ul>
</template>

<script>

import {mapState} from "vuex";
export default {
  props:['list'],
  data(){
    return{
      myList:[]
    }
  },
  computed: {
    ...mapState([
      'bWidth',
      'width'
    ])
  },
  watch:{
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
    toPage(url){
        this.$router.push(url)
    }
  }
}
</script>

<style lang="less" scoped>
  @import url("../assets/css/init.less");
    .scaleList{
      box-sizing: border-box;
      border-left: 1px solid rgba(0,0,0,0.2);
      border-top: 1px solid rgba(0,0,0,0.2);
      margin-bottom: 20px;
      li{
        border-right: 1px solid rgba(0,0,0,0.2);
        border-bottom: 1px solid rgba(0,0,0,0.2);
        box-sizing: border-box;
        width: 25%;
        display: inline-block;
        height: 300px;
        cursor: pointer;
        position: relative;
        font-size: 16px;
        .showContent{
          width: 100%;
          height: 66%;
          display: flex;
          display: -webkit-flex;
          align-items: center;
          justify-content: center;
          flex-direction: column;
          img{
            width: 55px;
            height: auto;
            margin-bottom: 15px;
          }
          p{
            text-align: center;
          }
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
