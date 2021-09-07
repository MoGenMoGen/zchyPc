<template>
<!-- 设计预约——弹窗-->
  <div id="bg"  v-show="show">
    <div  class="mainDesign">
        <div class="head">
          <p>
            <!-- <img src="" alt=""> -->
            <img class="iconZ" :src="marks" alt="">
            <!-- <span>设计预约</span> -->
          </p>
          <img :src="close" @click="toClose" class="point">
        </div>
        <div class="popupDBody">
          <div class="title">
            <img :src="headLeft" alt=""><p>船舶基本参数</p><img :src="headRight" alt="">
          </div>
          <div class="msg">
            <div class="floatL"><span>船舶户籍:</span><input type="text" v-model="form.registry" placeholder="船籍"></div>
            <div class="floatR" style="opacity: 0;"><span>:</span><input type="text" placeholder=""></div>
            <div class="floatL">
              <span><i>*</i>船舶类型:</span>
              <el-select v-model="form.typesCd" placeholder="请选择船舶类型">
                <el-option
                  v-for="item in typeList"
                  :key="item.label"
                  :label="item.nm"
                  :value="item.cd">
                </el-option>
              </el-select>
            </div>
            <div class="floatR"><span>船舶型宽:</span><input type="text" v-model="form.width" placeholder="船舶型宽(m)"></div>
            <div class="floatL">
              <span>船舶航区:</span>
              <el-select v-model="form.sailingAreaCd" placeholder="请选择航区">
                <el-option
                  v-for="item in areaList"
                  :key="item.label"
                  :label="item.nm"
                  :value="item.cd">
                </el-option>
              </el-select>
            </div>
            <div class="floatR"><span>船舶型深:</span><input type="number" v-model="form.deep" placeholder="船舶型深(m)"></div>
            <div class="floatL">
              <span><i>*</i>船舶材质:</span>
              <el-select v-model="form.hullMaterialCd" placeholder="请选择材质">
                <el-option
                  v-for="item in materList"
                  :key="item.label"
                  :label="item.nm"
                  :value="item.cd">
                </el-option>
              </el-select>
            </div>
            <div class="floatR"><span>船舶总吨位:</span><input type="text" v-model="form.tonnage" placeholder="船舶总吨位"></div>
            <div class="floatL"><span><i>*</i>船舶总长:</span><input type="number" v-model="form.totalLen" placeholder="船舶总长(m)"></div>
            <div class="floatR"><span>船舶航速:</span><input type="text" v-model="form.speed" placeholder="船舶航速(kn)"></div>
            <div class="floatL"><span>公约船长:</span><input type="number" v-model="form.pactLen" placeholder="公约船长(m)"></div>
            <div class="floatR"><span>船舶定员:</span><input type="text" v-model="form.complement" placeholder="船舶定员(P)"></div>
          </div>
          <div class="title">
            <img :src="headLeft" alt=""><p>船舶设备参数</p><img :src="headRight" alt="">
          </div>
          <div class="msg">
            <div class="floatL">
              <span>主机品牌:</span>
              <input type="text" v-model="form.hostBrand" placeholder="主机品牌">
            </div>
            <div class="floatR">
              <span>主机功率:</span>
              <el-select v-model="form.hostPower" placeholder="主机功率(kW)">
                <el-option
                  v-for="item in powerList"
                  :key="item.cd"
                  :label="item.nm"
                  :value="item.nm">
                </el-option>
              </el-select>
            </div>
            <div class="floatL"><span>齿轮箱品牌:</span><input v-model="form.gearBrand" type="text" placeholder="齿轮箱品牌"></div>
            <div class="floatR"><span>齿轮箱型号:</span><input v-model="form.gearModel" type="text" placeholder="齿轮箱型号"></div>
            <div class="floatL">
              <span>发电机组品牌:</span>
              <input type="text" v-model="form.electricGeneratorBrand" placeholder="发电机组品牌">
            </div>
            <div class="floatR"><span>发电机功率:</span>
              <el-select v-model="form.electricGeneratorPower" placeholder="发电机功率(kW)">
                <el-option
                  v-for="item in electricList"
                  :key="item.cd"
                  :label="item.nm"
                  :value="item.nm">
                </el-option>
              </el-select>
            </div>
            <div class="floatL"><span>其他要求:</span><input type="text" v-model="form.rmks" placeholder="请输入其他要求"></div>
          </div>
          <div class="designBtnList">
            <button class="cancel point" @click="toClose">取消</button>
            <button class="confirm point" @click="submit">确认</button>
          </div>
        </div>
      </div>
  </div>
