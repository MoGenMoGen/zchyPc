<template>
  <!-- DIY报价 -->
  <div id="home" :style="{width:bWidth + 'px'}">
    <div class="header">
      <img :height="bannerHeight+'px'" class="adert" alt="" src="../../assets/img/shipDisplay/船舶-DIY-报价.png" :style="{width:bWidth + 'px'}"/>
      <el-breadcrumb separator-class="el-icon-arrow-right" :style="{width:width+'px'}" class="bc main">
        <el-breadcrumb-item :to="{ path: './shipMan' }">船舶管理</el-breadcrumb-item>
        <el-breadcrumb-item v-if="tab==2" :to="{ path: './shipDesign' }">船舶设计</el-breadcrumb-item>
        <el-breadcrumb-item v-if="tab==1" :to="{ path: './shipBuild' }">船舶建造</el-breadcrumb-item>
        <el-breadcrumb-item ><p class="textRed">DIY报价</p></el-breadcrumb-item>
      </el-breadcrumb>
    </div>
    <div class="shipDIYMain" :style="{width:width + 'px'}">
      <p>船舶DIY报价</p>
      <div class="diyTitle">
        <img :src="leftIc"><span>基本参数</span><img :src="rightIc">
      </div>
      <el-form :rules="rules" ref="form" :model="form" label-width="80px">
        <el-form-item prop="typesCd" label="船舶类型:">
          <el-select v-model="form.typesCd" placeholder="请选择船舶类型">
            <el-option
              v-for="(item,index) in typeList"
              :key="index"
              :label="item.nm"
              :value="item.cd">
            </el-option>
          </el-select>
        </el-form-item>
        <el-form-item prop="lengthCd" label="公约船长:">
          <el-select v-model="form.lengthCd" placeholder="公约船长(m)">
            <el-option
              v-for="(item,index) in lengthList"
              :key="index"
              :label="item.nm"
              :value="item.cd">
            </el-option>
          </el-select>
        </el-form-item>
        <el-form-item prop="hullMaterialCd" label="船舶材质:">
          <el-select v-model="form.hullMaterialCd" placeholder="请选择材质">
            <el-option
              v-for="(item,index) in materialList"
              :key="index"
              :label="item.nm"
              :value="item.cd">
            </el-option>
          </el-select>
        </el-form-item>
        <el-form-item label="船舶户籍:">
          <el-input v-model="form.registry" placeholder="船籍"></el-input>
        </el-form-item>
        <el-form-item label="船舶航区:">
          <el-select v-model="form.sailingAreaCd" placeholder="请选择航区">
            <el-option
              v-for="(item,index) in areaList"
              :key="index"
              :label="item.nm"
              :value="item.cd">
            </el-option>
          </el-select>
        </el-form-item>
        <el-form-item label="船舶定员:">
          <el-input v-model="form.complement" placeholder="船舶定员(P)"></el-input>
        </el-form-item>
        <el-form-item label="船舶总长:">
          <el-input v-model="form.totalLen" placeholder="船舶总长(m)"></el-input>
        </el-form-item>
        <el-form-item label="船舶型宽:">
          <el-input v-model="form.width" placeholder="船舶型宽(m)"></el-input>
        </el-form-item>
        <el-form-item label="船舶型深:">
          <el-input v-model="form.deep" placeholder="船舶型深(m)"></el-input>
        </el-form-item>
        <el-form-item label="船舶总吨位:">
          <el-input v-model="form.tonnage" placeholder="船舶总吨位"></el-input>
        </el-form-item>
        <el-form-item label="船舶航速:">
          <el-input v-model="form.speed" placeholder="船舶航速(kn)"></el-input>
        </el-form-item>
        <el-form-item label="冷冻系统:">
          <el-select v-model="form.freezingSys" placeholder="冷冻系统有无">
            <el-option
              v-for="(item,index) in freeList"
              :key="index"
              :label="item.nm"
              :value="item.cd">
            </el-option>
          </el-select>
        </el-form-item>
        <el-form-item label="主机品牌:">
          <el-select v-model="form.hostBrandId" placeholder="选择主机品牌">
            <el-option
              v-for="(item,index) in hostList"
              :key="index"
              :label="item.nm"
              :value="item.id">
            </el-option>
          </el-select>
        </el-form-item>
        <el-form-item label="制冷机组品牌:">
          <el-select v-model="form.refrigeratorBrandId" placeholder="选择制冷机组品牌">
            <el-option
              v-for="(item,index) in refrigeratorList"
              :key="index"
              :label="item.nm"
              :value="item.id">
            </el-option>
          </el-select>
        </el-form-item>
        <el-form-item label="发电机组品牌:">
          <el-select v-model="form.electricGeneratorBrandId" placeholder="选择发电机组品牌">
            <el-option
              v-for="(item,index) in electricList"
              :key="index"
              :label="item.nm"
              :value="item.id">
            </el-option>
          </el-select>
        </el-form-item>
        <el-form-item label="主机功率:">
          <el-select v-model="form.hostPowerCd" placeholder="选择主机功率(kW)">
            <el-option
              v-for="(item,index) in hostPList"
              :key="index"
              :label="item.nm"
              :value="item.cd">
            </el-option>
          </el-select>
        </el-form-item>
        <el-form-item label="制冷机组功率:">
          <el-select v-model="form.refrigeratorPowerCd" placeholder="选择制冷机组功率(kW)">
            <el-option
              v-for="(item,index) in refrigeratorPList"
              :key="index"
              :label="item.nm"
              :value="item.cd">
            </el-option>
          </el-select>
        </el-form-item>
        <el-form-item label="发电机功率:">
          <el-select v-model="form.electricGeneratorPowerCd" placeholder="选择发电机功率(kW)">
            <el-option
              v-for="(item,index) in electricPList"
              :key="index"
              :label="item.nm"
              :value="item.cd">
            </el-option>
          </el-select>
        </el-form-item>
        <!--<el-form-item label="发电机:">-->
          <!--<el-select v-model="parmaInfo.motoBrandCd"-->
                     <!--placeholder="选择发电机"-->
                     <!--clearable-->
                     <!--@change="changePamra()">-->
            <!--<el-option-->
              <!--v-for="(item,index) in fdjList"-->
              <!--:key="index"-->
              <!--:label="item.nm"-->
              <!--:disabled="item.disabled"-->
              <!--:value="item.cd">-->
            <!--</el-option>-->
          <!--</el-select>-->
        <!--</el-form-item>-->
        <el-form-item label="船体安全级别/配置档次/发电机/航速:" style="width: 64.6%;height: auto;margin: 0;" class="form">
          <el-select v-model="parmaInfo.safeLevelCd"
                     placeholder="选择船体安全级别/配置档次/发电机/航速"
                     clearable
                     @change="changePamra">
            <el-option
              v-for="(item,index) in levelList"
              :key="index"
              :label="item.safeLevelNm +'/'+item.confLevelNm+'/'+item.motoBrandNm+'/'+item.speedLevelNm"
              :value="item.id">
              <span style="float: left">{{ item.safeLevelNm }}/{{item.confLevelNm}}/{{item.motoBrandNm}}/{{item.speedLevelNm}}</span>
            </el-option>
          </el-select>
        </el-form-item>
        <!--<el-form-item label="配置档次:">-->
          <!--<el-select v-model="parmaInfo.confLevelCd"-->
                     <!--placeholder="选择配置档次"-->
                     <!--clearable-->
                     <!--@change="changePamra()">-->
            <!--<el-option-->
              <!--v-for="(item,index) in pzdcList"-->
              <!--:key="index"-->
              <!--:label="item.nm"-->
              <!--:disabled="item.disabled"-->
              <!--:value="item.cd">-->
            <!--</el-option>-->
          <!--</el-select>-->
        <!--</el-form-item>-->

        <!--<el-form-item label="航速:">-->
          <!--<el-select v-model="parmaInfo.speedLevelCd"-->
                     <!--placeholder="选择发航速"-->
                     <!--clearable-->
                     <!--@change="changePamra()">-->
            <!--<el-option-->
              <!--v-for="(item,index) in hsList"-->
              <!--:key="index"-->
              <!--:label="item.nm"-->
              <!--:disabled="item.disabled"-->
              <!--:value="item.cd">-->
            <!--</el-option>-->
          <!--</el-select>-->
        <!--</el-form-item>-->
        <el-form-item label="其他要求:" style="width: 64.6%;height: auto;margin: 0;">
          <el-input type="textarea" v-model="form.intro" placeholder="其他特殊要求"></el-input>
        </el-form-item>
      </el-form>
      <div class="diyBtn">
        <p>整船参考价格:<span>¥ {{money}} </span> 万元起 </p>
        <button class="reset point" @click="reset">重置</button>
        <button class="confirm point" @click="toSubmit">确认</button>
      </div>
    </div>
    <div class="footerImg" :style="{width:bWidth+'px'}">
      <img src="../../assets/img/footer.png" alt=""/>
    </div>
  </div>
