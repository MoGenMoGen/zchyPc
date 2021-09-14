<template>
    <!--船舶档案-->
    <div class="main">
      <!--    查看视频预览弹出框-->
      <el-dialog :visible.sync="show" width="2%" :before-close="test" :show-close='closeShow'>

        <div style="position:fixed;top:0;right:0;bottom:0;left:0;margin:auto;height:600px;width:800px;">
          <yunjianzao v-if="show"  ref="videoPlayer"></yunjianzao>
        </div>
          <!--<hik v-if="show" ref="videoPlayer" :cameraIndexCode="code"></hik>-->
        <!--</div>-->
      </el-dialog>
      <div class="line0">
        <tobbar></tobbar>
      </div>

      <div class="line1">
        <div>
          <p  v-for="(item,index) in tab1" :key="item.id" :class="{active: tab1Id==item.id}" @click="chooseTab1(item,index)"> <!--控制显示 v-show="formW!='jiance'||(formW=='jiance'&&index==2)" -->
            <span>{{item.nm}}</span>
          </p>
        </div>
      </div>
      <div class="line2" v-if="!showWeiBao && !showBian">
        <div>
          <p v-for="(item,index) in selectList" :key="item.id" :class="{active2: tab2Id==item.id}" @click="chooseTab2(item,index)"> <!--控制显示 v-show="formW!='jiance'||(formW=='jiance'&&index==3)" -->
            <span>{{item.nm}}</span>
          </p>
          <span class="btm" :style="{left:tabIndex*129+'px'}"></span>
        </div>
      </div>
<!--      变更记录-->
      <div class="line3" v-if="showBian">
        <bian/>
      </div>
<!--      维保记录-->
      <div class="line3" v-if="showWeiBao">
        <weibao/>
      </div>
      <div class="line4">
        <!--基本信息-->
        <base-info1 v-if="tab2Id==20 " :title="designTitle"></base-info1>
        <!--船舶设备-->
        <ship-equip v-if="tab2Id==21 " type="1" ifPerson="true" :id='id' :width="width*0.81-60"></ship-equip>
        <!--船舶检验证书-->
        <certificate v-if="tab2Id==22 "></certificate>
        <!-- 设备及其他证书 -->
        <jianyanjiance v-if="tab2Id==23"></jianyanjiance>
        <!-- 合同列表 -->
        <contract v-if="tab2Id==24"></contract>
      </div>
      <div class="line4">

        <base-info3 v-if="tab2Id==30 " :title="designTitle"></base-info3>
        <jianzaoliucheng v-if="tab2Id==31"></jianzaoliucheng>
        <pinzhibaozhang v-if="tab2Id==32"></pinzhibaozhang>
        <testreport v-if="tab2Id==33"></testreport>
        <!--<yunjianzao v-if="tab2Id==34" ref="videoPlayer" :cameraIndexCode="code"></yunjianzao>-->
      </div>
      <div class="line4">
        <base-info2 v-if="tab2Id==10 " :title="designTitle"></base-info2>
        <fangansheji v-if="tab2Id==11 || tab2Id==12 || tab2Id==13" :title="designTitle" :flowCd="tab2Id"></fangansheji>
      </div>
    </div>

</template>

