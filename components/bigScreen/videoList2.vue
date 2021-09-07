<template>
  <div>
    <video id="video" ref="video" controls width="100%"></video>
  </div>
</template>

<script>

import {mapState} from "vuex";
export default {
  layout:'index',
  props:['code'],
  data(){
    return{
      url:'',
    }
  },
  computed: {

  },
  components: {
  },
  watch:{
    code:{
      handler:function () {
        if(this.code){
          this.getUrl()
        }
      },
      immediate:true
    }
  },
  mounted(){

  },
  methods:{
    //获取地址
    getUrl(){
      this.api.getVideoUrl(this.code).then(res=>{
        this.url = res
        console.log(this.url)
        this.getStream(this.url)
      })
    },
    //视频开启
    getStream(source) {
      if (Hls.isSupported()) {
        // this.nm = this.videList[this.currentIndex].cameraNm

        this.hls = new Hls();
        this.hls.loadSource(source);
        this.hls.attachMedia(this.$refs.video);
        this.hls.on(Hls.Events.MANIFEST_PARSED, () => {
          console.log('加载成功');
          this.$refs.video.play();
        });
        this.hls.on(Hls.Events.ERROR, (event, data) => {
          // console.log(event, data);
          // 监听出错事件
          console.log('加载失败');
          console.log(event)
          console.log(data)
        });
      }
    },
  }
}
</script>
<style lang="less" scoped>
  #videoList{
    height: 100%;
  }
</style>

