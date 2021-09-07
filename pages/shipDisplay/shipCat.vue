<template>
<!--  船舶展示分类列表-->
     <div class="cont" :style="{width:bWidth+'px'}">
          <div class="cont-1">
            <el-carousel :height="bannerHeight+'px'">
              <el-carousel-item v-for="item in adertList" :key="item.id">
                <img :src="item.imgUrl" style="width:100%">
              </el-carousel-item>
            </el-carousel>
          </div>
           <div class="cont-2 main" :style="{width:width+'px'}" >
             <div class="cont-top">
                <p><span></span>船舶展示</p>
<!--                <p>查看全部<img :src="moreIc"></p>-->
             </div>

             <div class="line1" :style="{height:(width*2/3-30)/3*164/256*2+10+'px'}" >
                <div class="line1-1" v-if="shipClassify[0]" :style="{backgroundImage:`url('${shipClassify[0].imgUrl}')`}" @click="toScroll(shipClassify[0].id)">
                  <div>
                    <p><span></span>{{shipClassify[0].nm}}<span></span></p>
                    <p>50多种</p>
                  </div>
                </div>
               <div class="line1-2">
                 <div v-for="(item,index) in shipClassify" v-if="index>0"
                      :key="item.id"
                      @click="toScroll(item.id)"
                      :style="{backgroundImage:`url('${item.imgUrl}')`, width:(width*2/3-30)/3+'px',height:(width*2/3-30)/3*164/256+'px'}">
                    <p>{{item.nm}}</p>
                 </div>
               </div>
             </div>

             <div class="line2">
               <div class="line2-item" v-for="(item,index) in shipClassify" :key="item.id" :id="'ship'+item.id">
                 <div class="cont-top">
                   <p><span></span>{{item.nm}}</p>
                   <p @click="toList(item)">查看全部<img :src="moreIc"></p>
                 </div>
                 <div class="item-cont">
                   <div class="item-pre"  @click="toList(item)" :style="{height:(width*534/1200-20)/2*164/256*2+10+'px'}">
                       <img :src="item.imgUrl2">
                     </div>
                     <div class="item-img">
                       <div v-for="(j,i) in item.list" :key="j.id"
                        :style="{backgroundImage:`url('${j.imgUrl}')`,width:(width*534/1200-20)/2+'px',height:(width*534/1200-20)/2*164/256+'px'}"
                        @click="toList2(j)">
                         <p>
                           <span >{{j.nm}}</span>
                           <!--<span>{{j.rmks}}</span>-->
                         </p>
                       </div >
                     </div>
                   </div>
                 </div>

             </div>

           </div>


     </div>
</template>

<script>
    import moreIc from '../../assets/img/shipDisplay/更多.png'
    import {mapState} from "vuex";
    export default {
        name: "shipCat",
        data(){
            return{
              adertList:[],
              bannerHeight:'400',
              shipClassify:[],
              moreIc,
            }
        },
      computed: {
        ...mapState([
          'bWidth',
          'width'
        ]),
      },
      mounted() {
        this.getAdert()
        this.getShip()
      },
      methods:{
        //获取广告轮播
        async getAdert(){
          this.adertList = await this.api.getAdert('shipAdert')
          if(this.adertList.length>0){
            let myImg = new Image()
            myImg.src = this.adertList[0].imgUrl
            myImg.onload = ()=>{
              if(myImg.width>this.bWidth){
                this.bannerHeight = myImg.height/myImg.width*this.bWidth
              }else {
                this.bannerHeight = myImg.height
              }
            }
          }
        },
        //获取船舶分类信息
        async getShip(){
          let qry = this.query.new()
          this.query.toW(qry,'pid','4942035725390848','EQ')
          this.query.toO(qry,'seq','asc')
          this.shipClassify = await this.api.getClassify(this.query.toEncode(qry))
          this.shipClassify = this.shipClassify.slice(0,7)
          for(let i=0;i<this.shipClassify.length;i++){
            let qry2 = this.query.new()
            this.query.toW(qry2,'pid',this.shipClassify[i].id,'EQ')
            this.query.toO(qry2,'seq','asc')
            let data = await this.api.getClassify(this.query.toEncode(qry2))
            let listN=[]
            data.forEach((item,index)=>{
                if(item.imgUrl){
                  listN.push(item)
                }
            })
            this.shipClassify[i].list=listN.slice(0,4)
            console.log(this.shipClassify[i].list)


            this.shipClassify[i].imgUrl = this.shipClassify[i].imgUrl ? this.shipClassify[i].imgUrl.split(',')[0] : ''
            this.$set(this.shipClassify,i,this.shipClassify[i])
          }
          if(this.$route.query.id){
            this.toScroll(this.$route.query.id)
          }
        },
        //跳转到列表
        toList(item){
          this.$router.push({
            path:'./shipShow',
            query:{
              cId:item.id,
              cNm:item.nm,
              cImg:item.icon1
            }
          })
        },
        toList2(j){
          this.$router.push({
            path:'./shipShow',
            query:{
              cId3:j.id,
              cNm:j.nm,
              cImg:j.imgUrl2
            }
          })
        },
        //跳转到详情
        toDetail(item){
          this.$router.push({
            path:'./showDetail',
            query:{
              id:item.id,
              id:item.id,
            }
          })
        },
        //跳转到指定位置
        toScroll(id){
          document.getElementById('ship'+id).scrollIntoView()
        }
      }
    }
