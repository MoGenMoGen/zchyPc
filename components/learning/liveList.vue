<template>
  <div id="liveList">
    <div class="part" v-for="(v,i) in list" :key="i" :style="{height:width*0.186*1.1 + 'px'}">
      <img class="ad" :alt="v.name" :src="v.coverImg" @click="livePlay(v)"/>
      <p class="child1">{{v.name}}</p>
      <p class="child2"><img src="@/assets/img/learning/人物.png" />{{v.anchorName }}</p>
      <button @click="livePlay(v)">立即观看</button>
      <img class="ok" src="@/assets/img/learning/播放.png" @click="livePlay(v)" v-if="v.liveStatus == 3"/>
      <img class="open" src="@/assets/img/learning/直播中.png" @click="livePlay(v)" v-if="v.liveStatus == 2"/>
      <img class="open" src="@/assets/img/learning/未开始.png" @click="livePlay(v)" v-if="v.liveStatus == 1"/>
    </div>
  </div>
</template>
<script>
  import {mapState} from "vuex";
  export default {
    props:['list'],
    data(){
      return{

      }
    },
    components: {
    },
    watch:{

    },
    computed: {
      ...mapState([
        'bWidth',
        'width',
        'ifLogin'
      ])
    },
    mounted(){
        // this.getProvice()
    },
    methods:{
//在线学习播放直播
      livePlay(e){
        console.log('=====play')
        if(!this.ifLogin){
          this.$confirm('仅登录会员查看，立即登录?', '提示', {
            confirmButtonText: '确定',
            cancelButtonText: '取消',
            type: 'warning'
          }).then(() => {
            location.href = './login/login'
          }).catch(() => {

          });
        }else {
          location.href = "./learning/videoDetail?id="+e.id
        }

      },
    }
  }
</script>
<style lang="less">
  @import url("../../assets/css/init.less");
  #liveList{
    width: 100%;
    overflow: hidden;
    .part{
      width: 18.6%;
      margin-right: 1.75%;
      background-color: #F1F2F4;
      margin-bottom: 30px;
      border-radius: 10px;
      position: relative;
      float: left;
      .ok{
        position: absolute;
        max-height: 50px;
        max-width: 40px;
        top: 55px;
        left: 90px;
        opacity:0.55;
        cursor: pointer;
      }
      .open{
        max-width: 79px;
        opacity:0.75;
        position: absolute;
        left: 10px;
        top: 10px;
      }
      >button{
        color: #FFFFFF;
        background-color: #2778BE;
        font-size: 12px;
        display: flex;
        align-items: center;
        justify-content: center;
        width: 70px;
        height: 26px;
        border: none;
        position: absolute;
        right: 10px;
        bottom: 12px;
        cursor: pointer;
        border-radius: 4px;
      }
      .child1{
        font-size: 16px;
        color: #333333;
        padding:11px 11px 0 11px;
        line-height: 30px;
        overflow: hidden;
        text-overflow:ellipsis;
        white-space: nowrap;
      }
      .child2{
        padding-left: 11px;
        margin-top: 17px;
        font-size: 13px;
        color: #999999;
        overflow: hidden;
        text-overflow:ellipsis;
        white-space: nowrap;
        >img{
          max-width: 11px;
          max-height: 13px;
          margin-right: 10px;
        }
      }
      .ad{
        width: 100%;
        height: 60%;
        border-radius: 10px 10px 0 0;
      }
      &:nth-of-type(5n){
        margin-right: 0;
      }
    }
  }
</style>

