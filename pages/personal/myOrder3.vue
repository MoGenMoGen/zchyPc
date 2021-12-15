<template>
<!--销售订单(供应商)-->
<div >
  <!-- 填写物流信息 -->
  <div v-if="deliveryShow" id="bg">
    <div class="mainDelivery">
      <div class="head">
        <p>立即发货</p>
        <img :src="close" @click="toClose" class="point">
      </div>
      <div class="popupDBody">
        <div>
          <span>物流公司 :</span>
          <el-select v-model="deliveryList.logiEntpId" placeholder="请选择物流公司"  filterable>
            <el-option
              v-for="item in logiList"
              :key="item.id"
              :label="item.nm"
              :value="item.id">
            </el-option>
          </el-select>
        </div>
        <div>
          <span>物流编号 :</span>
          <input type="text" v-model="deliveryList.logiCd" placeholder="请输入物流编号">
        </div>
        <div>
          <span></span>
          <div class="popupBtn">
            <button class="cancel" @click="toClose">取消</button>
            <button class="confirm" @click="confirmDelivery">确认</button>
          </div>
        </div>
      </div>
    </div>

  </div>
  <!-- 物流信息弹窗 -->
  <el-dialog title="物流信息"  :visible.sync="dialogVisible"   width="40%"  >
    <el-timeline >
      <el-timeline-item
        v-for="(item, index) in expressList"
        :key="index"
        :timestamp="item.time">
        {{item.context}}
      </el-timeline-item>
    </el-timeline>
  </el-dialog>

  <div class="head">
    <p><span class="lineC"></span>销售订单</p>
    <span @click="back" class="point">< 返回</span>
  </div>
  <div class="mainOrder">
     <div class="nav">
       <p v-for="item in options" @click="choose(item)" :class="{clickP:statusCd==item.value}" class="point">
         {{item.label}}
       </p>
     </div>
     <div class="tableOrder">
       <div class="orderTitle">
         <p>订单详情</p><p>规格型号</p><p>单价</p><p>数量</p>
         <p>实付款</p><p>交易状态</p><p>交易操作</p>
       </div>
       <div class="orderBox" v-for="item in tableData">
         <div class="boxHead">
           <span>{{item.crtTm}}</span><span>订单号：{{item.orderId}}</span>
         </div>
         <div class="boxBodyer">
           <div class="infoBox">
             <div v-for="v in item.dsPurchaseItms">
                <p v-show="v.goodsImgUrl"><img :src="v.goodsImgUrl"></p>
                <p class="nmBox" :style="v.goodsImgUrl?'flex: 3;':'flex: 4;'">
                  <span style="color: #333333;">{{v.goodsNm }}</span>
                  <span style="color: #999999;">{{v.goodsSkuAttrNm}}</span>
                </p>
                <p style="color: #E4393C;">￥ {{fmoney(v.unitPrice)}}</p><p>{{v.qty}}</p>
             </div>

           </div>
           <div style="color: #E4393C;">￥{{fmoney(item.orderPrice)}}</div>
           <div>{{item.statusNm}}</div>
           <div class="btnList">
             <el-button @click="toDetail(item.id)" type="text" size="small" :style="item.statusCd==3?'color:#666;':''" v-if="item.statusCd!=5" class="detail">查看详情</el-button>
             <el-button @click="toDelivery(item.id,item.orderId)" type="danger" size="small" plain v-if="item.statusCd==2" class="pay">立即发货</el-button>
             <!-- <el-button @click="toPay(item.id)" type="danger" size="small" plain v-if="item.statusCd==1" class="pay">确认收款</el-button> -->
             <!-- <el-button @click="toPay(item.id)" type="danger" size="small" plain v-if="item.statusCd==1" class="pay">去支付</el-button> -->
             <!-- <el-button @click="cancelOrder(item.id)" type="text" size="small" v-if="item.statusCd==1" class="cancel">取消订单</el-button> -->
             <!-- <el-button type="text" size="small" v-if="item.statusCd==4" class="contrat">查看合同</el-button> -->
             <el-button type="text"  size="small" v-if="item.statusCd==3" class="contrat" @click="trace(item)">查看物流</el-button>
             <!-- <el-button type="text" size="small" v-if="item.statusCd==3" class="confirm">确认收货</el-button> -->
             <el-button type="text" size="small" v-if="item.statusCd==4||item.statusCd==5" class="delete" @click="delOrder(item)" >删除订单</el-button>
           </div>
         </div>
       </div>

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
</div>
</template>

