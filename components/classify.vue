<template>
  <div class="navigation" @mouseenter="showClassify=true" @mouseleave="closeClassify" :style="{width:width*0.1835+'px',background:background?background:'#2778BE'}">
    <div
      v-for="(item,index) in naviList" :key="item.id" class="item"  @mouseenter="changeMenu(index)" :class="{active:item.id==currentId}" @click="toList(1,item)">

      <p class="level1">
       <span> <img :src="item.id==currentId ? item.icon2 : item.icon1" alt="1"/> {{item.nm}}   <i class="el-icon-arrow-right"></i></span>
        <span class="rmks">{{item.rmks}}</span>
      </p>

    </div>
    <el-scrollbar class="classifyChildren" v-show="showClassify" :style="{background:background?'rgba(255,255,255,0.9)':'#ffffff'}">
      <div v-for="item in currentChildren" :key="item.id" class="classifyChildList">
        <p  @click="toList(2,item)">
          <span style="display: block">{{item.nm}}</span>
          <span style="display: block;font-size: 12px">{{item.rmks}}</span>
        </p>
        <i class="el-icon-arrow-right"></i>
        <ul>
          <li v-for="(v,i) in item.children" @click="toList(3,v)">
          <span style="display: block">{{v.nm}}</span>
          <span style="display: block;font-size: 12px" >{{v.rmks}}</span>
          </li>
        </ul>
      </div>
    </el-scrollbar>
  </div>
</template>
<script>
  import {mapState} from "vuex";
  export default {
    props:['background'],
    data(){
      return{
        showClassify:false,
        currentChildren:[],
        //导航栏
        naviList:[],
        currentId:'',
      }
    },
    components: {
    },
    watch:{

    },
    computed: {
      ...mapState([
        'ifLogin',
        'bWidth',
        'width',
        'userInfo',
      ])
    },
     mounted(){
      this.getData()
    },
    methods:{
      async getData(){
        this.naviList = await this.api.shopClassify()
        this.naviList = this.naviList.slice(0,9)

      },
      //页面跳转
      toPage(item){
        if(item.href){
          this.$router.push(item.href)
        }else {
          this.$router.push(item)
        }
      },
      //去列表
      toList(type,item){
        console.log(item)
        let idArr=[]
        if(type==1){
          idArr[0]=item.id
        }
        if(type==2){
          idArr = [item.pid,item.id]
        }
        if(type==3){
          idArr = [this.currentId,item.pid,item.id]
        }
        if(item.ship){
          this.$router.push({
            path:'../Trade/shipTrade',
            query:{
              id:JSON.stringify(idArr)
            }
          })
        }else {
          this.$router.push({
            path:'../Trade/productTrading',
            query:{
              id:JSON.stringify(idArr)
            }
          })
        }

      },

      //关闭分类弹窗
      closeClassify(){
        this.showClassify=false
        this.currentId = ''
      },
      //鼠标滑动
      changeMenu(index){
        this.currentId = this.naviList[index].id
        this.currentChildren = this.naviList[index].children
        // console.log('分类列表')
        // console.log(this.currentChildren)
      }
    }
  }
</script>

<style lang="less" scoped>
  @import url("../assets/css/init.less");
  .navigation{
    display: inline-flex;
    vertical-align: top;
    height: 440px;
    position: relative;
    flex-direction: column;
    font-size:16px;
    font-weight: bold;

    .item{
      flex: 1;
      display: flex;
      align-items: center;
      height: 30px;
      width: 100%;
      /*background-color: #2778BE;*/
      margin: 0;
      border: 0;
      cursor: pointer;



      .level1{
        color: #ffffff;
        width: 100%;

        text-align: left;
        padding: 0;
        margin-left: 0;
        span{
          height: 20px;
          margin-left: 0;
          padding: 0;
          display: flex;
          align-items: center;
          position: relative;
          i{
            position: absolute;
            right: 7px;
            top: 50%;
            transform: translateY(-50%);
          }
          img{
            margin-left: 14px;
            margin-right: 7px;
            width: 16.8px;
          }
        }
        .rmks{
          white-space: nowrap;
          font-size: 14px;
          padding: 0 ;
          padding-left: 36.5px;
          font-weight: bold;
        }
      }
    }




    .classifyChildren{
      z-index: 99;
      width: 620px;
      /*overflow-x: hidden;*/
      position: absolute;
      top:0;
      left: 100%;
      height: 100%;
      box-shadow:0px 0px 5px 0px rgba(86,86,86,0.33);
      text-align: left;
      font-size: 16px;
      font-weight: normal;
      .classifyChildList{
        display: flex;
        display: -webkit-flex;
        padding: 20px 0;
        line-height: 22px;
        align-items: center;
        >p{
          width: 130px;
          // text-align: justify;
          // text-align-last: justify;
          padding-right: 10px;
          color: #333333;
          cursor: pointer;
          padding-left: 20px;
        }
        p:hover{
          color: @themeColor;
        }
        i{
          color: #999;
        }
        ul{
          flex: 1;
          padding-right: 20px;
          text-align: left;
          li{
            display: inline-block;
            padding-left:30px;
            color: #666666;
            cursor: pointer;
          }
          li:hover{
            color: @themeColor;
          }
        }
      }
    }

    .active{
      background-color: #ffffff !important;
      box-shadow:0px 0px 5px 0px rgba(86,86,86,0.33)  !important;
      p{
        color: @themeColor  !important;
      }
    }
  }


</style>

