<template>
<!--  船舶档案详情-->
<div class="detail">
  <!--    查看视频预览弹出框-->
  <el-dialog :visible.sync="show" width="2%" :before-close="test" :show-close='closeShow'>

    <div style="position:fixed;top:0;right:0;bottom:0;left:0;margin:auto;height:600px;width:800px;">
      <yunjianzao v-if="show"  ref="videoPlayer"></yunjianzao>
    </div>
    <!--<hik v-if="show" ref="videoPlayer" :cameraIndexCode="code"></hik>-->
    <!--</div>-->
  </el-dialog>
  <div class="head">
    <p><span class="lineC"></span>船舶档案</p>
    <span @click="back" class="point">< 返回</span>
  </div>
  <div class="mainFileDetail">
     <div class="tab">
       <p @click="tabId=1" :class="{clickP:tabId==1}" class="point">船舶资料</p> <!-- 控制显示 v-show="formW!='jiance'" -->
       <p @click="tabId=2" :class="{clickP:tabId==2}" class="point">设计档案</p><!-- 控制显示 v-show="formW!='jiance'" -->
       <p @click="tabId=3" :class="{clickP:tabId==3}" class="point">建造档案</p>
       <p @click="tabId=4" :class="{clickP:tabId==4}" class="point">变更记录</p><!-- 控制显示 v-show="formW!='jiance'" -->
       <p @click="tabId=5" :class="{clickP:tabId==5}" class="point">维保记录</p><!-- 控制显示 v-show="formW!='jiance'" -->
     </div>
    <div class="line2" v-if="tabId==3">
      <div>
        <p v-for="(item,index) in selectList" :key="item.id" :class="{active2: tab2Id==item.id}" @click="chooseTab2(item,index)"> <!-- 控制显示 v-show="formW!='jiance'||(formW=='jiance'&&index==3)" -->
          <span>{{item.nm}}</span>
        </p>
        <span class="btm" :style="{left:tabIndex*129+'px'}"></span>
      </div>
    </div>
    <div class="line2" v-if="tabId==2 && formW!='design'">
      <div>
        <p v-for="(item,index) in tab22" :key="item.id"  :class="{active2: tab2Id==item.id}" @click="chooseTab2(item,index)">
          <span>{{item.nm}}</span>
        </p>
        <span class="btm" :style="{left:tabIndex*129+'px'}"></span>
      </div>
    </div>
    <div class="line2" v-if="tabId==1">
      <div>
        <p v-for="(item,index) in tab21" :key="item.id"  :class="{active2: tab2Id==item.id}" @click="chooseTab2(item,index)">
          <span>{{item.nm}}</span>
        </p>
        <span class="btm" :style="{left:tabIndex*129+'px'}"></span>
      </div>
    </div>

     <div class="line4" v-if="tabId==1">
       <base-info1 v-if="tab2Id==20 " :title="designTitle"></base-info1>
       <ship-equip v-if="tab2Id==21"  :id="id" :width="shipEquipWidth" type="1"></ship-equip>
       <certificate v-if="tab2Id==22"></certificate>
       <jianyanjiance v-if="tab2Id==23 && formW!='jiance'"></jianyanjiance>
       <inspection v-if="tab2Id==23 && formW=='jiance'"></inspection>
     </div>

     <design-file v-if="tabId==2 && formW=='design'"></design-file>

    <div class="line4" v-if="tabId==2 && formW!='design'">
      <base-info2 v-if="tab2Id==10" :title="designTitle"></base-info2>
      <fangansheji v-if="tab2Id==11 || tab2Id==12 || tab2Id==13" :title="designTitle"></fangansheji>
    </div>
     <div class="line4" v-if="tabId==3">
       <base-info3 v-if="tab2Id==30 " :title="designTitle"></base-info3> <!-- 控制显示 && formW!='jiance' -->
       <jianzaoliucheng v-if="tab2Id==31 && formW!='build'"></jianzaoliucheng> <!-- 控制显示 && formW!='jiance' -->
       <build-process v-if="tab2Id==31 && formW=='build'"></build-process>
       <pinzhibaozhang v-if="tab2Id==32 && formW!='jianli'" ></pinzhibaozhang> <!-- 控制显示 && formW!='jiance' -->
       <qua-guar v-if="tab2Id==32 && formW=='jianli'" ></qua-guar> <!--品质保障编辑-->
       <testreport v-if="tab2Id==33 && formW!='jiance'"></testreport>
       <testreportedit v-if="tab2Id==33 && formW=='jiance'"></testreportedit>
       <yunjianzao v-if="tab2Id==34"></yunjianzao>
     </div>
     <bian v-if="tabId==4"></bian>
     <weibao v-if="tabId==5"></weibao>
  </div>