<script>
    import {mapState} from "vuex";
    import pageInation from "@/components/pageInation.vue"
    import close from "@/assets/img/close.png"
    export default {
      layout:'person',
        name: "shipowner",
      components: {
        pageInation
      },
      data(){
          return{
            close,
            deliveryShow:false, //立即发货 弹窗显示
            expressList :[],    //物流信息列表
            dialogVisible : false,  //物流信息 弹窗显示
            tabId:1,
            statusCd:0,
            currentPage1: 1,      //前往哪页
            pageSize:4,       //每页数量
            total:0,         //总数
            options: [
              {
                value: '0',
                label: '全部',
              }, {
                value: '1',
                label: '未付款',
              }, {
                value: '2',
                label: '待发货',
              }, {
              value: '3',
              label: '已发货',
              }, {
              value: '4',
              label: '已完成',
            }],
            tableData: [],
            currentRole:{},
            deliveryList:{
              id:'',
              logiCd:'',
              logiEntpId:'',
              orderId:'',
            },
            logiList:{},

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
         this.statusCd = this.until.getQueryString('statusCd')
         this.getlogiList()
         this.getInfo()
      },
      watch:{
      },
      methods: {
        fmoney(s, n) {
            n = n > 0 && n <= 20 ? n : 2;
            s = parseFloat((s + "").replace(/[^\d\.-]/g, "")).toFixed(n) + "";
            var l = s.split(".")[0].split("").reverse(), r = s.split(".")[1];
            var t = "";
            for (let i = 0; i < l.length; i++) {
                t += l[i] + ((i + 1) % 3 == 0 && (i + 1) != l.length ? "," : "");
            }
            return t.split("").reverse().join("") + "." + r;
        },
        async getlogiList(){
          this.logiList=await this.api.logiList()
          console.log('物流公司列表')
          console.log(this.logiList)
        },
        async getInfo(){
          this.tableData = [];
          let qry = this.query.new()
          if(this.statusCd){
            if(this.statusCd=='0'){
            }
            else if(this.statusCd=='1'){
              this.query.toW(qry,'status','10','EQ')
            }
            else if(this.statusCd=='2'){
              this.query.toW(qry,'status','20','EQ')
            }
            else if(this.statusCd=='3'){
              this.query.toW(qry,'status','30,40','BT')
            }
            else if(this.statusCd=='4'){
              this.query.toW(qry,'status','50,90','BT')
            }
          }
          this.query.toO(qry,'crtTm','desc')
          this.query.toP(qry,this.currentPage1,this.pageSize)
          let param={
            orgEnterId:this.currentRole.id,
          }
          let data=await this.api.shopOrder(this.query.toEncode(qry))

          this.total=data.page.total
          // this.$set(this)
          if(this.total>0){
            data.data.list.forEach((item,index)=>{
              if(item.status=='10'){
                item.statusCd=1
                item.statusNm='待支付'
              }else if(item.status=='20'){
                item.statusCd=2
                item.statusNm='待发货'
              }else if(item.status=='30'){
                item.statusCd=3
                item.statusNm='已发货'
              }else if(item.status=='40'){
                item.statusCd=3
                item.statusNm='未签收'
              }else if(item.status=='50'){
                item.statusCd=4
                item.statusNm='自动签收'
              }else if(item.status=='60'){
                item.statusCd=4
                item.statusNm='用户确认收货'
              }else if(item.status=='70'){
                item.statusCd=4
                item.statusNm='自动确认收货'
              }else if(item.status=='80'){
                item.statusCd=4
                item.statusNm='待评价'
              }else if(item.status=='90'){
                item.statusCd=4
                item.statusNm='已完成'
              }
              item.index=index+1

              this.tableData.push(item)
            })
            console.log('销售订单')
            console.log(this.tableData)
          }
          // console.log('订单')
          // console.log(data)
        },
        //去发货
        toDelivery(id,orderId){
          console.log('发货')
          this.deliveryList.id=id
          this.deliveryList.orderId=orderId
          this.deliveryShow=true

        },
        //确认发货
        async confirmDelivery(){
          console.log(this.deliveryList)
          if(!this.deliveryList.logiCd){
            this.$message({
              message: '请输入物流编号',
              type: 'warning',
              duration: '1000'
            });
            return
          }
          if(!this.deliveryList.logiEntpId){
            this.$message({
              message: '请选择物流公司',
              type: 'warning',
              duration: '1000'
            });
            return
          }
          let data=await this.api.shopBasicDetail(this.deliveryList.id)
          console.log('详情信息')
          console.log(data)
          let logiEntpCd=''
          let logiEntpNm=''
          this.logiList.forEach(item=>{
            if(item.id==this.deliveryList.logiEntpId){
              logiEntpCd=item.cd
              logiEntpNm=item.nm
            }
          })
          let param={
              id: this.deliveryList.orderId,
            	logiCd: this.deliveryList.logiCd,
            	logiEntpId: this.deliveryList.logiEntpId,
            	logiEntpCd: logiEntpCd,
            	logiEntpNm: logiEntpNm,

            	receNm: data.receNm,
            	receMob: data.receMob,
            	addrNm: data.addrNm
          }
          console.log(param)

          this.api.shopDelivery(param).then(res=>{

            this.$message({
              message: '成功',
              type: 'success',
              duration:'1000'
            });
            setTimeout(() => {
              this.deliveryShow = false
              this.tableData=[]
              this.deliveryList={
                id:'',
                logiCd:'',
                logiEntpId:'',
                orderId:'',
              }
              this.getInfo()
            }, 1000)
          })
        },
        //查询物流
        trace(item){
          if (item.logiEntpCd != null && item.logiCd != null){
            this.api.trace(item.logiEntpCd,item.logiCd).then((res) => {
              let parse = JSON.parse(res.msg);
              if (parse.status === '200'){
                this.expressList = parse.data;
                this.dialogVisible = true;
              }else {
                this.$message.error("未找到物流信息")
              }
            })
          }else {
            this.$message.error("未找到物流信息")
          }
        },
        //删除订单
         delOrder(item){
          this.$confirm('是否确认删除订单?', '提示', {
            confirmButtonText: '确认',
            cancelButtonText: '取消',
            type: 'warning'
          }).then(() => {
            this.api.delOrder(item.id).then((res) => {
                if (res.code === 0) {
                  this.tableData = []
                  this.getInfo()
                }
              }
            )

          })
        },
        toClose(){
          this.deliveryList={
            id:'',
            logiCd:'',
            logiEntpId:'',
            orderId:'',
          }
          this.deliveryShow=false
        },
        toDetail(id){
          this.$router.push('../order/orderDetail3?form=order3&id='+id)
        },
        toPage(url){
          // console.log(url)
          this.$router.push(url)
        },
        handleCurrentChange(val){
          this.currentPage1=val
          this.tableData=[]
          this.getInfo()
        },
        choose(item){//选择状态
          this.statusCd=item.value
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
</style>
