<template>
<!--往来款项(供应商)-->
<div >
  <div class="head">
    <p><span class="lineC"></span>往来款项</p>
    <span @click="back" class="point">< 返回</span>
  </div>
  <div class="mainOrder">
     <div class="nav">
       <p v-for="item in options" @click="choose(item)" :class="{clickP:statusCd==item.value}" class="point">
         {{item.label}}
       </p>
     </div>
     <div class="tableOrder" v-if="statusCd==0">
       <div class="orderTitle">
         <!-- <p style="width: 14px;flex: none;margin-left: 50px;" @click="checkAll">
           <img v-if="checkAllShow" :src="check" alt="">
           <img v-else :src="unCheck" alt=""></p>
         <p style="flex: 0.5;">全选</p> -->
         <p style="margin-left: 45px;">订单详情</p><p>规格型号</p>
         <p>单价</p><p>数量</p><p>实付款</p>
       </div>
       <div class="orderBox" v-for="(item,index) in tableData"  :key="index">
         <div class="boxHead">
           <p style="width: 14px;margin-left: -12px;margin-right: 24px;display: inline-block;">
          </p>
             <span>{{item.crtTm}}</span><span>订单号：{{tabId==1 ? item.orderCd : item.cd}}</span>
         </div>
         <div class="boxBodyer">
           <div class="infoBox" v-if="tabId==1">
             <div v-for="(v,k) in item.itms" :key="k">

               <img v-if="v.check" :src="check" alt="" @click="checkT(v,index,k,1)" style="width: 20px;height: 20px;margin-right: 10px;">
               <img v-else :src="unCheck" alt="" @click="checkT(v,index,k,0)" style="width: 20px;height: 20px;margin-right: 10px;">
                <p v-show="v.goodsImgUrl"><img :src="v.goodsImgUrl"></p>
                <p class="nmBox" :style="v.goodsImgUrl?'flex: 3;':'flex: 4;'">
                  <span style="color: #333333;">{{v.goodsNm }}</span>
                  <span style="color: #999999;">{{v.goodsSkuAttrNm}}</span>
                </p>
                <p style="color: #E4393C;">￥ {{v.unitPrice}}</p><p>{{v.qty}}</p>
             </div>
           </div>
           <div class="infoBox" v-else>
             <div>
               <p class="nmBox" style="flex: 4;">
                 <span style="color: #333333;">{{item.goodsNms}}</span>
                 <span style="color: #333333;">实收款：￥ {{item.paid}}</span>
               </p>
             </div>
           </div>
           <div style="color: #E4393C;">￥ {{tabId==1 ? item.orderPrice : item.amt}}</div>
         </div>
       </div>
     </div>
     <div v-if="statusCd==1" class="statements">
         <el-table
          :data="stateList"
          :cell-style='cellStyle'
          style="width: 100%;"
          :header-cell-style="{'background-color':'#f7f7f7','color':'#666666'}"
          >
            <el-table-column
              prop="crtTm"
              align="center"
              width="200"
              label="生成时间">
            </el-table-column>
            <el-table-column
              prop="total"
              width="200"
              label="订单总额">
            </el-table-column>
            <el-table-column
              width="200"
              prop="payment"
              label="已收"
              >
            </el-table-column>
            <el-table-column
              width="200"
              prop="uncollected"
              label="未收">
            </el-table-column>
            <el-table-column
              fixed="right"
              align="center"
              width="200"
              prop="operations"
              label="操作">
              <template slot-scope="scope">
                <el-button type="text" size="small" class="detail" @click='toDetailStat(scope.row)'>查看详情</el-button>
              </template>
            </el-table-column>
         </el-table>
     </div>
  </div>
  <div class="footPage">
       <!-- <page-ination :total="total" :pageSize="pageSize" @changeP='pageChange'></page-ination> -->
       <el-pagination
         background
         @current-change="handleCurrentChange"
         :current-page.sync="currentPage1"
         :page-size="pageSize"
         layout="prev, pager, next, jumper"
         :total="total">
       </el-pagination>
  </div>
  <div v-if="statusCd==0">
    <button @click="createOrder" class="createBtn">生成对账单</button>
  </div>
</div>
</template>

