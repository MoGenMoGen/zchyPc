<template>
<!--  基本信息-->
  <div>
    <div class="line1">

      <div class="th1">
       <el-carousel height="150px" v-viewer>
          <el-carousel-item v-for="(item,index) in info.imgList" :key="index">
            <img :src="item" >
          </el-carousel-item>
        </el-carousel>
        <!-- <img :src="info.imgList? info.imgList[0] : info.imgUrl" > -->
      </div>
      <div class="th2">
        <p>船舶编号：<span style="color: #2778BE;font-weight: bold ">{{info.cd}}</span></p>
        <p>船舶名称：<span>{{info.nm}}</span></p>
        <!--<p>整船金额：<span>￥{{info.totalPrice}}/艘</span></p>-->
        <p>船舶状态：<span style="color: #2778BE;">{{info.statusNm}}</span></p>
        <p>交付时间：<span>{{info.deliveryTm}}</span></p>
      </div>
      <div class="th3">
<!--        <p>合同编号：<span >{{info.contractNo}}</span></p>-->
<!--        <p>订单编号：<span>{{info.orderNo}}</span></p>-->
      </div>
      <div class="th4">
        <img :src="'/ship/docs/apis/qrcode/'+id">
      </div>


    </div>

    <div>
      <p style="margin-bottom: 10px">合同编号：<span style="word-break: break-word;">{{info.contractNo}}</span></p>
      <p>订单编号：<span style="word-break: break-word">{{info.orderNo}}</span></p>
    </div>

    <p class="title">主要参数</p>
    <div class="info">
<!--      1-->
      <div>
        <p>
          <span>船舶状态:</span>{{info.statusNm}}
        </p>
        <p>
          <span>船舶证签发日期:</span>{{info.signDate}}
        </p>
        <p>
          <span>船舶类型:</span>{{info.typesNm}}
        </p>
        <p>
          <span>船舶户籍:</span>{{info.registry}}
        </p>

      </div>
<!--     2 -->
      <div>
        <p>
          <span>航区:</span>{{info.sailingAreaNm}}
        </p>
        <p>
          <span>船舶总长(m):</span>{{info.totalLen}}
        </p>
        <p>
          <span>船体材质:</span>{{info.hullMaterialNm}}
        </p>
        <p>
          <span>完工日期:</span>{{info.completeDate}}
        </p>
      </div>
<!--     3 -->
      <div>
        <p>
          <span>公约长度:</span>{{info.lengthNm}}
        </p>
        <p>
          <span>公约船长(m):</span>{{info.pactLen}}
        </p>
        <p>
          <span>主机:</span>{{info.hostBrand}}
        </p>
        <p>
          <span>主机功率(kW):</span>{{info.hostPower}}
        </p>
      </div>
<!--     4 -->
      <div>
        <p>
          <span>齿轮箱:</span>{{info.gearBrand}}
        </p>
        <p>
          <span>齿轮箱型号:</span>{{info.gearModel}}
        </p>
        <p>
          <span>发电机:</span>{{info.generatorSetBrand}}
        </p>
        <p>
          <span>发电机功率(kW):</span>{{info.generatorSetPower}}
        </p>
      </div>
<!--     5 -->
      <div>
        <p>
          <span>制冷机组品牌:</span>{{info.refrigeratorBrand}}
        </p>
        <p>
          <span>制冷机功率(kW):</span>{{info.refrigeratorPower}}
        </p>
        <p>
          <span>船舶型宽(m):</span>{{info.width}}
        </p>
        <p>
          <span>船舶型深(m):</span>{{info.deep}}
        </p>

      </div>
<!--     6 -->
      <div>
        <p>
          <span>满载排水量(t):</span>{{info.loadDisplacement}}
        </p>
        <p>
          <span>满载吃水(m):</span>{{info.loadDraught}}
        </p>
        <p>
          <span>定员(P):</span>{{info.complement}}
        </p>
        <p>
          <span>航速(kn):</span>{{info.speed}}
        </p>
      </div>
<!--     7 -->
      <div>
        <p>
          <span>总吨位:</span>{{info.totalTon}}
        </p>
        <p>
          <span>净吨位:</span>{{info.netTon}}
        </p>
        <p></p>
        <p></p>
      </div>

      <div class="entireLine" v-for="item in paraList">
        <span class="colNm" v-show="item.colNm">{{item.colNm}} : </span><span> {{item.value}}</span>
      </div>

    </div>
    <p class="title">船舶展示</p>
      <div v-html="info.intro">
      </div>
  </div>

</template>

<script>
    export default {
        name: "fangansheji",
        props:{
          title:{
              type:String,
              default:'设计'
          }
        },
        data(){
            return{
              info:{},
              paraList:[],
              qrCode:'',
              id:''
            }
        },
        mounted() {
          this.id= this.until.getQueryString('id')
          this.getInfo()
        },
        methods:{
          async getInfo(){
            this.info=await this.api.shipInfoBasic(this.id,{orgEnterId:JSON.parse(this.until.seGet('currentRole')).id})
            this.info.imgList=this.info.imgUrl?this.info.imgUrl.split(','):[]
            console.log(this.info.imgList)
            let dataList=JSON.parse(this.info.mainParams)
            this.paraList=dataList.args

            // console.log(this.paraList)
          },
        }

    }
</script>

<style scoped lang="less">
  .line1{
    display: flex;
    width: 100%;
    height: 144px;
    margin-bottom: 52px;
    justify-content: space-between;
    div{
      width: 249px;
    }
    .th1{
      img{
        height: 100%;
        width: 249px;
      }
    }
    .th2{
      display: flex;
      flex-direction: column;
      justify-content: space-between;
      margin-left: 10px;
    }
    .th3{
      p:nth-of-type(2){
        margin-top: 11px;
      }
    }
    .th4{
      width: 105px;
      height: 105px;
    }
  }

  .title{
    width: 100%;
    height: 40px;
    background: #EEEEEE;
    color: #666666;
    padding-left: 28px;
    display: flex;
    align-items: center;
  }
  .info{

    color: #666666;
    padding: 15px 10px;
    div{
      display: flex;
      p{
        flex: 1;
        height: 45px;
        display: flex;
        align-items: center;
      }
    }
    .entireLine{
      height: 45px;
      align-items: center;
      .colNm{
        margin-right: 0.4rem;
      }
    }
  }
</style>
