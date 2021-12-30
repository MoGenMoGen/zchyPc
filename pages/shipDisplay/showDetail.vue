<template>
<!--  船舶展示详情-->
     <div class="cont" :style="{width:bWidth+'px'}">
       <design-appo :show="show" @close="close" @submit="submit"></design-appo>
        <div class="cont-0 main" :style="{width:width+'px'}">
          <el-breadcrumb separator-class="el-icon-arrow-right" :style="{width:width+'px'}" class="bc main">
            <el-breadcrumb-item :to="{ path: './shipMan' }">船舶管理</el-breadcrumb-item>
            <el-breadcrumb-item :to="{ path: './shipDesign' }">船舶设计</el-breadcrumb-item>
            <el-breadcrumb-item ><p class="textBlue">船舶方案详情</p></el-breadcrumb-item>
          </el-breadcrumb>
        </div>

        <div class="cont-1 main" :style="{width:width+'px'}">
          <div class="cont-1-m">
            <img :src="selectImg">
          </div>
          <div class="cont-1-p" @mouseleave="leave()">
            <div v-for="(item,index) in imgList" :key="index" @click="toChoose(item,index)" >
              <img :src="item" :class="{active:index==selectId}">
            </div>
          </div>
        </div>
        <div class="cont-2 main" :style="{width:width+'px'}">
          <div class="cont-2-1">
            <div style="display: flex;align-items: center;">{{info ? info.nm : ''}}<img class="img2" :src="VRImg" @click.stop="toVR(info.id)" v-if="info.vrUrl&&info.vrUrl.split(',')[0]"></div>
            <div>
              <p @click="toCollect"><img :src="info.collected ? collected : noCollect" >收藏</p>
              <share></share>
            </div>
          </div>
          <div class="cont-2-2">
            <div>
              <p>整船价格:<span style="margin-left: 5px">￥</span><span class="big">{{fmoney(info.origMinPrice)}}</span><span>~</span><span class="big">{{fmoney(info.origMaxPrice)}}万</span></p>
              <p>设计院:<span @click="toDesign(info)" class="cursor">{{info.designNm}}</span></p>
            </div>
            <div>
              <p @click="toPage('./allDesign')">设计院逛逛</p>
              <p @click="show=true">设计预约</p>
            </div>
          </div>
        </div>
       <!--商品详细信息栏-->
       <div class="detail main"  :style="{width:width+'px'}">
         <pro-detail :info="info" ref="shipDetail" shipShow="true"></pro-detail>
       </div>
     </div>
</template>

<script>
  import {mapState} from 'vuex'
  import proDetail from "@/components/onlineTrading/proDetail";
  import DesignAppo from "@/components/designAppo";
  import share from "@/components/share";
  import moreIc from '../../assets/img/shipDisplay/更多.png'
  import collected from '@/assets/img/proDetail/collected.png'
  import noCollect from '@/assets/img/proDetail/noCollect.png'
  import VRImg from '../../assets/img/personal/VR.png'
    export default {
        name: "shipCat",
        data(){
            return{
              VRImg,
              collected,
              noCollect,
              moreIc,
              info:{
                nm:'',
                collected:'',
              },
              testImg:'https://fuss10.elemecdn.com/8/27/f01c15bb73e1ef3793e64e6b7bbccjpeg.jpeg',
              selectId:0,//初始状态激活的幻灯片的索引，从 0 开始
              selectImg:'',
              imgList:[],
              time:'',//定时器
              show:false,

            }
        },
      components:{proDetail,DesignAppo,share},
      computed: {
        ...mapState([
          'bWidth',
          'width',
          'proSize'
        ]),
      },
      async asyncData(context){
        if(process.client){
          return
        }
        let data = await context.app.api.shipDetail(context.query.id)
        return{
          data:data,
        }
      },
      mounted() {
        this.getInfo()
      },
      watch:{
        $route:{
          handler:function () {
            // this.getInfo()
          },
          immediate:true
        }
      },
      methods:{
        fmoney(s, n) {
            n = n > 0 && n <= 20 ? n : 2;
            s = parseFloat((s + "").replace(/[^\d\.-]/g, "")).toFixed(n) + "";
            var l = s.split(".")[0].split("").reverse(), r = s.split(".")[1];
            var t = "";
            for (let i = 0; i < l.length; i++) {
                t += l[i] + ((i + 1) % 3 == 0 && (i + 1) != l.length ? "," : "");
            }
            return t.split("").reverse().join("") + "." + r;
        },
        //收藏
        toCollect(){
          if (this.info.collected === 1) {

            this.api.delFollow(this.info.id).then(()=>{
              this.info.collected = 0;
            })
          } else {

            //收藏商品
            let param = {
              targetId: this.info.id,
              type: 1,
            }
            this.api.followShop(param).then(()=>{
              this.info.collected = 1;
            })
          }
        },
        async getInfo(){
          this.info = await this.api.shipDetail(this.$route.query.id)
          this.imgList = this.info.imgUrl.split(',')
          this.selectImg = this.imgList[0]
          this.startTime()
        },
        toPage(url){
          this.$router.push(url)
        },
        toDesign(info){
            this.$router.push({
              path:'./design',
              query:{
                id:info.designId,
              }
            })
        },
        leave(){
          window.console.log("鼠标离开")
            if(!this.time){
                this.startTime()
            }
        },
          //开始计时
          startTime(){
              let this_=this
              this.time= setInterval(()=>{
                  this_.selectId++
                  if(this_.selectId==this_.imgList.length){
                      this_.selectId=0
                  }
                  this_.selectImg=this_.imgList[this_.selectId]
              },2000)
          },
          //暂停计时期
          pauseTime(){
              //清除计时器
              clearInterval(this.time)
              this.time=''
          },
          toChoose(item,index){
              window.console.log(index)
              if(index!=this.selectId){
                  this.selectId=index
                  this.selectImg=item
                  this.pauseTime()
              }
          },
        close() {
          // 取消弹窗回调
          this.show = false
        },
        submit() {
          // 确认弹窗回调
          this.show = false
        },
        toVR(id){
          window.open('./mframe?typeCd=1&id='+id)
        }
      },

    }