</div>
</template>

<script>
    import {mapState} from "vuex";
    const certificate = resolve => require(['@/components/onlineTrading/certificate'],resolve); //检测证书
    const shipEquip = resolve => require(['@/components/onlineTrading/shipEquipTree'],resolve); //设备
    const baseInfo1 = resolve => require(['@/components/person/baseInfo1'],resolve); //基本信息
    const baseInfo2 = resolve => require(['@/components/person/baseInfo2'],resolve); //基本信息
    const baseInfo3 = resolve => require(['@/components/person/baseInfo3'],resolve); //基本信息
    const bian = resolve => require(['@/components/person/bian'],resolve); //变更记录
    const weibao = resolve => require(['@/components/person/weibao'],resolve); //维保
    const yunjianzao = resolve => require(['@/components/person/yunjianzao'],resolve); //云监造
    const fangansheji = resolve => require(['@/components/person/fangansheji'],resolve); //方案设计
    const jianzaoliucheng = resolve => require(['@/components/person/jianzaoliucheng'],resolve); //建造流程(查看)
    const buildProcess = resolve => require(['@/components/person/buildProcess'],resolve); //建造流程(编辑)
    const pinzhibaozhang = resolve => require(['@/components/person/pinzhibaozhang'],resolve); //品质保障(查看)
    const quaGuar = resolve => require(['@/components/person/quaGuar'],resolve); //品质保障(编辑)
    const jianyanjiance = resolve => require(['@/components/person/jianyanjiance'],resolve); //检验检测(查看)
    const inspection = resolve => require(['@/components/person/inspection'],resolve); //检验检测(编辑)
    const testreport = resolve => require(['@/components/person/testreport'],resolve); //检测报告(查看)
    const testreportedit = resolve => require(['@/components/person/testreportedit'],resolve); //检测报告(编辑)
    const pageInation = resolve => require(['@/components/pageInation'],resolve); //
    const designFile = resolve => require(['@/components/person/designFile'],resolve); //设计流程 新增（修改）
    import excel from '@/assets/img/personal/excel.png'
    import ppt from '@/assets/img/personal/ppt.png'
    import word from '@/assets/img/personal/word.png'
    import pdf from '@/assets/img/personal/pdf.jpg'
    import del from '@/assets/img/personal/del.png'
    export default {
      layout:'person',
        name: "shipFile",
      components: {
        pageInation,designFile,bian,
        weibao,jianyanjiance,inspection,
        jianzaoliucheng,
        buildProcess,
        fangansheji,
        baseInfo1,
        baseInfo2,quaGuar,
        baseInfo3,yunjianzao,pinzhibaozhang,
        shipEquip,certificate,testreport,testreportedit
      },
      data(){
          return{
            closeShow:false,
            show:false,
            excel,
            id:'',
            ppt,
            word,
            pdf,
            del,
            cd:'',
            tabId:1,
            tab2Id:'',
            tabIndex:0,
            designTitle:'',
            flag:0,
            formW:'',          //判断从设计院身份点击进入还是船厂
            currentPage1: 1,   //前往哪页
            pageSize:4,        //每页数量
            total:33,          //总数
            shipEquipWidth:'',
            tab21:[
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
              },{
                id:23,
                nm:'设备及其他证书',
              }
            ],
            tab22:[
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
            selectList:[
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
              },{
                id:33,
                nm:'检测报告',
              },{
                id:34,
                nm:'云监造',
              },
            ]

          }
      },
      computed:{
        ...mapState([
          'bWidth',
          'width'
        ])
      },
      mounted() {
        this.shipEquipWidth = this.width*0.81-80
        this.id= this.until.getQueryString('id')
         this.tabId = this.until.getQueryString('cdType') ? this.until.getQueryString('cdType') : 1
         this.tab2Id = this.until.getQueryString('cdType2') ? this.until.getQueryString('cdType2') : ''
         console.log(this.tab2Id)
         this.formW = this.until.getQueryString('formW')
      },
      watch:{
        $route(){
          this.cd = this.until.getQueryString('cdType')
        },
        tabId(){
          console.log(this.tabId)
          if(this.tabId==1){
            this.tab2Id = '20'
            this.tabIndex = this.tab21.findIndex(item=>item.id==this.tab2Id)
          }else if(this.tabId==2){
            this.tab2Id='10'
            this.tabIndex = this.tab22.findIndex(item=>item.id==this.tab2Id)
          }else if(this.tabId==3){
            console.log('===============')
            if(this.formW=='jiance'){
              this.tab2Id = '33'
            }else{
              this.tab2Id = '30'
            }
            this.tabIndex = this.selectList.findIndex(item=>item.id==this.tab2Id)

          }else{
            this.tab2Id = ''
          }
          return
          //控制显示 只留 this.tabIndex=0
          if(this.formW=='jiance'){
              this.tabIndex=3
          }else{
              this.tabIndex=0
          }
          console.log('tabIndex1:'+this.tabIndex)

          if(this.flag==0 && this.until.getQueryString('cdType2')){
            console.log('22222222222')
            this.tab2Id = this.tabId+ this.until.getQueryString('cdType2')
            if(this.formW=='jiance'){
              this.tabIndex=0
            }else{
              this.tabIndex=this.until.getQueryString('cdType2')
            }

            this.flag++
          }

          console.log('tabIndex2:'+this.tabIndex)
        },
      },
      methods: {
        //关闭监控弹窗
        test() {
          console.log('关闭了')
          this.$refs.videoPlayer.off();
          this.show = false;

        },
        back(){//返回
			this.$router.back()
        },
        chooseTab2(item,index){
          console.log(item)
          if(item.id==34){
            window.open('http://visual.ship88.cn/bigScreen/index3.html?id='+this.id)
          }else if(item.id!=this.tab2Id){
            this.tab2Id=item.id
            this.tabIndex=index
            this.designTitle=item.nm
          }
        },
      },
    }