<script>
    import {mapState} from "vuex";
    import pageInation from "@/components/pageInation.vue"
    import close from "@/assets/img/close.png"
    import check from "@/assets/img/order/check.png"
    import unCheck from  "@/assets/img/order/noCheck.png"
    export default {
      layout:'person',
        name: "shipowner",
      components: {
        pageInation
      },
      data(){
          return{
            close,
            check,
            unCheck,
            checkAllShow:false, //全选勾选
            tabId:1,
            statusCd:0,
            currentPage1: 1,      //前往哪页
            pageSize:10,       //每页数量
            total:0,         //总数
            options: [
              {
                value: '0',
                label: '销售出库单',
              }, {
                value: '1',
                label: '对账单',
              }],
            tableData: [],
            stateList:[],
            currentRole:{},
            checkList:[],
          }
      },
      computed:{
        ...mapState([
          'bWidth',
          'width',
        ])
      },
      mounted() {
         this.currentRole=JSON.parse(this.until.seGet('currentRole'))
         this.getInfo()
      },
      watch:{
      },
      methods: {
        //全选
        checkAll(){
          console.log('全选')

          if(this.checkList.length==this.tableData.length){
            this.checkList=[]
            this.tableData.forEach(item=>{
              item.itms.forEach(v=>{
                v.check=false
              })

              // this.checkList.push(item)
            })
            this.checkAllShow=false
          }else{
            this.checkList=[]
            this.tableData.forEach(item=>{
              item.itms.forEach(v=>{
                v.check=true
                this.checkList.push(v)
              })

            })
            this.checkAllShow=true
          }
          console.log(this.checkList)
        },
        //单选
        checkT(item,index,k,num){
          if(num==1){
            this.tableData[index].itms[k].check=false
            this.checkAllShow=false
            let z=''
            this.checkList.forEach((v,k)=>{
              if(v.id==item.id){
                z=k
              }
            })
            this.checkList.splice(z,1)
          }else{
            if(item.account_status==1){
              this.$message({
                message: '当前订单已报价请选择其他订单',
                type: 'warning',
                duration: '1000'
              });
            }else{
              this.tableData[index].itms[k].check=true
              this.checkList.push(item)
              
            }

            // if(this.checkList.length==this.tableData.length){
            //   this.checkAllShow=true
            // }
          }
          this.$set(this.tableData,index,this.tableData[index])
          console.log(this.checkList)
        },
        //生成对账单
        createOrder(){
          let ids=''
          let flag=0
          let flag1=0
          this.checkList.forEach((item,index)=>{
            // item.itms.forEach((v,k)=>{
            //   ids+=v.id
            //   if(index<this.checkList.length-1 || k<this.checkList[index].itms.length-1){
            //     ids+=','
            //   }
            //   if(v.isAccount==0){
            //     flag++
            //   }
            // })
              ids+=item.id
              if(index<this.checkList.length-1){
                ids+=','
              }
              if(item.account_status==1){
                flag1++
              }
              if(item.isAccount==0){
                  flag++
              }
          })
          console.log(ids)
          if(!ids){
            this.$message({
              message: '请选择有商品的数据',
              type: 'warning',
              duration: '1000'
            });
            return
          }
          if(flag!=0){
            this.$message({
              message: '当前所选项有不可报价的的商品,请重新选择',
              type: 'warning',
              duration: '1000'
            });
            return
          }
          // if(flag1!=0){
          //   this.$message({
          //     message: '当前所选项有已报价过的订单请不要再次选择',
          //     type: 'warning',
          //     duration: '1000'
          //   });
          //   return
          // }
            this.api.createOrder(ids).then(res=>{
              if(res.code=='0'){
                this.$message({
                  message: '报价成功',
                  type: 'success',
                  duration: '1000'
                });
                setTimeout(() => {
                  this.checkList=[]
                  this.tableData=[]
                  this.getInfo()
                }, 1000);

              }
            })

        },
        async getInfo(){
          this.tableData = [];
          this.stateList=[];
          let qry
          let data
          if(this.statusCd==0){
             qry= this.query.new()
            this.query.toO(qry,'crtTm','desc')
            this.pageSize=4    //销售出库订单每页4个
            this.query.toP(qry,this.currentPage1,this.pageSize)
            data=await this.api.outgoingList(this.query.toEncode(qry))
          }else if(this.statusCd==1){
            qry = this.query.new()
            this.query.toO(qry,'crtTm','desc')
            this.pageSize=10  //对账单每页10个
            this.query.toP(qry,this.currentPage1,this.pageSize)
            data=await this.api.statList(this.query.toEncode(qry))
            console.log(data)
          }

          this.total=data.page.total
          // this.$set(this)
          if(this.total>0){
            data.data.list.forEach((item,index)=>{
              if(this.statusCd==0){
                let flag=0
                this.checkList.forEach(v=>{
                  if(v.id==item.id){
                    flag++
                    item.check=true
                  }
                })
                if(flag==0){
                  item.check=false
                }
                this.tableData.push(item)
              }else if(this.statusCd==1){
                item.total=item.total? '￥'+item.total:'￥0'
                item.payment=item.payment? '￥'+item.payment:'￥0'
                item.uncollected=item.uncollected? '￥'+item.uncollected:'￥0'
                this.stateList.push(item)
              }
            })
            console.log('销售出库单')
            console.log(this.tableData)
            console.log('对账单')
            console.log(this.stateList)
          }
        },
        // 单元格的 style 的回调方法
        cellStyle({ row, column, rowIndex, columnIndex }) {
          if (columnIndex === 2) {
            return `color:#2777BD;`;
          }else if (columnIndex === 3){
            return `color:#E3393B;`;
          }else{
            return '';
          }
        },
        toDetail(id){
          this.$router.push('../order/orderDetail?id='+id)
        },
        toDetailStat(item){
          console.log(item.id)
          this.$router.push('../order/stateDetail?id='+item.id)
        },
        toPage(url){
          // console.log(url)
          this.$router.push(url)
        },
        handleCurrentChange(val){
          this.currentPage1=val
          this.tableData=[]
          this.stateList=[]
          this.getInfo()
        },
        choose(item){//选择状态
          this.checkList=[]
          this.currentPage1=1
          this.statusCd=item.value
          this.stateList=[]
          this.tableData=[]
          this.getInfo()
        },
        back(){//返回
			    this.$router.back()
        },
      },
    }