</template>

<script>
    import {mapState} from "vuex";
    import close from "@/assets/img/close.png"
    import headRight from "@/assets/img/personal/右.png"
    import headLeft from "@/assets/img/personal/左.png"
    import unchecked from "@/assets/img/unchecked.png"
    import checked from "@/assets/img/checked.png"
    import marks from '@/assets/img/shipDisplay/角标.png'
    import message from "../pages/personal/message";
    export default {
      layout:'person',
        name: "designAppo",
      components: {

      },
      data(){
          return{
            close,
            headRight,
            headLeft,
            unchecked,
            checked,
            marks,
            cd:'',
            engShow:false,        //添加项目工程
            editorShow:false,     //编辑
            deleteShow:false,     //删除
            projectShow:false,    //添加项目
            typeList:[],//船舶类型
            // typeValue:'',//船舶类型选中
            areaList:[],//船舶航区
            // areaValue:'',//船舶航区选中
            materList:[],//船舶材质
            // materValue:'',//船舶材质选中
            powerList:[],//主机功率
            // powerValue:'',//主机功率选中
            electricList:[],//发电机功率
            money:132,
            check:false,
            tableData: [{
              no: '2016-05-02',
              orderNo: '王小虎',
              productName: '上海市普陀区金沙江路 1518 弄',
              productParam: '2016-05-02',
              num: '王小虎',
              realPayment: '上海市普陀区金沙江路 1518 弄',
              status: '2016-05-02',
              }],
            form:{
              registry: "",//船舶户籍
              typesCd: "",//船舶类型
              sailingAreaCd: "",//船舶航区
              hullMaterialCd: "",//船舶材质
              lengthCd: "",//船舶长度
              totalLen: "",//船舶总长
              pactLen: "",//公约船长
              width: "",//船舶型宽
              deep: "",//船舶型深
              tonnage: "",//船舶总吨位
              speed: "",//船舶航速
              complement: "",//定员
              hostBrand: "",//主机品牌
              hostPower: "",//主机功率
              gearBrand: "",//齿轮箱品牌
              gearModel: "",//齿轮箱型号
              electricGeneratorBrand: "",//发动机品牌
              electricGeneratorPower: "",//发动机功率
              rmks:"",
            },
            projectNow:'模具使用费用',
            list:[{nm:11},{nm:11},{nm:11},{nm:11}]
            }
      },
      props: {
        show: {
          type: Boolean,
          default: false
        },
      },
      computed:{
        ...mapState([
          'bWidth',
          'width',
          'loading',
        ])
      },
      mounted() {
        this.getInfo()
        this.getDataDictionary()
      },
      watch:{
        $route(){
          this.cd = this.until.getQueryString('cdType')
        },

      },
      methods: {
        getInfo(){
          this.tableData=[]
          this.list.forEach((item,index)=>{
            this.tableData.push({ no:index+1,
               orderNo: '202007210001',
               productName: '船舶设备名称',
               productParam: 'BL112 ',
               num: index+1,
               realPayment: '¥ 1600',
               status: '待付款'})

          })
        },
        async getDataDictionary(){
          //船舶类型
            this.typeList = await this.api.dataDictionary('SHIP_TYPES')
          //船舶航区
            this.areaList = await this.api.dataDictionary('SAILING_AREA')
          //船舶材质
            this.materList = await this.api.dataDictionary('HULL_MATERIAL')
          //主机功率
            this.powerList = await this.api.dataDictionary('HOST_POWER')
          //主机品牌
          //   this.typeValue = await this.api.dataDictionary(cd)
          //发动机品牌
          //   this.materValue = await this.api.dataDictionary(cd)
          //发电机功率
          this.electricList = await this.api.dataDictionary('ELECTRIC_POWER')

        },
        addAll(type){   //弹窗
          console.log(type)
          if(type=='engineer'){//添加项目工程
            this.engShow=true
          }else if(type=='editor'){//编辑
            this.editorShow=true
          }else if(type=='delete'){//删除
            this.deleteShow=true
          }else if(type=='project'){//添加项目
            this.projectShow=true
          }
        },
        checkEng(){   //选择项目工程
          this.check=!this.check
        },
        toClose(){
          this.$emit('close')
        },
        submit(){
          if(!this.form.typesCd){
            this.$message({
              message: '请选择船舶类型',
              type: 'warning',
              duration:'1500',
              customClass:'mzindex'
            });
            return
          }
          if(!this.form.hullMaterialCd){
            this.$message({
              message: '请选择船舶材质',
              type: 'warning',
              duration:'1500',
              customClass:'mzindex'
            });
            return
          }
          if(!this.form.totalLen){
            this.$message({
              message: '请选择船舶长度',
              type: 'warning',
              duration:'1500',
              customClass:'mzindex'
            });
            return
          }
          this.$emit('submit')
          this.store.commit('changeLoading',true)
          this.api.designAdd(this.form).then(res=>{
            this.$message({
              message: '预约成功',
              type: 'success',
              duration:'1500',
              customClass:'mzindex'
            });
          })
          console.log('this.form=',this.form)
        },
      },
    }