</script>
<style lang="less">
  .status .el-input__inner{
          height: 36px;
          line-height: 36px;
  }
  .tableOrder{
    .el-table__header thead{
      color: #666666;
      font-size:16px;
      font-weight:400;
    }
    .el-table__header th{
      background-color: #F7F7F7;

    }
    .el-table__body tr{
      color: #333333;
      font-size:15px;
    }
    .el-table__fixed-right{
      height: 100%!important;
    }
  }
  .main .content .right{
    .el-date-editor.el-input{
      width: 59.5%;

    }
    .el-input--prefix .el-input__inner{
      height: 52px;
    }
  }
</style>
<style lang="less" scoped>
  @import url("../../assets/css/init.less");
  .point{
    cursor:pointer;
  }
  .detail{
    /deep/.el-dialog{
      box-shadow: none;
    }
    /deep/.el-dialog, .el-pager li{
      background: none;
    }
  }
  .head{
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin: 30px 0;
    width: 100%;
    padding: 0 24px 0 30px;
    // border-bottom: 1px solid rgba(0,0,0,0.1);;
    >p{
      display: flex;
      align-items: center;
      font-size:20px;
      font-family:Microsoft YaHei;
      font-weight:400;
      color:#333333;
      height: 18px;
      .lineC{
        display: block;
        height: 18px;
        width: 3px;
        background-color: #2778BE;
        margin-right: 8px;
      }
    }
    >span{
      display: block;
      height:12px;
      font-size:14px;
      font-family:Microsoft YaHei;
      font-weight:400;
      color: #666666;
    }
  }
  .mainFileDetail{
    width: 100%;
    .line4{
      margin-top: 30px;
      padding: 0 30px;
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
    .tab{
      margin: 0 36px 20px 30px;
      border-bottom: 1px solid #2778BE;
      background: #F7F7F7;
      height: 40px;
      display: flex;
      >p{
        display: flex;
        justify-content: center;
        align-items: center;
        width:14.1%;
        height: 100%;
        color: #666666;
        font-size:14px;
        font-family:Microsoft YaHei;
        font-weight:400;
      }
      .clickP{
        background-color: #2778BE;
        color: #FFFFFF;
      }
    }
    .nav{
      display: flex;
      margin: 17px 36px 0px 30px;
      height: 40px;
      border-bottom:  1px solid rgba(0,0,0,0.1);
      >p{
        display: flex;
        justify-content: center;
        align-items: center;
        width:14.3%;
        height: 100%;
        color: #666666;
        font-size:14px;
        font-family:Microsoft YaHei;
        font-weight:400;
      }
      .clickP{
        color: #2778BE;
        border-bottom:  2px solid #2778BE;
      }
    }
    .main{
      width: 100%;
      >div{
         width: 100%;
         padding: 30px 30px 0;
        >p{
          // margin: 30px 30px 0;
          height:40px;
          background:rgba(247,247,247,1);
          border:1px solid rgba(238, 238, 238, 1);
          // padding:14px 0 14px 28px;
          padding-left: 28px;
          font-size:14px;
          font-family:Microsoft YaHei;
          font-weight:bold;
          color:#666666;
          line-height:40px;
        }
        .content{
          display: flex;
          padding-bottom: 30px;
          width: 100%;
          font-family:Microsoft YaHei;
          font-weight:400;
          >div{
            width: 50%;
            padding-top: 30px;
          }
          .left{
            display: flex;
            .left-title{
              padding-top: 19px;
              font-size:15px;
              color:rgba(102,102,102,1);
            }
            .left-msg{
              width: 59.5%;
              >textarea{
                width: 100%;
                height:165px;
                padding: 20px 21px 0;
                background:rgba(255,255,255,1);
                border:1px solid rgba(221, 221, 221, 1);
                resize: none;
              }
              >button{
                width: 100%;
                height:52px;
                background:rgba(39,120,190,1);
                border: none;
                margin-top: 59px;
                font-size:16px;
                color:rgba(255,255,255,1);
                line-height:52px;
              }
            }
          }
          .right{
            display: flex;
            flex-direction: column;
            >div{
              display: flex;
              justify-content: center;
              // align-items: center;
              margin-bottom: 28px;
              >p{
                margin-top: 20px;
                margin-right: 28px;
              }
              .imgBox{
                width: 59.5%;
                .img{
                  //overflow: hidden;
                  width: 100%;
                  >div{
                    width:30%;
                    // width: 85px;
                    height: 85px;
                    margin-right: 3%;
                    margin-bottom: 10px;
                    float: left;
                    position: relative;
                    img{
                      width: 100%;
                      height: 100%;
                    }
                    .delImg{
                      cursor: pointer;
                      position: absolute;
                      top: -7.5px;
                      right: -7.5px;
                      z-index: 2;
                      width: 15px;
                      height: 15px;
                    }
                  }
                  .uploadImg{
                    display: flex;
                    display: -webkit-flex;
                    align-items: center;
                    // width: 85px;
                    >p{
                      color: #333333;
                      font-size: 15px;
                      float: left;
                      line-height: 85px;
                      margin-left: 3%;
                    }
                    >div{
                      display: flex;
                      display: -webkit-flex;
                      color: #C6C6C6;
                      // width: 85px;
                      width: 100%;
                      height: 85px;
                      p{
                        // width: 85px;
                        width: 100%;
                        text-align: center;
                        height: 85px;
                        line-height: 70px;
                        border: 1px solid #C6C6C6;
                        font-size: 50px;
                      }
                      input{
                        width: 100%;
                        height: 100%;
                        opacity: 0;
                        position: absolute;
                        left: 0;
                        top: 0;
                      }
                    }
                  }
                }
                .img::after{
                  content: "";
                  display: block;
                  clear: both;
                }
              }
            }
          }
        }
      }
    }
  }
</style>
