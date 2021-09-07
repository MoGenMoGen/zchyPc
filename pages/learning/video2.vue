<template>
    <div class="p-padding">
      <video class="full-height full-width" ref="video" controls v-if="!ifMp4"></video>
      <video class="full-height full-width" :src="url" controls v-else></video>
      <!--<video-player class="video-player vjs-custom-skin"-->
                    <!--ref="videoPlayer"-->
                    <!--:playsinline="true"-->
                    <!--:options="playerOptions"-->
      <!--&gt;</video-player>-->
    </div>
</template>

<script>

  export default {
    name: '',
    components: {

    },
    data () {
      return {
        hls:null,
        url:'',
        ifMp4:false,
      }
    },
    async mounted () {
        let id = this.until.getQueryString('id')
      let url = ''

      if(id){
          let data = await this.api.livePlay(id)
          let info = data.data

          if(info.liveStatus<3){
            url = info.m3u8Url

          }else {
            url = info.storedUrl || info.m3u8Url
            // url = info.flvUrl
          }
          let imgUrl = info.cover2Img
        }else {
          url = decodeURIComponent(this.until.getQueryString('url'))

      }
      console.log(url)

      this.url = url
      this.ifMp4 = this.url.toLowerCase().indexOf('.mp4')==-1 ? false : true
      if(!this.ifMp4){
        this.getStream(this.url)
      }

    },
    methods: {
      getStream(source) {
        if (Hls.isSupported()) {
          this.hls = new Hls();
          console.log(source)
          this.hls.loadSource(source);
          this.hls.attachMedia(this.$refs.video);
          console.log('===========')
          console.log(this.hls.on)
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
      videoPause() {
        if (this.hls) {
          this.$refs.video.pause();
          this.hls.destroy();
          this.hls = null;
        }
      },
    },
    beforeDestroy() {
      this.videoPause();
    }
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
@import url( 'https://g.alicdn.com/de/prismplayer/2.8.2/skins/default/aliplayer-min.css');
  .p-padding{
    text-align: center;
    padding: 20px 0;
  }
video{
  width: 80%;
  margin: 0 auto;
}
</style>