</template>

<script>
    import RuleMenu from "./components/ruleMenu";
    import {mapState} from "vuex";
    import leftIc from '@/assets/img/personal/left.png'
    import rightIc from '@/assets/img/personal/right.png'
    export default {
        name: "buildRule",
      data(){
          return{
            leftIc,
            rightIc,
            bannerHeight:450,
            money:0,
            tab:1,
            form:{
              coefId:'',
              typesCd:'',
              hullMaterialCd:'',
              lengthCd:'',
              registry:'',
              sailingAreaCd:'',
              complement:'',
              width:'',
              deep:'',
              pactLen:'',
              tonnage:'',
              speed:'',
              freezingSys:'',
              hostBrandId:'',
              refrigeratorBrandId:'',
              electricGeneratorBrandId:'',
              intro:'',
              hostPowerCd:'',
              refrigeratorPowerCd:'',
              electricGeneratorPowerCd:''
            },
            rules: {
              typesCd: [
                { required: true, message: '请选择船舶类型', trigger: 'blur' },
              ],
              hullMaterialCd: [
                { required: true, message: '请选择材质', trigger: 'blur' },
              ],
              lengthCd: [
                { required: true, message: '请选择船舶总长', trigger: 'blur' },
              ],
            },
            typeList:[],          //船舶类型
            materialList:[],      //船舶材质
            lengthList:[],        //船舶总长
            areaList:[],          //船舶航区
            freeList:[{cd:'1',nm:'有'},{cd:'0',nm:'没有'}],          //冷冻系统
            hostList:[],          //主机品牌
            hostPList:[],         //主机功率
            refrigeratorList:[],  //制冷机组品牌
            refrigeratorPList:[], //制冷机组功率
            electricList:[],      //发电机组品牌
            electricPList:[],     //发电机功率
            levelList:[],
            // ctaqjbList:[],//船体安全级别
            // pzdcList:[],//配置档次
            // fdjList:[],//发电机
            // hsList:[],//航速
            parmaInfo:{
              safeLevelCd:'',
              confLevelCd:'',
              motoBrandCd:'',
              speedLevelCd:''
            },
            ifChange:true,
          }
      },
      components: {RuleMenu},
      computed: {
        ...mapState([
          'bWidth',
          'width',
          'currentRole',
          'ifLogin'
        ])
      },

      mounted() {
        this.tab=this.until.getQueryString('tab')
        this.getCd()
      },
      methods:{

        changePamra(e){

          console.log(e)
          let info = this.levelList.find(item=>item.id==e)
          let param = {
            safeLevelCd:info.safeLevelCd,
            confLevelCd:info.confLevelCd,
            motoBrandCd:info.motoBrandCd,
            speedLevelCd:info.speedLevelCd
          }
          this.api.diyParamDetail(param).then(res=>{
            if(res){
              this.form.coefId = res.id
            }
          })

        },
        async getCd(){ //获取下拉框数据
          this.typeList=await this.api.dataDictionary('SHIP_TYPES')
          this.materialList=await this.api.dataDictionary('HULL_MATERIAL')
          this.lengthList=await this.api.dataDictionary('SHIP_LENGTH')
          this.areaList=await this.api.dataDictionary('SAILING_AREA')
          // this.ctaqjbList = await this.api.dataDictionary('SAFE_LEVEL')
          // this.pzdcList = await this.api.dataDictionary('CONF_LEVEL')
          // this.fdjList = await this.api.dataDictionary('MOTO_BRAND_LEVEL')
          // this.hsList = await this.api.dataDictionary('SPEED_LEVEL')

          this.hostList=await this.api.listByCat({catId:'5021608050136064'})

          this.refrigeratorList=await this.api.listByCat({catId:'5021604766159872'})

          this.electricList=await this.api.listByCat({catId:'5021608555484160'})

          this.hostPList=await this.api.dataDictionary('HOST_POWER')
          this.refrigeratorPList=await this.api.dataDictionary('REFRIGERATOR_POWER')
          this.electricPList=await this.api.dataDictionary('ELECTRIC_POWER')

          let qry = this.query.new()
          this.levelList = await this.api.diyParamList(this.query.toEncode(qry))
        },
        reset(){
          this.form.typesCd=''
          this.form.hullMaterialCd=''
          this.form.lengthCd=''
          this.form.registry=''
          this.form.sailingAreaCd=''
          this.form.complement=''
          this.form.width=''
          this.form.deep=''
          this.form.pactLen=''
          this.form.tonnage=''
          this.form.speed=''
          this.form.freezingSys='0'
          this.form.hostBrandId=''
          this.form.refrigeratorBrandId=''
          this.form.electricGeneratorPowerId=''
          this.form.intro=''
          this.form.hostPowerCd=''
          this.form.refrigeratorPowerCd=''
          this.form.electricGeneratorPowerCd=''

        },
        toSubmit(){
          if(!this.form.typesCd){
            this.$message({
              message: '请选择船舶类型',
              type: 'warning',
              duration:'1500'
            });
            return
          }
          if(!this.form.hullMaterialCd){
            this.$message({
              message: '请选择材质',
              type: 'warning',
              duration:'1500'
            });
            return
          }
          if(!this.form.lengthCd){
            this.$message({
              message: '请选择船舶总长',
              type: 'warning',
              duration:'1500'
            });
            return
          }
          this.api.diyOffer(this.form) .then((res) => {
            this.$alert('整船参考价格:¥ '+res+' 万元起', '提示', {
              confirmButtonText: '确定',
            });
            this.money = res;
            this.money = this.until.formatNumberRgx(this.money)
            console.log(res)
          })
        },
      }
    }