</script>
<style lang="less">
  .mainDesign .msg{
    .el-input--suffix .el-input__inner{
      height: 42px;
      border-radius: 0;
    }
  }
  .mzindex{
          z-index:3000 !important;
   }
</style>
<style lang="less" scoped>
  // @import url("@/assets/css/init.less");
  #bg {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: rgba(0,0,0,.5);
    z-index: 500;
    display: flex;
    justify-content: center;
    align-items: center;

    .mainDesign{
      /*width: 54.8%;*/
      width: 790px;
      height: 80%;
      background-color: #FFFFFF;
      display: flex;
      flex-direction: column;
      border-radius:8px;

      .head{
        display: flex;
        justify-content: space-between;
        align-items: center;
        height: 59px;
        width: 100%;
        padding: 0 24px 0 30px;
        // border-bottom: 1px solid rgba(0,0,0,0.1);;
        >p{
          margin: -12px auto 0;
          width: 37.4%;
        }
        >img{
          width:20px;
          height:20px;
        }
      }
      .popupDBody{
        flex: 1;
        overflow-y: scroll;
        .title{
          display: flex;
          align-items: center;
          justify-content: center;
          // margin-bottom: 60px;
          padding-top: 40px;
          >p{
            font-size:16px;
            color:rgba(51,51,51,1);
            margin: 0 30px;
          }
          >img{
            width: 14.8%;
          }
        }
        .msg{
          // padding: 0 30px;
          padding: 40px 40PX 0;
          overflow: hidden;
          // height: 492px;
          >div{
            width: 45%;
            display: flex;
            align-items: center;
            margin-bottom: 20px;
            >span{
              width: 125px;
              font-size:16px;
              color:rgba(102,102,102,1);
              >i{
                margin-left: -12px;
                margin-right: 5px;
                color: #F56C6C;
              }
            }
            >input{
              width: 78%;
              height:42px;
              border:1px solid rgba(221, 221, 221, 1);

              font-size:14px;
              line-height: 42px;
              padding-left: 20px;
            }
            .el-select{
              width: 78%;
              height:42px;
              // border:1px solid rgba(221, 221, 221, 1);
            }
          }
        }
        .designBtnList{
          padding: 40px 18%;
          display: flex;
          justify-content: space-between;
          >button{
            width: 48%;
            height:42px;
            font-size:16px;
          }
          .cancel{
            background:#FFFFFF;
            border:1px solid rgba(203, 203, 203, 1);
            color:rgba(102,102,102,1);
          }
          .confirm{
            background:rgba(39,120,190,1);
            border: none;
            color:rgba(255,255,255,1);
          }
        }
      }

    }
  }


  input::-webkit-input-placeholder {
    /* Chrome/Opera/Safari */
        color: #999;
  }
  input::-moz-placeholder {
      /* Firefox 19+ */
      color: #999;
  }
  input:-ms-input-placeholder {
      /* IE 10+ */
      color: #999;
  }
  input:-moz-placeholder {
      /* Firefox 18- */
      color: #999;
  }
  .point{
    cursor:pointer;
  }
  .floatL{
    float: left;
  }
  .floatR{
    float: right;
  }
</style>
