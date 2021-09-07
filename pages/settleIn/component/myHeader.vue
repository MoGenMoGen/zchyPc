<template>
  <div class="header" :style="{width:bWidth+'px'}">
      <div class="main"  :style="{width:width+'px'}">
         <img :src="logoNew" @click="toPageIndex()"/>
        <!--audit: "审核状态  [r]（1：待审核，2：审核通过，3：审核不通过）"-->
        <p v-if="info && (info.audit==1 || info.audit==0)">已提交入驻，<span @click="toPage()">请查看审核进度 ></span></p>
        <p v-if="info && info.audit==2">已提交入驻，<span @click="toPage()">审核通过 ></span></p>
        <p v-if="info && info.audit==3">已提交入驻，<span @click="toPage()">审核不通过 ></span></p>
      </div>
  </div>
</template>

<script>
import {mapState} from "vuex";
import logo from '@/assets/img/logo.png'
export default {

  data(){
    return{
      // logoNew:'https://sinovat.oss-cn-shanghai.aliyuncs.com/0875a132ec924b17b73aae3d9c644a81_logo.png',//聚联logo
      logoNew:logo,
      info:null,
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
    currentRole:{
      handler:function (e) {
        if(e){
          this.getInfo()

        }

      },
      immediate:true
    }
  },
  components: {
  },
  mounted(){
   // console.log(this.until.seGet('currentRole'))

  },
  methods:{
    async getInfo(){
      console.log(this.currentRole)
      this.info = await this.api.getRoleDetail(this.currentRole.id)
    },
    //页面跳转
    toPage(){
      this.$router.push('../personal/persInfo')
    },
    toPageIndex(){
      this.$router.push('/')
    }
  }
}
</script>
<style lang="less">


</style>
<style lang="less" scoped>
  @import url("../../../assets/css/init.less");
  .header{
    padding: 27px 0;
    box-shadow:0px 6px 10px 0px rgba(13,4,8,0.10);
    margin-bottom: 20px;
    .main{
      display: flex;
      display: -webkit-flex;
      align-items: flex-end;
      img{
        cursor: pointer;
        width: 450px;
      }
    }

      p{
        flex: 1;
        text-align: right;
        font-size: 14px;
        span{
          color: @redColor;
          cursor: pointer;
        }
      }
  }
</style>
