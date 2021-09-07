<template>
    <div class="p-padding">
      <!--阿里云-->
      <div  class="prism-player" id="J_prismPlayer"></div>
      <!--{{url}}-->
      <!--<div class="video-player-box"-->
           <!--:playsinline="playsinline"-->
           <!--@play="onPlayerPlay($event)"-->
           <!--@pause="onPlayerPause($event)"-->
           <!--@ended="onPlayerEnded($event)"-->
           <!--@loadeddata="onPlayerLoadeddata($event)"-->
           <!--@waiting="onPlayerWaiting($event)"-->
           <!--@playing="onPlayerPlaying($event)"-->
           <!--@timeupdate="onPlayerTimeupdate($event)"-->
           <!--@canplay="onPlayerCanplay($event)"-->
           <!--@canplaythrough="onPlayerCanplaythrough($event)"-->
           <!--@ready="playerReadied"-->
           <!--@statechanged="playerStateChanged($event)"-->
           <!--v-video-player:myVideoPlayer="playerOptions">-->
      <!--</div>-->

    </div>
</template>

<script>

  export default {
    name: '',
    components: {

    },
    data () {
      return {

        playsinline: true,
        playerOptions: {
          muted: true,
          controls: true,
          language: 'lang',
          playbackRates: [0.7, 1.0, 1.5, 2.0],  //播放速率
          sources: [
            {
              type: 'application/x-mpegURL',
              src: 'https://videos8.jsyunbf.com/20190717/s6DaVnKb/index.m3u8'  //视频流地址
            }
          ],
          hls: true,  //启用hls？
          fluid: true,  //设置播放器为流体  宽度为外层盒子大小  ps：想设置width：100%找不到方法？这个就对了
          poster: "/static/images/author.jpg",
        },

        url:'',
        msg: 'Welcome to Your Vue.js App',
        filterText:'',
        treeKey:'',
        defaultProps: {// 可把label，children转换成自己想要的名称，如lable：name
          children: 'children',
          label: 'label'
        },
      }
    },
    async mounted () {
        let id = this.until.getQueryString('id')
        let data = await this.api.livePlay(id)
        let info = data.data

      let url = ''
      if(info.liveStatus<3){
         url = info.flvUrl

      }else {
          url = info.storedUrl
      }
      let imgUrl = info.cover2Img
      this.url = url
      // videojs("my-video",
      //   function() {
      //     this.play();
      //   });
      // 阿里云初始化播放器
      var player = new Aliplayer({
        id: "J_prismPlayer", // 容器id
        // source:"http://cloud.video.taobao.com/play/u/2554695624/p/1/e/6/t/1/fv/102/28552077.mp4",
        // source:"http://kbs-dokdo.gscdn.com/dokdo_300/_definst_/dokdo_300.stream/playlist.m3u8",
        source: url,  //视频地址
        cover: imgUrl,  //播放器封面图
        autoplay: false,      // 是否自动播放
        width: "100%",       // 播放器宽度
        height: "600px",      // 播放器高度
        playsinline: true,
        seekable: true,
        skinLayout: [{
          "name": "bigPlayButton",
          "align": "cc"
          //"x": 30,
          //"y": 80
        }, {
          "align": "blabs",
          "x": 0,
          "y": 0,
          "name": "controlBar",
          "children": [
            {
              "align": "tl",
              "x": 15,
              "y": 26,
              "name": "playButton"
            }, {
              "align": "tl",
              "x": 10,
              "y": 24,
              "name": "timeDisplay"
            }, {
              "align": "tr",
              "x": 20,
              "y": 25,
              "name": "fullScreenButton"
            }, {
              "align": "tr",
              "x": 20,
              "y": 25,
              "name": "volume"
            },
            {
              "name": "progress",
              "align": "tlabs",
              "x": 0,
              "y": 0
            }
          ]
        }]
      })
    },
    methods: {
      // listen event
      onPlayerPlay(player) {
        // console.log('player play!', player)
      },
      onPlayerPause(player) {
        // console.log('player pause!', player)
      },
      onPlayerEnded(player) {
        // console.log('player ended!', player)
      },
      onPlayerLoadeddata(player) {
        // console.log('player Loadeddata!', player)
      },
      onPlayerWaiting(player) {
        // console.log('player Waiting!', player)
      },
      onPlayerPlaying(player) {
        // console.log('player Playing!', player)
      },
      onPlayerTimeupdate(player) {
        // console.log('player Timeupdate!', player.currentTime())
      },
      onPlayerCanplay(player) {
        // console.log('player Canplay!', player)
      },
      onPlayerCanplaythrough(player) {
        // console.log('player Canplaythrough!', player)
      },
      // or listen state event
      playerStateChanged(playerCurrentState) {
        // console.log(playerCurrentState)
      },
      // player is ready
      playerReadied(player) {
        // console.log('example 01: the player is readied', player)
      },
      filterNode (value, data) {
        if (!value) return true;
        return data.label.indexOf(value) !== -1;
      },
      handleNodeClick (data, node, obj) {
        if (data.id) {
          this.treeId = data.id;
          this.treeDataObj = data;
          this.nodeDataTree = node;
          console.log(data);
        }
      }
    }
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
@import url( 'https://g.alicdn.com/de/prismplayer/2.8.2/skins/default/aliplayer-min.css');
</style>
