<template>
  <!-- 推荐船舶 -->

    <div class="recBody">
      <div class="title">
        <p><span class="line"></span>推荐船型</p>
        <ul>
          <li v-for="(item,index) in list2" :key="index" :class="{active : item.id==idNow}" @click="changeActive(item)">
            <p>{{item.nm}}</p>
            <span class="line2"></span>
          </li>
        </ul>
        <p class="more" @click="toCat">更多<i class="el-icon-arrow-right"></i></p>
      </div>
      <div class="content">
        <!--大图-->
        <img  alt="" :src="imgUrlNow" :style="{width:width*666/1200 + 'px',height:(width*534/1200-20)/2*164/256*2+10+'px'}" @click="toShow"/>
        <div class="down">
          <div class="part" v-for="(item,index) in list3" :key="index" :style="{width:(width*534/1200-20)/2+'px',height:(width*534/1200-20)/2*164/256+'px'}" @click="toList(item)">
            <img :src="item.imgUrl" alt="" />
            <div class="ct">
              <p>{{item.nm}}</p>
              <!--<p>{{item.rmks}}</p>-->
            </div>
          </div>
        </div>
      </div>
    </div>
</template>

<script>
  import {mapState} from "vuex";
  export default {
    name: "shipDesign",
    components: {},
    data(){
      return{
        list2:[],
        idNow:'',
        imgUrlNow:'',
        nmNow:'',
        list3:[],
      }
    },
    computed: {
      ...mapState([
        'bWidth',
        'width',
        'currentRole',
        'typeSize',
        'ifLogin'
      ])
    },
    created(){
    },
    mounted() {
      this.getShipList()
    },
    methods:{
      async getShipList(){
        let qry = this.query.new()
        this.query.toW(qry,'pid','4942035725390848','EQ')
        this.query.toO(qry,'seq','asc')
        this.list2=await this.api.getClassify(this.query.toEncode(qry))
        console.log('船舶类型')
        console.log(this.list2)
        this.idNow=this.list2[0].id
        this.nmNow=this.list2[0].nm
        this.imgUrlNow=this.list2[0].imgUrl2
        this.changeActive(this.list2[0])
      },

      async changeActive(item){
        // console.log('id:'+item.id)
        this.idNow=item.id
        this.nmNow=item.nm
        this.imgUrlNow=item.imgUrl2
        let qry = this.query.new()
        this.query.toW(qry,'pid',item.id,'EQ')
        this.query.toO(qry,'seq','asc')
        this.list3=await this.api.getClassify(this.query.toEncode(qry))
        this.list3 = this.list3.filter(item=> item.imgUrl)
        this.list3=this.list3.slice(0,4)

        // console.log('this.list3',this.list3)
      },
      toShow(){
        this.$router.push({
          path:'./shipShow',
          query:{
            cId:this.idNow,
            cNm:this.nmNow,
            cImg:this.imgUrlNow
          }
        })
      },
      toList(item){
        this.$router.push({
          path:'./shipShow',
          query:{
            cId3:item.id,
            cNm:item.nm,
            cImg:item.imgUrl2
          }
        })
      },
      toCat(){
        this.$router.push('./shipCat')
      }
    },
  }
</script>

<style lang="less" scoped>
  @import url("../assets/css/init.less");

  .recBody{
    .title{
      display: inline-flex;
      align-items: center;
      width: 100%;
      >p{
        font-size: 18px;
        color: #333333;
        display: flex;
        flex-direction: row;
        align-items: center;
        line-height: 76px;
        width: 15%;
        .line{
          display: block;
          width: 4px;
          height: 16px;
          background-color: #2778BE;
          margin: 0 8px 0 0;
        }
      }
      ul{
        flex: 1;
        display: flex;
        flex-direction: row;
        justify-content: space-between;
        margin-left: 30%;
        height: auto;
        li{
          width: auto;
          display: flex;
          align-items: center;
          cursor: pointer;
          line-height: 35px;
          >p{
            color: #666;
            font-size: 14px;
            line-height: 35px;
          }
          .line2{
            display: block;
            width: 1px;
            height: 14px;
            opacity:0.2;
            background-color: #000;
            margin-left: 20px;
          }
        }
        .active{
          >p{
            color: #0064B2;
            border-bottom: 2px solid #2778BE;
          }
        }
      }
      .more{
        font-size: 14px;
        line-height: 30px;
        color: #999999;
        margin-left: 20px;
        width: auto;
        cursor:pointer;
      }
    }
    .content{
      width: 100%;
      display: flex;
      display: -webkit-flex;
      >img{
        cursor: pointer;
        flex: 666;
      }
      .down{
        /*margin-top: 5px;*/
        display: flex;
        flex-wrap: wrap;
        flex: 534;
        .part{
          position: relative;
          cursor: pointer;
          background-repeat: no-repeat;
          background-size: cover;
          background-position: center;
          margin-bottom: 10px;
          margin-left: 10px;
          img{
            width: 100%;
            height: 100%;
            // border: 1px solid #999999;
          }
          .ct{
            position: absolute;
            bottom: 0px;
            width: 100%;
            align-items: center;
            text-align: center;
            p:nth-child(1){
              line-height: 30px;
              font-size: 20px;
              text-align: center;
              color: #FFFFFF;
              // color: #999999;
            }
            p:nth-child(2){
              line-height: 20px;
              text-align: center;
              color: #FFFFFF;
              // color: #999999;
            }
          }
        }
        .part:last-child{
          margin-right: 0;
        }
      }
    }
  }

</style>
