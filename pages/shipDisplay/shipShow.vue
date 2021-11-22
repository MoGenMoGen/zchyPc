<template>
<!--  船舶展示分类列表-->
     <div class="cont" :style="{width:bWidth+'px'}">
        <div class="cont-1 main" v-if="cImg" :style="{width:width+'px'}">
<!--          < el-image-->
<!--            :src="cImg"-->
<!--          :style="{height:370 +'px'}"></el-image>-->
          <img :src="cImg"/>
        </div>
        <div class="cont-2 main" :style="{width:width+'px'}">
            <div class="line1">
              <el-breadcrumb v-if="cNm" separator-class="el-icon-arrow-right" :style="{width:width+'px'}" class="bc main">
                <el-breadcrumb-item :to="{ path: './shipCat' }">船舶展示</el-breadcrumb-item>
                <el-breadcrumb-item ><p class="textBlue">{{cNm}}展示</p></el-breadcrumb-item>
              </el-breadcrumb>
              <el-breadcrumb v-else separator-class="el-icon-arrow-right" :style="{width:width+'px'}" class="bc main">
                <el-breadcrumb-item >>船舶展示</el-breadcrumb-item>
              </el-breadcrumb>
<!--              <p v-if="cNm"><span>船舶展示</span><span>&nbsp;&nbsp; >&nbsp;&nbsp; </span><span>{{cNm}}展示</span></p>-->
<!--              <p v-else><span>船舶展示</span></p>-->
            </div>
          <div class="line2">
<!--            <div class="line2-item" v-for="item in list" :key="item.id" @click="toDetail(item)" :style="{marginRight:width/24+'px',width:(width-width*4/24)/5+'px'}">-->
            <div class="line2-item" v-for="item in list" :key="item.id" @click="toDetail(item)" >
               <div class="img" :style="{height: width*0.185*9/16 + 'px'}" style="width: 100%;" ><img class="img1" :src="item.imgUrl" ><img class="img2" :src="VRImg" @click.stop="toVR(item.id)" v-if="item.vrUrl"></div>
               <p>{{item.nm}}</p>
               <p>船型：{{item.totalLen}}m</p>
            </div>
          </div>
        </div>
     </div>
</template>

