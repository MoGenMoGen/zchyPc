<template>
  <div id="divPlugin"  ref="divPlugin" v-if="plugin">    </div>
</template>

<script>

import {mapState} from "vuex";
export default {
  layout:'index',
  props:['code'],
  data(){
    return{
      plugin:true ,
      oWebControl:null,   //插件对象
      initCount:0,
      pubKey:'',         //公钥加密
      cameraIndexCode:'908ca5b29cb4400b945790b6270d0753',
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
          this.initVideo()
        }
      },
      immediate:true
    }
  },
  mounted(){
//插件初始化
    this.initPlugin();
  },
  methods:{
    //关闭插件
    off(){
      let _this = this;
      _this.oWebControl.JS_HideWnd();
      _this.oWebControl.JS_Disconnect();
      // _this.oWebControl.JS_DestroyWn();
    },

    //   插件初始化
    initPlugin () {
      let _this = this;
      this.oWebControl = new WebControl({
        szPluginContainer: "divPlugin",
        iServicePortStart: 15900,
        iServicePortEnd: 15909,
        szClassId:"23BF3B0A-2C56-4D97-9C03-0CB103AA8F11",   // 用于IE10使用ActiveX的clsid
        cbConnectSuccess: (e) => {
          this.plugin=true;
          // _this.setCallbacks();
          if(_this.oWebControl){
            _this.oWebControl.JS_StartService("window", {
              dllPath: "./VideoPluginConnect.dll"
            }).then( (res) => {
              _this.oWebControl.JS_CreateWnd("divPlugin",1440,740).then(() => {
                this.initVideo();
              });
            }, function () {
            });
          }else{
          }
        },
        cbConnectError: () => {
          _this.oWebControl = null;
          // $("#divPlugin").html("插件未启动，正在尝试启动，请稍候...");
          WebControl.JS_WakeUp("VideoWebPlugin://");
          _this.initCount ++;
          if (_this.initCount < 3) {
            setTimeout(() => {
              _this.initPlugin();
            }, 3000)
          } else {
            // $("#divPlugin").html("插件启动失败，请检查插件是否安装！");
            this.plugin=false;
          }
        },
        cbConnectClose: () => {
          console.log("cbConnectClose");
          this.oWebControl.JS_Disconnect().then(function(){}, function() {});
          this.oWebControl = null;
        }
      });
    },
    // 设置窗口控制回调
    setCallbacks() {
      this.oWebControl.JS_SetWindowControlCallback({
        cbIntegrationCallBack: this.cbIntegrationCallBack
      });
    },
    // 推送消息
    cbIntegrationCallBack(oData) {
      this.$message.warning((JSON.stringify(oData.responseMsg)));
    },
    // 初始化参数  29479825  EzTLktlCN85iz6ihDVQB  112.14.58.50:81
    // 223.94.51.37:449 28794018 nNibIIgRf1gUXcUP74h1
    initVideo () {
      this.getPubKey(() =>{
        let appkey = "29479825";                                  //综合安防管理平台提供的appkey，必填
        let secret = this.setEncrypt("EzTLktlCN85iz6ihDVQB");   //综合安防管理平台提供的secret，必填
        let ip = "112.14.58.50";                              //综合安防管理平台IP地址，必填
        let port = 81;                                     //综合安防管理平台端口，若启用HTTPS协议，默认443

        // let appkey = "29479825";                                  //综合安防管理平台提供的appkey，必填
        // let secret = this.setEncrypt("EzTLktlCN85iz6ihDVQB");   //综合安防管理平台提供的secret，必填
        // let ip = "112.14.58.50";                              //综合安防管理平台IP地址，必填
        // let port = 81;                      //综合安防管理平台端口，若启用HTTPS协议，默认443
        let playMode = 0;                                  //初始播放模式：0-预览，1-回放


        let snapDir = "D:\\SnapDir";                       //抓图存储路径
        let videoDir = "D:\\VideoDir";                     //紧急录像或录像剪辑存储路径
        let layout = "1x1";                                //playMode指定模式的布局
        let enableHTTPS = 0;                               //是否启用HTTPS协议与综合安防管理平台交互，是为1，否为0
        let encryptedFields = 'secret';					            //加密字段，默认加密领域为secret
        let showToolbar = 1;                               //是否显示工具栏，0-不显示，非0-显示
        let showSmart = 1;                                 //是否显示智能信息（如配置移动侦测后画面上的线框），0-不显示，非0-显示
        let buttonIDs = "0,16,256,257,258,259,260,512,513,514,515,516,517,768,769";  //自定义工具条按钮
        this.oWebControl.JS_RequestInterface({
          funcName: "init",
          argument: JSON.stringify({
            appkey: appkey,                            //API网关提供的appkey
            secret: secret,                            //API网关提供的secret
            ip: ip,                                    //API网关IP地址
            playMode: playMode,                        //播放模式（决定显示预览还是回放界面）
            port: port,                                //端口
            snapDir: snapDir,                          //抓图存储路径
            videoDir: videoDir,                        //紧急录像或录像剪辑存储路径
            layout: layout,                            //布局
            enableHTTPS: enableHTTPS,                  //是否启用HTTPS协议
            encryptedFields: encryptedFields,          //加密字段
            showToolbar: showToolbar,                  //是否显示工具栏
            showSmart: showSmart,                      //是否显示智能信息
            buttonIDs: buttonIDs                       //自定义工具条按钮
          })
        }).then(
          this.startRealPlay()
        );
      })
    },
    // 获取公钥
    getPubKey (callback) {
      this.oWebControl.JS_RequestInterface({
        funcName: "getRSAPubKey",
        argument: JSON.stringify({
          keyLength: 1024
        })
      }).then((oData) => {
        //获取公钥
        if (oData.responseMsg.data) {
          this.pubKey = oData.responseMsg.data;
          callback()
        }
      })
    },
    // RSA加密
    setEncrypt (value) {
      console.log("RSA加密");
      let encrypt = new JSEncrypt();
      encrypt.setPublicKey(this.pubKey);
      return encrypt.encrypt(value);
    },
    // 视频预览
    startRealPlay (IndexCode) {
      console.log(this.cameraIndexCode)
      console.log('------开始播放-------');
      let cameraIndexCode  =  this.cameraIndexCode;            //"ef7431a9b47c43d0a6c26c2037dcb18b";
      let streamMode = 0;                                     //主子码流标识：0-主码流，1-子码流
      let transMode = 1;                                      //传输协议：0-UDP，1-TCP
      let gpuMode = 0;                                        //是否启用GPU硬解，0-不启用，1-启用
      let wndId = -1;                                         //播放窗口序号（在2x2以上布局下可指定播放窗口）
      cameraIndexCode = cameraIndexCode.replace(/(^\s*)/g, "");
      cameraIndexCode = cameraIndexCode.replace(/(\s*$)/g, "");
      let code = ['970610b5c5e84fbbb8a51884de2b753a','3585e0467d1742f3848cf8050e7d606e']
      let list = []
      code.forEach((item,index)=>{
        let code = item.replace(/(^\s*)/g, "")
        list.push({
          cameraIndexCode :  code,
          ezvizDirect: 0,
          recordLocation: 0,
          streamMode: streamMode,
          transMode: transMode,
          gpuMode: gpuMode,
          wndId: index+1
        })
      })
      let info={
        cameraIndexCode :  this.code,
        ezvizDirect: 0,
        recordLocation: 0,
        streamMode: streamMode,
        transMode: transMode,
        gpuMode: gpuMode,
        wndId: 1
      }
      let param = {
        funcName: "startPreview",
        argument: JSON.stringify(info)
      }
      let paramList = {
        funcName: "startMultiPreviewByCameraIndexCode",
        argument: JSON.stringify({list:list})
      }
      console.log(paramList)
      this.oWebControl.JS_RequestInterface(param);
    },
  }
}
</script>
<style lang="less" scoped>
  #videoList{
    height: 100%;
  }
</style>

