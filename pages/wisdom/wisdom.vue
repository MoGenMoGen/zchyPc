<template>
<div id="home">
  <el-carousel :height="bannerHeight+'px'" class="adert">
    <el-carousel-item v-for="item in adertList" :key="item.aiid">
      <div class="bannerImg" :style="{backgroundImage:'url('+item.imgUrl+')'}" @click="toPage(item.locUrl)"/>
    </el-carousel-item>
  </el-carousel>
  <div class="adertBox main" :style="{width:width+'px'}">
  <div class="adertNav point"
       v-for="(item,index) in adertList2"
       @mouseenter="changeLiTrans(index,1,1)"
       @mouseleave="changeLiTrans(index,0,1)"
       @click="toPage(item.locUrl)">
    <el-image
      style="width: 100%;height: 100%"
      :class="{toScale:item.ifShow}"
      :src="item.imgUrl"
      fit="cover"></el-image>
    <p><span>——</span> <span class="textS">{{item.nm}}</span> <span>——</span></p>
  </div>
</div>
  <div class="adertBox main" :style="{width:width+'px'}">
    <div class="adertNav point"
         v-for="(item,index) in adertList3"
         @mouseenter="changeLiTrans(index,1,2)"
         @mouseleave="changeLiTrans(index,0,2)"
         @click="toPage(item.locUrl)">
      <el-image
        style="width: 100%;height: 100%"
        :class="{toScale:item.ifShow}"
        :src="item.imgUrl"
        fit="cover"></el-image>
      <p><span>——</span> <span class="textS">{{item.nm}}</span> <span>——</span></p>
    </div>
  </div>
</div>
</template>

<script>
  import {mapState} from "vuex";
    export default {
        name: "wisdom",
      data(){
          return{
            bannerHeight:0,
            adertList:[],
            adertList2:[],
            adertList3:[],
          }
      },
      computed: {
        ...mapState([
          'bWidth',
          'width',
          'currentRole'
        ])
      },
      watch:{

      },
      async asyncData(context){
        let arr = await Promise.all([
          context.app.api.getAdert('ZHCBAdert'),
          context.app.api.getAdert('ZHCB'),
        ])
        return {
          arr:arr
        }
      },
      mounted(){
        this.getData()
      },
      methods:{
        async getData(){
          this.adertList = await this.api.getAdert('ZHCBAdert')
          if(this.adertList.length>0){
            let myImg = new Image()
            myImg.src = this.adertList[0].imgUrl
            myImg.onload = ()=>{
              this.bannerHeight = myImg.height
            }
          }
          let list = await this.api.getAdert('ZHCB')
          this.adertList2 = list.splice(0,3)
          this.adertList3 = list
        } ,
        //鼠标滑动效果  索引，类型（1滑入，0滑出）
        changeLiTrans(index,type,nm){
          if(nm==1){
            this.adertList2[index].ifShow = type ? true : false
            this.$set(this.adertList2, index, this.adertList2[index])
          }
          if(nm==2){
            this.adertList3[index].ifShow = type ? true : false
            this.$set(this.adertList3,index,this.adertList3[index])
          }
        },
        toPage(url){
          if(url){
            window.location.href = url
          }
        }
      },

    }
</script>

<style lang="less" scoped>
  @import url("../../assets/css/init.less");
#home{
  background-image: url("../../assets/img/footer.png");
  background-position: center bottom;
  background-repeat: repeat-x;
  padding-bottom: 80px;
  .point{
    cursor: pointer;
  }
  .adert{
    width: 100%;
    .bannerImg{
      width: 100%;
      height: 100%;
      background-position: center;
    }
  }
  .adertBox{
    display: flex;
    justify-content: space-between;
    margin:10px auto;
    .adertNav{
      position: relative;
      overflow: hidden;
      /*display: inline-block;*/
      margin-right: 10px;
      >img{
        width: 100%;
        height: 100%;
        max-width: 200%;
        max-height: 200%;
      }

      >p{
        width: 100%;
        background-color: rgba(0,0,0,0.35);
        position: absolute;
        padding: 8px 0;
        bottom: 0;
        color:rgba(255,255,255,1);
        text-align: center;
        .textS{
          margin: 0 10px;
          opacity: 1;
          font-size:20px;
        }
        >span{
          opacity: 0.2;
        }
      }
    }
    .adertNav:last-child{
      margin-right: 0;
    }
    .cloumnBox{
      display: flex;
      flex-direction: column;
      justify-content: space-between;
    }
  }
}
</style>
