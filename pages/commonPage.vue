<template>
  <div id="commonPage" :style="{width:bWidth+'px'}">
     <div class="commonMain main" :style="{width:width+'px'}">
       <h4>{{info.nm}}</h4>
        <div v-html="info.cont"></div>
       <div id="map" v-if="info.rmks"></div>
     </div>
  </div>
</template>

<script>

import {mapState} from "vuex";
export default {
  data(){
    return{
     id:'',
      mapId:'5142826823865344',
      info:{},
      pointX:'',
      pointY:'',
    }
  },
  computed: {
    ...mapState([
      'bWidth',
      'width'
    ])
  },
  components: {
  },
  mounted(){

    this.getData()

  },
  methods:{
    async getData(){
      this.id = this.until.getQueryString('id')
      this.info = await this.api.ruleDetail(this.id)
      console.log(this.info.cont)
      this.info.cont = this.info.cont.replace(/<img[^>]*>/gi, function (match, capture) {
        return match.replace(/style\s*?=\s*?([‘"])[\s\S]*?\1/ig, 'style="max-width:100%;height:auto;width:auto;"') // 替换style

      })
      if(this.info.rmks){
        this.pointY = this.info.rmks.split(',')[1]
        this.pointX = this.info.rmks.split(',')[0]
        console.log(this.pointX)
        console.log(this.pointY)
        this.$nextTick(()=>{
          this.setMap()

        })
      }
    },
    setMap(){
      // 百度地图API功能
      var map = new BMapGL.Map("map");

      map.centerAndZoom(new BMapGL.Point(this.pointX,this.pointY),11);

      map.enableScrollWheelZoom(true);
      map.clearOverlays();
      var new_point = new BMapGL.Point(this.pointX,this.pointY);
      var marker = new BMapGL.Marker(new_point);  // 创建标注
      map.addOverlay(marker);              // 将标注添加到地图中
      map.panTo(new_point);
    }
  }
}
</script>
<style lang="less" scoped>
  #map{
    height: 600px;
  }
  #commonPage{
    .commonMain{
      background: #ffffff;
      margin: 20px auto;
      padding: 20px;
      box-sizing: border-box;
      min-height: 250px;
      line-height: 26px;
      h4{
        text-align: center;
        padding: 10px 0 30px;
        font-size: 16px;
      }
    }
  }
</style>

