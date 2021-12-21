<template>
    <div class="detailMain">
      <div class="tab">
        <!--@click="currentType=item.cd"-->
        <p @click="currentType=item.cd" :class="{active:currentType==item.cd}" v-for="(item,index) in tabList" :key="index" v-if="item.type.indexOf(type)>-1">
          {{item.nm}}
        </p>
      </div>
      <div>
        <div class="detailDesc" v-show="currentType=='detail'">
          <!--仅船舶产品和船舶方案有下面的参数-->
          <div class="infoParam" v-if="info.ship || type=='manage'">
             <div class="info">
               <p>船舶名称：{{param ? param.nm : ''}}</p>
               <!--<p>船舶所有权人：中创海洋</p>-->
               <p v-if="param && param.cd">中创船型编码：{{param.cd}}</p>
               <p v-if="info.supplier">承建船厂：<span @click="toPage()">{{info.supplier}}</span></p>
             </div>
             <p v-if="param">船舶主要参数</p>
            <ul v-if="param">
              <li>船舶类型：{{param.typesNm}}</li><li>船体材质：{{param.hullMaterialNm}}</li>
              <li>船籍：{{param.registry}}</li><li>船舶状态：{{param.statusNm}}</li>
              <li>船舶航区：{{param.sailingAreaNm}}</li><li>完工日期：{{param.completeDate}}</li>
              <li>船舶证书签发日期：{{param.signDate}}</li><li>船舶总长(m)：{{param.totalLen}}</li>
              <li>公约船长(m)：{{param.pactLen}}</li><li>船舶型宽(m)：{{param.width}}</li>
              <li>船舶型深(m)：{{param.deep}}</li><li>满载吃水(m)：{{param.loadDraught}}</li>
              <li>满载排水(t)：{{param.loadDisplacement}}</li><li>船舶航速(m/s)：{{param.speed}}</li>
              <li>船舶定员：{{param.complement}}</li><li>总吨位：{{param.totalTon}}</li>
              <li>净吨位：{{param.netTon}}</li><li>主机型号：{{param.hostBrandModel}}</li>
              <li>主机功率(kW)：{{param.hostPower}}</li><li>齿轮箱型号：{{param.gearModel}}</li>
              <li>发电机组型号：{{param.generatorSetBrandModel}}</li><li>发电机组功率(kW)：{{param.generatorSetPower}}</li>
              <li>制冷机组功率(kW)：{{param.refrigeratorPower}}</li><li></li>
              <!-- <li>公约长度：{{param.lengthNm}}</li>
              <li>主机品牌：{{param.hostBrand}}</li>
              <li>齿轮箱品牌：{{param.gearBrand}}</li>
              <li>发电机组品牌：{{param.generatorSetBrand}}</li>
              <li>制冷机组品牌：{{param.refrigeratorBrand}}</li> -->
              <li v-for="item in paramList" v-if="paramList.length>0">
                {{item.colNm}}：{{item.value}}
              </li>
            </ul>
          </div>
          <div v-html="info.cont || info.intro" ></div>
          <!--船舶管理详情不显示下面的东西-->
          <service v-if="!shipShow"></service>
          <div class="title"  v-if="!shipShow">本店精选商品</div>
          <featured :list="proList" :type="type"  v-if="!shipShow" />
        </div>
        <!--仅船舶产品和船舶方案有下面的设备-->
        <ship-equip :width="width-40" :type="shipShow ? 2 : 3" v-if="type ==='ship'" v-show="currentType ==='equip'" :id="currentId"></ship-equip>
        <!--设计方案-->
        <ship-equip-design :width="width-40" :type="shipShow ? 2 : 3" v-if="shipShow" v-show="currentType ==='equip'" :id="currentId"></ship-equip-design>
        <certificate v-if="currentType ==='certificate'" :list = "ceList"></certificate>
        <!--监理建造-->
        <jianlijianzao v-if="currentType ==='service1'" ref="jianlijianzao"></jianlijianzao>
        <!--检验检测-->
        <jianyanjiance v-if="currentType ==='service2'" ref="jianyanjiance"></jianyanjiance>
        <!--<div v-show="currentType ==='service'">-->
          <!--<div class="serviceList" v-for="(item,index) in serviceList" :key="index">-->
              <!--<img :src="item.imgUrl"/>-->
          <!--</div>-->
        <!--</div>-->
      </div>
    </div>
</template>