</script>

<style scoped lang="less">
  .cont{
    margin: 0 auto;
    padding-bottom: 121px;
    background: url("../../assets/img/bg.png") no-repeat bottom center;
    .popupBox{
      position: fixed;
      width: 100%;
      height: 100%;
      left: 0;
      top: 0;
      display: flex;
      justify-content: center;
      align-items: center;
      background-color: rgba(0,0,0,0.23);
      z-index: 1001;

    }
    .cont-0{
      height: 54px;
      display: flex;
      align-items: center;
      padding-left: 20px;

    }
    .cont-1{
      background: white;
      padding: 21px;
      box-sizing: border-box;
      position: relative;
      .cont-1-m{
        width: 100%;
        img{
          width: 100%;
        }

      }
      .cont-1-p{
          position: absolute;
          display: flex;
          bottom: 21px;
          right: 30px;
          z-index: 1000;
          width: 100%;
          justify-content: flex-end;

          div{

            flex-shrink: 0;
            &:hover{
              cursor: pointer;
            }
            img{
              width: 120px;
              /*height: 120px;*/
              margin: 10px;
              transition: all 0.5s;
            }
          }
      }
    }
    .cont-2{

      background: white;
      padding: 0 21px;
      .cont-2-1{
        display: flex;
        justify-content: space-between;
        border-bottom: 1px solid #E1E6F0;
        padding-bottom: 21px;
        div:nth-of-type(1){
          color: #333333;
          font-size: 18px;
          padding:0 19px;
          // max-width:724px;
          flex: 1;
        }
        div:nth-of-type(2){
          display: flex;
          align-items: center;
          flex-shrink: 0;
          p{
            // height: 12px;
            font-size: 14px;
            display: flex;
            align-items: center;
            margin-right: 30px;
            &:hover{
              cursor: pointer;
            }
            img{
              width: 12px;
              height: 12px;
              margin-right: 5px;
            }
          }
        }
        .img2 {
          margin-left: 10px;
          cursor: pointer;
        }
      }
      .cont-2-2{
        display: flex;
        justify-content: space-between;
        height: 124px;
        align-items: center;
        div:nth-of-type(1){
          height: 100%;
          display: flex;
          flex-direction: column;
          justify-content: space-between;
          padding: 33px 20px;
          p:nth-of-type(1){
            span{
              color: #FF3E3E;
            }
            .big{
              font-size: 17px;
            }
          }
          p:nth-of-type(2){
            span{
             color: #2778BE;
              margin-left: 5px;
            }
          }
        }



        div:nth-of-type(2){
          display: flex;
          p{
            display: flex;
            align-items: center;
            justify-content: center;
            &:hover{
              cursor: pointer;
            }
            &:active{
              opacity: 0.5;
            }
          }
          p:nth-of-type(1){
            width:167px;
            height:42px;
            background:rgba(39,120,190,0);
            border:1px solid rgba(39, 120, 190, 1);
            border-radius:2px;
            font-size:16px;
            font-family:Microsoft YaHei;
            font-weight:400;
            color:rgba(39,120,190,1);
            margin-right: 30px;
          }
          p:nth-of-type(2){

            width:167px;
            height:42px;
            background:rgba(39,120,190,1);
            border-radius:2px;
            font-size:16px;
            font-family:Microsoft YaHei;
            font-weight:400;
            color:rgba(255,255,255,1);
            margin-right: 10px;
          }
        }
      }

    }
    .detail{
      background-color: #FFFFFF;
      border: 1px solid #ECECEC;
      box-sizing: border-box;
      padding: 20px;

    }
  }

.active{
  border: 2px solid white;
  transform: scale(1.1);
}
.textBlue{
  color: #2778BE;
}
</style>
