<template>
<!--  我的预约-->
    <div class="main">
      <div style="padding:0 30px">
        <tobbar :title="title" :show-all="false"></tobbar>

      </div>
      <p style="border-bottom: 1px solid rgba(0,0,0,0.1);"></p>

      <div class="line4">
        <div class="item" v-for="item in list" :key="item.id" @click="getDetail(item.id)">
          <img :src="rightArrow">
          <div>
            <p>船舶户籍:{{item.registry}}</p>
            <p>船舶类型:{{item.typesNm}}</p>
            <p>船舶航区:{{item.sailingAreaNm}}</p>
            <p>船舶材质:{{item.hullMaterialNm}}</p>
          </div>
          <div>
            提交时间:{{item.crtTm}}
          </div>
        </div>

      </div>
      <div class="line5">
        <el-pagination
          @current-change="handleCurrentChange"
          :current-page="pageNo"
          :page-sizes="[10, 20, 30, 40]"
          :page-size="pageSize"
          prev-text="上一页"
          next-text="下一页"
          layout="total, sizes, prev, pager, next, jumper"
          :total="total">
        </el-pagination>
      </div>


      <div v-if="dialogVisible" class="pop">
        <div class="pop-cont" @click.stop>
          <div class="head">
            <img :src="topIc" class="titleImg">
            <img :src="close" class="closeImg" @click="dialogVisible=false">
          </div>
         <div class="pop-bodyer">
           <div class="pop-line0"></div>
           <div class="pop-line1">
             <img :src="leftIc">
             <p>船舶基本参数</p>
             <img :src="rightIc">
           </div>
           <div class="pop-line2" >
             <ul>
               <li>船舶户籍:{{info.registry}}</li>
               <li></li>
             </ul>
             <ul>
               <li>船舶类型:{{info.typesNm}}</li>
               <li>船舶型宽:{{info.width}}m</li>
             </ul>
             <ul>
               <li>船舶航区:{{info.sailingAreaNm}}</li>
               <li>船舶型深:{{info.deep}}m</li>
             </ul>
             <ul>
               <li>船舶材质:{{info.hullMaterialNm}}</li>
               <li>船舶总吨位:{{info.tonnage}}吨</li>
             </ul>
             <ul>
               <li>船舶总长:{{info.totalLen}}</li>
               <li>船舶航速:{{info.speed}}km/h</li>
             </ul>
             <ul>
               <li>公约船长:{{info.pactLen}}</li>
               <li>船舶定员:{{info.complement}}p</li>
             </ul>
           </div>

           <div class="pop-line1">
             <img :src="leftIc">
             <p>船舶设备参数</p>
             <img :src="rightIc">
           </div>
           <div class="pop-line2">
             <ul>
               <li>主机品牌:{{info.hostBrand}}</li>
               <li>主机功率:{{info.hostPower}}kw</li>
             </ul>
             <ul>
               <li>齿轮箱品牌:{{info.gearBrand}}</li>
               <li>齿轮箱型号:{{info.gearModel}}</li>
             </ul>
             <ul>
               <li>发电机品牌:{{info.electricGeneratorBrand}}</li>
               <li>发电机功率:{{info.electricGeneratorPower}}kw</li>
             </ul>

           </div>
           <!-- <div class="pop-line3">
             <p>船舶预算报价：<span>￥{{info.ddd}}</span></p>
           </div> -->

         </div>

        </div>
      </div>

    </div>
</template>

<script>
    import tobbar from "../../components/person/tobbar";
    import searchIc from '../../assets/img/personal/search.png'
    import deleteIc from '../../assets/img/personal/delete.png'
    import successIc from '../../assets/img/success.png'
    import rightArrow from '../../assets/img/personal/rightArrow.png'
    import topIc from '../../assets/img/personal/预约详情.png'
    import leftIc from '../../assets/img/personal/left.png'
    import rightIc from '../../assets/img/personal/right.png'
    import close from "@/assets/img/close.png"
    import StoreProduct from "../Trade/storeProduct";
    export default {
        name: "allShip",
        components:{
            StoreProduct,
            tobbar,

        },
        methods:{

            handleSizeChange(val) {
                console.log(`每页 ${val} 条`);
            },
            handleCurrentChange(val) {
                console.log(`当前页: ${val}`);
            },
            async getList(){
                let qry=this.query.new();
                this.query.toP(qry,this.pageNo,this.pageSize)
                let obj=  await this.api.subscribeList(this.query.toEncode(qry))
                this.list=obj.data.list
                this.total=obj.page.total
            },
            async getDetail(id){
              this.info= await this.api.subscribeDetail(id)
              console.log(this.info)
              this.dialogVisible=true
            },

        },
        mounted(){
          this.getList()
        },
        data(){
            return{
                close,
                searchIc,
                deleteIc,
                successIc,
                rightArrow,
                topIc,
                leftIc,
                rightIc,
                dialogVisible:false,
                title:'我的预约',
                pageNo:1,
                pageSize:10,
                total:0,//
                list:[],
                subscribeDetail:{},
            }
        },
        layout:'person',

    }
</script>