<script>
  import {mapState} from "vuex";
  import certificate from './certificate'
  import service from './service'
  import featured from './featured'
  import shipEquip from './shipEquipTree'
  import shipEquipDesign from './shipEquipTree'
  import jianlijianzao from '../person/pinzhibaozhang' //监理建造
  import jianyanjiance from '../person/jianyanjiance' //检验检测
    export default {
      name: "productList",
      props:['info','shipShow'],
      data(){
          return{
            currentId:'',
            serviceList:[],
            proList:[],
            ceList:[],
            currentType:'detail',
            type:'',
            tabList:[{
               nm:'详细描述',
               cd:'detail',
               type:'product,ship,manage',
            },{
              nm:'船舶设备',
              cd:'equip',
              type:'ship,manage',
            },{
              nm:'检验证书',
              cd:'certificate',
              type:'product'
            },{
              nm:'建造监理',
              cd:'service1',
              type:'',
            },{
              nm:'检验检测',
              cd:'service2',
              type:'',
            }],
            param:{},
            paramList:[]
          }
      },
      components:{
        certificate,
        service,
        featured,
        shipEquip,
        shipEquipDesign,
        jianlijianzao,
        jianyanjiance
      },
      computed:{
        ...mapState([
          'bWidth',
          'width'
        ]),

      },
      watch:{
        info:{
          handler:function () {
            if(this.info){
              this.getData()
            }
          },
          immediate:true
        },
        currentType(e){
          // if(e ==='service1'){
          //   console.log('ddd')
          //   this.$refs.jianlijianzao.getInfo(this.currentId)
          // }else if(e ==='service2'){
          //   // console.log('ddd')
          //   this.$refs.jianyanjiance.getInfo(this.currentId)

          // }
        }
      },
      methods:{
        async getData(){
          this.type = this.info.ship ? 'ship' : 'product'
          // console.log('============'+this.shipShow)
          if(this.shipShow){ //船舶管理
            this.type = 'manage'
            console.log('船舶管理')
            console.log(this.info)
             this.param = this.info
            this.currentId = this.info.id
            let arr = JSON.parse(this.param.mainParam)
            this.paramList.push(...arr.args)
          }else { //装备在线
            //本店精选
            let id = this.info.id ? this.info.id : this.until.getQueryString('id')
            this.proList = await this.api.proDetailChoice(id)
            //检测证书
            this.ceList = await this.api.proDetailCeList(id)
          }

        },
        async getInfo(id){  //船舶参数信息
          this.currentId = id
          this.param = await this.api.shipInfo(id)
          // console.log('=================')
          // console.log(this.param)
          if(!this.param){
            return
          }
          this.$emit('setType',this.param)
          let arr = JSON.parse(this.param.mainParams)
          this.paramList.push(...arr.args)
          //检验证书
          this.ceList = await this.api.shipDetailCeList(this.info.id)
          //品质保障
          this.serviceList = await this.api.shipDetailSurvey(this.info.id)
          this.$refs.jianyanjiance.getInfo(id)
          this.$refs.jianlijianzao.getInfo(id)
        }
      }
    }
</script>

<style lang="less" scoped>
  @import url("../../assets/css/init.less");
  .detailMain{
    .tab{
      width: 100%;
      background: #F7F7F7;
      height: 40px;
      line-height: 40px;
      box-sizing: border-box;
      border-bottom: 1px solid @redColor;
      p{
        width: 129px;
        display: inline-block;
        text-align: center;
        color: #666666;
        cursor: pointer;
      }
      p.active{
        color: #ffffff;
        background: @redColor;
      }
    }
    .title{
      width: 100%;
      background: #F7F7F7;
      border: 1px solid #eeeeee;
      height: 40px;
      line-height: 40px;
      text-indent: 28px;
      color: #666666;
      margin: 30px 0;
    }
    .infoParam{
      .info{
        display: flex;
        display: -webkit-flex;
        justify-content: space-between;
        height: 72px;
        line-height: 72px;
        span{
          cursor: pointer;
          color: @themeColor;
        }
      }
      >p{
        border-bottom: 1px solid #E8E8E8;
        border-top:1px solid #E8E8E8;
        font-size: 16px;
        height: 55px;
        line-height: 55px;
      }
      ul{
        padding: 20px 0;
        li{
          display: inline-block;
          width: 24.5%;
          color: #666666;
          padding: 10px 0;
        }
      }
    }
    .serviceList{
      margin: 10px auto;
      img{
        width: 100%;
      }
    }
  }

</style>