<script>
  import {mapState} from "vuex";
  const certificate = resolve => require(['@/components/onlineTrading/certificate'],resolve); //检测证书
  const shipEquip = resolve => require(['@/components/onlineTrading/shipEquipTree'],resolve); //设备
    import tobbar from "../../components/person/tobbar";
    import bian from "../../components/person/bian";
    import weibao from "../../components/person/weibao";
    import fangansheji from "../../components/person/fangansheji";
    import baseInfo3 from "../../components/person/baseInfo3";
    import baseInfo2 from "../../components/person/baseInfo2";
    import baseInfo1 from "../../components/person/baseInfo1";
    import yunjianzao from "../../components/person/yunjianzao";
    import jianzaoliucheng from "../../components/person/jianzaoliucheng";
    import pinzhibaozhang from "../../components/person/pinzhibaozhang";
    import testreport from "../../components/person/testreport";
    import jianyanjiance from "../../components/person/jianyanjiance";
    import contract from "../../components/person/contract";
    export default {
        name: "archives",
        layout:'person',
        components:{
          certificate,
          shipEquip,
            tobbar,
            bian,
            weibao,
            fangansheji,
            baseInfo3,
            baseInfo2,
            baseInfo1,
            yunjianzao,
            jianzaoliucheng,
            pinzhibaozhang,
            jianyanjiance,
            testreport,
            contract,
        },
        computed:{
          ...mapState([
            'bWidth',
            'width'
          ]),

        },
        data(){
            return{
                closeShow:false,
                show:false,
                code:'121b6f4e237c4a889df93a060d428fa0',//视频编号
                tab1Id:-1,//第一个标签页选中
                tab2Id:-1,//第二个标签页选中
                tabIndex:0,//第二个标签页底部边框偏移
                selectList:[],//选中列表
                showWeiBao:false,
                showBian:false,
                designTitle:'方案设计',//设计标题
                id:"",
                tab1:[
                    {
                        id:0,
                        nm:'船舶资料',
                    },
                    {
                        id:1,
                        nm:'设计档案',
                    },
                    {
                        id:2,
                        nm:'建造档案',
                    },
                    {
                        id:3,
                        nm:'变更记录',

                    },
                    {
                        id:4,
                        nm:'维保记录',
                    },
                ],
                tab21:[
                    {
                        id:10,
                        nm:'基本信息',
                    },
                    {
                        id:11,
                        nm:'方案设计',
                    },
                    {
                        id:12,
                        nm:'送审设计',
                    },
                    {
                        id:13,
                        nm:'完工设计',
                    },
                ],
                tab22:[
                    {
                        id:20,
                        nm:'基本信息',
                    },
                    {
                        id:21,
                        nm:'船舶设备',
                    },
                    {
                        id:22,
                        nm:'船舶检验证书',
                    },
                    {
                        id:23,
                        nm:'设备及其他证书',
                    },
                    {
                        id:24,
                        nm:'合同列表',
                    },
                ],
                tab23:[
                    {
                        id:30,
                        nm:'基本信息',
                    },
                    {
                        id:31,
                        nm:'建造过程',
                    },
                    {
                        id:32,
                        nm:'建造监理',
                    },
                    {
                        id:33,
                        nm:'检测报告',
                    },
                    {
                        id:34,
                        nm:'云监造',
                    },
                ],
                formW:"",
            }
        },

        mounted(){
          this.formW = this.until.getQueryString('formW')
          this.id= this.until.getQueryString('id')
          console.log(this.id)
          //控制显示
          // if(this.formW=='jiance'){
          //   this.chooseTab1(this.tab1[2],2)
          // }else{
          //   this.chooseTab1(this.tab1[0],0)
          // }
          this.chooseTab1(this.tab1[0],0)

        },

        methods:{

          test() {
            console.log('关闭了')
            this.$refs.videoPlayer.off();
            this.show = false;

          },
            chooseTab1(item,index){
                if(item.id!=this.tab1Id){
                    this.tab1Id=item.id
                    if(index==0){
                        this.selectList=this.tab22
                        this.showBian=false
                        this.showWeiBao=false
                        this.chooseTab2(this.selectList[0],0)
                    }else if(index==1){
                        this.selectList=this.tab21
                        this.chooseTab2(this.selectList[0],0)
                        this.showBian=false
                        this.showWeiBao=false
                    }else if(index==2){
                        this.selectList=this.tab23
                        //控制显示
                        // if(this.formW=='jiance'){
                        //   this.chooseTab2(this.selectList[3],3)
                        // }else{
                        //   this.chooseTab2(this.selectList[0],0)
                        // }
                        this.chooseTab2(this.selectList[0],0)

                        this.showBian=false
                        this.showWeiBao=false
                    }else if(index==3){
                      this.tab2Id=0
                      this.showWeiBao=false
                      this.showBian=true
                    }else if(index==4){
                      this.tab2Id=0
                      this.showBian=false
                      this.showWeiBao=true
                    }

                }
            },
            chooseTab2(item,index){
              if(item.id==34){
                // this.tab2Id=item.id
                // this.show = true
                // console.log(this.$refs)
                // this.$refs.videoPlayer.getData()
                window.open('http://visual.ship88.cn/bigScreen/index3.html?id='+this.id)
              }else if(item.id!=this.tab2Id){
                    this.tab2Id=item.id
                // this.$refs.videoPlayer.off();

                    //控制显示
                    // this.tabIndex=this.formW=='jiance'?0:index
                    this.tabIndex=index

                    this.designTitle=item.nm

                }
            },
        },
    }
</script>

<style scoped lang="less">
  .main{
    width: 100%;
    /deep/.el-dialog{
      box-shadow: none;
    }
    /deep/.el-dialog, .el-pager li{
      background: none;
    }
    .line0{
      padding: 0 30px;
    }
    .line1{
      width: 100%;
      padding: 0 30px;
      div{

        display: flex;
        width: 100%;
        background: #F7F7F7;
        height: 40px;
        align-items: center;
        p{
          width: 129px;
          display: flex;
          align-items: center;
          justify-content: center;
          height: 100%;
          /*对所有属性过渡0.2s*/
          transition: all 0.2s;
          &:hover{
            cursor: pointer;
          }
        }
      }

    }
    .line2{
      padding: 0 30px;
      margin-top: 15px;
      width: 100%;

      div{

        border-bottom:1px solid rgba(0,0,0,0.1);
        display: flex;
        height: 42px;
        align-items: center;
        position: relative;
        p{
          width: 129px;
          display: flex;
          align-items: center;
          justify-content: center;
          height: 100%;
          /*对所有属性过渡0.2s*/
          &:hover{
            cursor: pointer;
          }
        }
        .btm{
          position: absolute;
          width: 129px;
          height: 100%;
          border-bottom: 2px solid #2778BE;
          /*对left属性过渡5秒*/
          transition: left 0.5s;
        }
      }


    }
    .line3{
      margin-top: 30px;
    }
    .line4{
      margin-top: 30px;
      padding: 0 30px;
    }
  }

  .active{
    background: #2778BE;
    color: white;
  }
  .active2{
    color: #2572B5;
    /*border-bottom: 2px solid #2572B5;*/
  }

</style>