<style scoped lang="less">
  .main{
    width: 100%;
    .line2{
      width: 100%;
      display: flex;
      align-items: center;
      height: 78px;
      padding-left: 29px;
      padding-right: 16px;
      div{
        flex:1;
        display: flex;
        align-items: center;
      }
      .slt{
        margin-right: 30px;
        p{
          margin-right: 10px;
        }
      }
      .search{
        display: flex;
        align-items: center;
        p:nth-of-type(1){
          border-top: 1px solid #DFDFDF;
          border-bottom: 1px solid #DFDFDF;
          border-left: 1px solid #DFDFDF;
          border-top-left-radius: 5px;
          border-bottom-left-radius: 5px;
          height: 38px;
          padding: 0 12px;
          align-items: center;
          display: flex;
          input{
            border: none;
            outline: none;
            color: #666666;
          }
          ::-webkit-input-placeholder { /* Chrome/Opera/Safari */
            color: #999999;
          }
          ::-moz-placeholder { /* Firefox 19+ */
            color: #999999;
          }
          :-ms-input-placeholder { /* IE 10+ */
            color: #999999;
          }
          :-moz-placeholder { /* Firefox 18- */
            color: #999999;
          }
        }
        p:nth-of-type(2){
          background:#2778BE;
          height: 38px;
          display: flex;
          align-items: center;
          justify-content: center;
          width: 55px;
          border-top-right-radius: 5px;
          border-bottom-right-radius: 5px;
          &:hover{
            cursor: pointer;
          }
          img{
            width: 18px;
          }
        }

      }
    }
    .line3{
      display: flex;
      align-items: center;
      height: 60px;
      padding: 0 31px;
      background:#F7F7F7;
      p:nth-of-type(1){
        flex:1;
      }
      p:nth-of-type(2){
        flex:1.5;
      }
      p:nth-of-type(3){
        flex:1;
      }
      p:nth-of-type(4){
        flex:1;
      }
      p:nth-of-type(5){
        flex:1;
      }
      p:nth-of-type(6){
        flex:1;
        text-align: center;
      }

    }
    .line4{

      .item{

        height: 106px;
        border-bottom: 1px solid #DFDFDF;
        padding: 0 31px;
        color: #333333;
        font-size: 15px;
        position: relative;
        &:hover{
          cursor: pointer;
          background: #F6F6F6;
          div:nth-of-type(1){
            color: #2778BE;
          }
        }

        img{
          position: absolute;
          width: 10px;
          right: 31px;
          top:50%;
          transform: translateY(-50%);
        }
        div:nth-of-type(1){
          padding-top: 21px;
          width: 70%;
          display: flex;
          font-size:15px;
          font-family:Microsoft YaHei;
          font-weight:400;
          color:rgba(102,102,102,1);
          line-height:24px;
          p:nth-of-type(1){
            flex:1;
          }
          p:nth-of-type(2){
            flex:1;
          }
          p:nth-of-type(3){
            flex:1;
          }
          p:nth-of-type(4){
            flex:1;
          }

        }
        div:nth-of-type(2){
          padding-top: 10px;
          font-size: 14px;
          font-family:Microsoft YaHei;
          font-weight:400;
          color:rgba(153,153,153,1);
          line-height:24px;
        }

      }


    }
    .line5{
      padding: 30px 40px;
      width: 100%;
      display: flex;
      justify-content: flex-end;
    }
  }


  .pop{
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: rgba(0, 0, 0, 0.5);
    z-index: 100;
    display: flex;
    justify-content: center;
    align-items: center;
    .pop-cont{
      width: 790px;
      height: 80%;
      background-color: #FFFFFF;
      display: flex;
      flex-direction: column;
      border-radius: 8px;
      .head{
        display: flex;
        justify-content: space-between;
        align-items: center;
        height: 59px;
        width: 100%;
        padding: 0 24px 0 30px;
        .titleImg{
          margin: -12px auto 0;
          width: 37.4%;
        }
        .closeImg{
          width: 20px;
          height: 20px;
          cursor: pointer;
        }
      }
      .pop-bodyer{
        flex: 1;
        overflow-y: scroll;
      }
      .pop-line0{
        display: flex;
        justify-content: space-between;
        align-items: center;
        height: 59px;
        width: 100%;
        padding: 0 24px 0 30px;
      }
      .pop-line1{
        margin-bottom: 15*100/1080vh;
        width: 100%;
        display: flex;
        justify-content: center;
        align-items: center;
        img{
          width: 117px;
          height: 16*100/1080vh;
        }
        p{
          color: #333333;
          font-size: 16px;
          margin: 0 29px;
          font-weight:400;
        }

      }
      .pop-line2{
        padding:0 41px;
        margin-bottom: 20*100/1080vh;
        ul{
          width: 100%;
          display: flex;
          li{
            padding: 18*100/1080vh 0;
            flex: 1;
            font-size:15px;
            font-family:Microsoft YaHei;
            font-weight:400;
            color:rgba(102,102,102,1);
            &:first-child{
              flex: 2;
            }
          }

        }
      }
      .pop-line3{
        width:729px;
        height:88*100/1080vh;
        background:rgba(243,243,243,1);
        margin: 0 auto 40px;
        display: flex;
        align-items: center;
        justify-content: center;
        font-size:14px;
        font-family:Microsoft YaHei;
        font-weight:400;
        p{
          display: flex;
          align-items: center;
          justify-content: center;
          span{
            font-size:24px;
            font-family:Microsoft YaHei;
            font-weight:400;
            color:rgba(230,67,71,1);
          }
        }
      }

    }
  }

</style>