</script>
<style lang="less">
  .shipDIYMain{

    .el-form{
      display: flex;
      flex-wrap: wrap;
      .el-form-item{
        width: 31.3%;
        /*height: 52px;*/
        height: 42px;
        margin-right: 2%;
        display: flex;
        margin-bottom: 20px;
        .el-form-item__label{
          width: 100px !important;
          line-height: 52px;
          padding: 0!important;
        }
        .el-form-item__content{
          margin-left: 10px !important;
          flex: 1;
          .el-select{
            width: 100%;
          }
          .el-input__inner{
            /*height: 52px;*/
            height: 42px;
            border-radius: 0;
          }
          .el-textarea{
            textarea{
              height: 134px;
              resize: none;
              border-radius:0;
            }
          }
        }
      }
      .form{
        .el-form-item__label{
          width: 250px !important;
          line-height: 52px;
        }
      }
      // .el-form-item:nth-child(3n){
      //   margin-right:0;
      // }
    }
  }
</style>
<style lang="less" scoped>
  @import url("../../assets/css/init.less");
    #home{
      background-color: #FFFFFF;
      display: flex;
      flex-wrap: wrap;
      .textRed{
        color: @themeColor;
      }
      .bc{
        height: 54px;
        display: flex;
        align-items: center;
      }
    }
    .header{
      width: 100%;
    }
    .shipDIYMain{
      margin: 0 auto;
      >p{
        width: 100%;
        text-align: center;
        border-bottom: 1px solid rgba(223,223,223,1);;
        font-size:22px;
        line-height: 30px;
        font-weight:500;
        color:rgba(48,48,48,1);
        padding: 30px 0 20px;
      }
      .diyTitle{
        text-align: center;
        padding: 30px 0 30px;
        >span{
          font-size:18px;
          margin: 0 40px;
        }
        >img{
          height:19px;
        }
      }
      .diyBtn{
        width: 100%;
        padding: 61px 39px 10px 32px;
        height: 52px;
        display: flex;
        justify-content: space-between;
        >p{
          width: 66%;
          height: 42px;
          line-height: 42px;
          font-size:14px;
          background:rgba(243,243,243,1);
          color:rgba(102,102,102,1);
          padding-left: 26px;
          >span{
            margin-left: 17px;
            font-size:24px;
            color:rgba(230,67,71,1);
          }
        }
        >button{
          height: 42px;
          font-size: 16px;
        }
        .reset{
          width: 14%;
          background:#FFFFFF;
          border:1px solid rgba(39, 120, 190, 1);
          color:rgba(39,120,190,1);
        }
        .confirm{
          width: 14%;
          background:rgba(39,120,190,1);
          border: none;
          color: #FFFFFF;
        }
      }
    }
    .footerImg{
      width: 100%;
      margin-top: 53px;
    }
    .point{
      cursor: pointer;
    }
</style>