</script>
<style lang="less">
  .status .el-input__inner{
          height: 36px;
          line-height: 36px;
  }
  .statements{
    // .el-table__header-wrapper{
    //   .has-gutter{
    //     th{
    //       color: #666666;
    //       background-color: #F7F7F7;
    //     }
    //   }
    // }
    .detail{
      background:rgba(39,120,190,1);
      color: #ffffff;
      border-radius:3px;
      margin: 0;
      padding: 9px 15px;
    }
  }
  #bg{
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
    .mainDelivery{
      width: 540px;
      height: 50%;
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
          font-size: 18px;
          color: #333333;
          margin: auto;
          padding-left: 36px;
        }
        >img{
          width:20px;
          height:20px;
        }
      }
      .popupDBody{
        >div{
          display: flex;
          align-items: center;
          padding: 0 30px;
          margin-top: 30px;
          height: 52px;
          >span{
            width: 95px;
          }
          >div{
            flex: 1;
            .el-input__inner{
              height: 52px;
              border-radius: 0;
            }
          }
          .popupBtn{
            display: flex;
            justify-content: space-between;
            height: 52px;
            >button{
              height: 100%;
              cursor: pointer;
            }
            .cancel{
              width: 44%;
              background: #ffffff;
              border:1px solid rgba(221, 221, 221, 1);
              color: #666666;
            }
            .confirm{
              width: 50.8%;
              background:rgba(39,120,190,1);
              border: none;
              color: #ffffff;
            }
          }
          >input{
            flex: 1;
            border:1px solid rgba(221, 221, 221, 1);
            height: 52px;
            padding-left: 15px;
          }
        }
      }
    }
  }