<script>
  import {mapState} from "vuex";
    import moreIc from '../../assets/img/shipDisplay/更多.png'
    import VRImg from '../../assets/img/personal/VR.png'
    export default {
        name: "shipCat",
        data(){
            return{
              VRImg,
              cId:'',
              cNm:'',
              cImg:'',
              list:[],
                moreIc,
                testImg:'https://fuss10.elemecdn.com/8/27/f01c15bb73e1ef3793e64e6b7bbccjpeg.jpeg',
              imgList:[
                  {
                      id:1,
                      src:'https://fuss10.elemecdn.com/8/27/f01c15bb73e1ef3793e64e6b7bbccjpeg.jpeg',
                      nm:'钓鱼传0',
                      type:'船型：13.5m',
                  },
                  {
                      id:2,
                      src:'https://fuss10.elemecdn.com/8/27/f01c15bb73e1ef3793e64e6b7bbccjpeg.jpeg',
                      nm:'钓鱼传1',
                      type:'船型：13.5m',
                  },
                  {
                      id:3,
                      src:'https://fuss10.elemecdn.com/8/27/f01c15bb73e1ef3793e64e6b7bbccjpeg.jpeg',
                      nm:'钓鱼传2',
                      type:'船型：13.5m',
                  },
                  {
                      id:4,
                      src:'https://fuss10.elemecdn.com/8/27/f01c15bb73e1ef3793e64e6b7bbccjpeg.jpeg',
                      nm:'钓鱼传3',
                      type:'船型：13.5m',
                  },
                  {
                      id:5,
                      src:'https://fuss10.elemecdn.com/8/27/f01c15bb73e1ef3793e64e6b7bbccjpeg.jpeg',
                      nm:'钓鱼传4',
                      type:'船型：13.5m',
                  },
                  {
                      id:6,
                      src:'https://fuss10.elemecdn.com/8/27/f01c15bb73e1ef3793e64e6b7bbccjpeg.jpeg',
                      nm:'钓鱼传5',
                      type:'船型：13.5m',
                  },
                  {
                      id:7,
                      src:'https://fuss10.elemecdn.com/8/27/f01c15bb73e1ef3793e64e6b7bbccjpeg.jpeg',
                      nm:'钓鱼传5',
                      type:'船型：13.5m',
                  },],
            }
        },
        watch:{
          $route:{
            handler:function () {
              this.cNm = this.$route.query.cNm ? this.$route.query.cNm : ''
              if(this.$route.query.cId){
                this.cId = this.$route.query.cId ? this.$route.query.cId : '4942035725390848'
              }else if(this.$route.query.cId3){
                this.cId = this.$route.query.cId3 ? this.$route.query.cId3 : '4942035725390848'
              }
              this.cImg = this.$route.query.cImg ? this.$route.query.cImg : ''
            },
            immediate:true
          },
          cId:{
            handler:function () {
              if(this.cId){
                this.getList()
              }
            },
            immediate:true
          }
        },
      async asyncData(context){
        if(process.client){
          return
        }
        let qry = context.app.query.new()
        //若url截取的是cid3 就cid3，若为cid 就cid2
        if(context.query.cId){
          context.app.query.toW(qry,'cid2',context.query.cId)
        }
        if(context.query.cId3){
          context.app.query.toW(qry,'cid3',context.query.cId3)
        }
        let list = await context.app.api.shipList2(context.app.query.toEncode(qry))

        return{
          list:list,
        }
      },
      mounted() {

      },
      methods:{
        async getList(){
          let qry = this.query.new()
          //若url截取的是cid3 就cid3，若为cid 就cid2
          if(this.$route.query.cId){
            this.query.toW(qry,'cid2',this.cId)
          }
          if(this.$route.query.cId3){
            this.query.toW(qry,'cid3',this.cId)
          }
          this.query.toO(qry,'seq','asc')
          this.list = await this.api.shipList2(this.query.toEncode(qry))
        } ,
        //跳转到详情
        toDetail(item){
          this.$router.push({
            path:'./showDetail',
            query:{
              id:item.id,
            }
          })
        },
        toVR(id){
          window.open('./mframe?typeCd=1&id='+id)
        }
      },
      computed: {
        ...mapState([
          'bWidth',
          'width',
          'proSize',
          'shipSize2'
        ]),
      },
    }
</script>

<style scoped lang="less">
  .cont{
    margin: 0 auto;
    background: white;
    padding-bottom: 121px;
    background: url("../../assets/img/bg.png") no-repeat bottom center;
    .cont-1{
      margin-top: 10px;
      img{
        width: 100%;
       }
    }
    .cont-2{
      .line1{
        height: 58px;
        display: flex;
        align-items: center;
        color: #333333;
        p{
          span:nth-of-type(3){
            color: #2778BE;
          }
        }
      }
      .line2{
        width: 100%;
        overflow: hidden;
        display: flex;
        display: -webkit-flex;
        flex-wrap: wrap;
        .line2-item{
          cursor: pointer;
          float: left;
          width: 18.5%;
          margin-right: 1.5%;
          margin-bottom: 40px;
          &:nth-of-type(5n+5){
            margin-right: 0!important;
          }
          .img{
            position: relative;
            display: flex;
            display: -webkit-flex;
            width: 100% ;
            height: 100%;
            .img1{
              width: 100%;
              /*height: 100%;*/

              object-fit: cover;
            }
            .img2{
              position: absolute;
              top: 0;
              left: 0;
            }

          }
          p:nth-of-type(1){
            margin-top: 14px;
            width: 100%;
            text-align: center;
            font-size: 16px;
            color: #666666;
          }
          p:nth-of-type(2){
            margin-top: 14px;
            width: 100%;
            text-align: center;
            font-size: 14px;
            color: #666666;
          }
        }
      }
    }

  }
.textBlue{
  color: #2778BE;
}


</style>