</script>

<style scoped lang="less">
  .cont{
    margin: 0 auto;
    background: white;
    padding-bottom: 121px;
    background: url("../../assets/img/bg.png") no-repeat bottom center;
    .cont-2{

      .line1{
        display: flex;
        .line1-1{
          flex: 1;
          height: 100%;
          position: relative;
          background-repeat: no-repeat;
          background-size: cover;
          background-position: center;
          cursor: pointer;
          div{
            width: 100%;
            position: absolute;
            bottom: 5px;
            color: white;
            p{
              margin-top: 10px;
              width: 100%;
              font-size: 18px;
              display: flex;
              display: -webkit-flex;
              align-items: center;
              justify-content: center;
              margin-bottom: 10px;
              /*分隔线*/
              span{
                height: 1px;
                width: 64px;
                background:rgba(243,243,243,0.4);
                display: inline-block;
                margin: 0 20px;
              }
              &:first-child{
                font-size: 24px;
              }

            }

          }
        }
        .line1-2{
          flex: 2;
          height: 100%;
          overflow: hidden;
          div {
            background-position: center;
            background-size: cover;
            background-repeat: no-repeat;
            position: relative;
            float: left;
            cursor: pointer;
            margin-bottom: 10px;
            margin-left: 10px;
            p{
              width: 100%;
              background:rgba(0,0,0,0.5);
              font-size: 18px;
              box-sizing: border-box;
              bottom: 0;
              height: 46px;
              line-height: 46px;
              position: absolute;
              color: white;
              text-align: center;
            }

          }

        }
      }
      .line2{
        .line2-item{
          margin-top: 20px;
          .item-cont{
            display: flex;
            .item-pre{
              flex: 666;
              img{
                width: 100%;
                height: 100%;
                cursor: pointer;
              }
            }
            .item-img{
              display: flex;
              flex-wrap: wrap;
              flex: 534;
              // padding-left: 10px;
              // justify-content: space-between;
              div{
                // flex: 1;
                position: relative;
                cursor: pointer;
                background-repeat: no-repeat;
                background-size: cover;
                background-position: center;
                margin-bottom: 10px;
                margin-left: 10px;
                p{
                  position: absolute;
                  color: white;
                  width: 100%;
                  bottom: 0px;
                  span{
                    width: 100%;
                    text-align: center;
                    display: block;
                    &:first-child{
                      font-size: 18px;
                      margin-bottom: 6px;
                    }
                  }
                }
              }
            }
          }
        }
      }
    }

  }

  /*船舶头部*/
  .cont-top{
    /*padding-left:20px;*/
    display: flex;
    align-items: center;
    justify-content: space-between;
    height: 36px;
    &:first-of-type{
      margin-top: 18px;
    }
    p:nth-of-type(1){
      font-size:18px;
      font-family:PingFang SC;
      font-weight:500;
      color:rgba(51,51,51,1);
      line-height:18px;
      display: flex;
      align-items: center;
      span{
        color:  #2778BE;
        height: 16px;
        width: 4px;
        background: #2778BE;
        display: inline-block;
        margin-right: 8px;
      }
    }
    p:nth-of-type(2){
      display: flex;
      align-items: center;
      font-family:PingFang SC;
      font-weight:500;
      color:rgba(51,51,51,1);
      line-height:24px;
      img{
        margin-left: 5px;
        width:15px;
        height:15px;
      }
      &:hover{
        cursor: pointer;
      }
    }
  }

</style>