</style>
<style lang="less" scoped>
  @import url("../../assets/css/init.less");

  .point{
    cursor:pointer;
  }
  .sail{
    margin: 5px auto;
    display: flex;
    color: #FFFFFF;
    background:#2778BE;
    width: 83px;
    height: 28px;
    align-items: center;
    justify-content: center;
    border-radius: 3px;
    cursor: pointer;
  }
  .sail2{
    margin: 5px auto;
    display: flex;
    background:@borderColor;
    width: 83px;
    height: 28px;
    align-items: center;
    justify-content: center;
    border-radius: 3px;
  }
  .head{
    display: flex;
    justify-content: space-between;
    align-items: center;
    height: 59px;
    width: 100%;
    padding: 0 24px 0 30px;
    border-bottom: 1px solid rgba(0,0,0,0.1);;
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
  .search{
    width: 100%;
    height: 78px;
    padding: 20px 22px;
    display: flex;
    justify-content: space-between;
    align-items: center;
    >div{
      display: flex;
      align-items: center;
      height: 38px;
      box-sizing: content-box;
      >span{
        white-space: nowrap;
        display: inline-block;
        height: 12px;
        font-size: 14px;
        margin-right: 20px;
        font-family: Microsoft YaHei;
        font-weight: 400;
        line-height: 12px;
        color: #333333;
      }
    }
    .orderNo{
      width: 27.1%;
      >input{
        width: 78.9%;
        background: #FFFFFF;
        border:1px solid #DFDFDF;
        border-radius:5px;
        height: 100%;
        font-size: 14px;
        padding-left: 10px;
      }
    }
    .status{
      width: 31.5%;
      .el-select{
        width: 81.9%;
        // height: 100%;
        background:rgba(255,255,255,1);
        border:1px solid rgba(223, 223, 223, 1);
        border-radius:5px;
        height: 100%;

      }
    }
    .shipNm{
      position: relative;
      width: 30.6%;
      background:rgba(255,255,255,1);
      border:1px solid rgba(223, 223, 223, 1);
      border-radius:5px;
      padding-left: 12px;
      >input{
        width: 77.6%;
        height: 100%;
        border: none;
      }
      >span{
        position: absolute;
        display: flex;
        justify-content: center;
        align-items: center;
        margin: 0;
        right: 0;
        width: 18.3%;
        height:100%;
        background:rgba(39,120,190,1);
        border-bottom-right-radius: 5px;
        border-top-right-radius: 5px;
        .icon-ziyuan{
          color: #ffffff;
        }
      }
    }
  }
  .mainOrder{
    width: 100%;
    .tab{
      margin: 0 36px 0 30px;
      border-bottom: 1px solid #2778BE;
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
      margin: 17px 36px 30px 30px;
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
    .tableOrder{
      width: 100%;
      .orderTitle{
        background-color: #F7F7F7;
        font-size: 16px;
        color: #666666;
        padding: 22px 0;
        display: flex;
        justify-content: space-between;
        >p{
          text-align: center;
          flex: 1;
        }
      }
      .orderBox{
        margin: 30px;
        .boxHead{
          padding: 14px 0 14px 30px;
          background:rgba(247,247,247,1);
          font-size:14px;
          color: #666666;
          >span{
            margin-right: 6.4%;
          }
        }
        .boxBodyer{
          display: flex;
          border-left:  1px solid #E7E7E7;
          >div{
            flex: 1;
            border-right: 1px solid #E7E7E7;
            padding-top: 20px;
            text-align: center;
            border-bottom: 1px solid #E7E7E7;
          }
          .infoBox{
            flex: 4;
            display: flex;
            flex-direction: column;
            border-bottom: none;
            padding-top: 0;
            >div{
              display: flex;
              padding: 20px 0 20px 30px;
              height: 100%;
              border-bottom: 1px solid #E7E7E7;
              >p{
                flex: 1;
                margin-right: 10px;
                text-align: center;
              }
              .nmBox{
                flex: 3;
                text-align: left;
                display: flex;
                flex-direction: column;
                justify-content: space-between;
                >span{
                  display: block;
                }
              }
            }
          }
          .btnList{
            display: flex;
            flex-direction: column;
            align-items: center;
            padding-top: 0;
            padding-bottom: 10px;
            >button{
              // width: 60%;
              margin: 0;
              margin-top: 12px;
              padding: 9px 15px;
            }
            .detail{
              background:rgba(39,120,190,1);
              color: #ffffff;
              border-radius:3px;
            }
            .pay{background: #E4393C;border: none;color: #FFF;}
            .cancel,.delete{color: #999;}
            .detail,.logistics{color: #2778BE;}
            .contrat{color: #2778BE;border-color: #2778BE;}
            .confirm{background-color: #2778BE;color: #FFF;}
          }
        }
      }
    }
  }
  .footPage{
    display: flex;
    justify-content: center;
    margin: 44px 0 20px;
    .btn_confirm{
      margin-left: 43px;
    }
  }
  .createBtn{
    width: 300px;
    height: 52px;
    margin: 40px auto 30px;
    display: block;
    background: rgba(39,120,190,1);
    border-radius: 5px;
    border: none;
    color: #ffffff;
    font-size: 18px;
    cursor: pointer;
  }
</style>
