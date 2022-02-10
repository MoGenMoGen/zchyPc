<template>
<!--快速下单-->
<div id="home">
  <div id="addBoxBg" v-show="addShow">
    <div class="addBox">
      <div class="head">
        <p>添加设备</p>
        <img :src="close" @click="addShow=false" class="point">
      </div>
      <div class="addBody">
        <p>
          <input type="text" v-model="searchNm" placeholder="请输入要搜索设备的关键词">
          <button @click="toSeacrh"> <i class="el-icon-search"></i> </button>
        </p>
        <div style="margin-top: 10px;">
          <i class="iconfont" :class="allCheck1?'icon-xuanzhong':'icon-fangkuang'" @click="checkAll1()"></i>
          <p style="flex: 1.8;">产品名称</p>
          <p style="flex: 1.4;">规格型号</p>
          <p style="flex: 1.2;">供应商</p>
        </div>
        <div v-for="(item,index) in goodsList" :key="index">
          <i class="iconfont" :class="item.check?'icon-xuanzhong':'icon-fangkuang'" @click="toCheck1(index)"></i>
          <p style="flex: 1.8;">{{item.goodsNm}}</p>
          <p style="flex: 1.4;">{{item.attrDesc}}</p>
          <p style="flex: 1.2;">{{item.supplier}}</p>
        </div>
        <div class="footPage" style="margin-top: 20px;">
             <el-pagination
               background
               @current-change="handleCurrentChange1"
               :current-page.sync="currentPage1"
               :page-size="pageSize1"
               layout="prev, pager, next, jumper"
               :total="goodsTotal">
             </el-pagination>
        </div>
      </div>
      <button @click="confirmAdd" style="background-color: #2778BE;">确认添加</button>
    </div>
  </div>
  <div class="title">
    <p><span class="lineC"></span>快速下单</p>
    <p @click="back"><i class="el-icon-arrow-left"></i>返回</p>
  </div>
  <div class="quicklyOrder">
    <div class="order-title">
      <i class="iconfont" :class="allCheck?'icon-xuanzhong':'icon-fangkuang'" @click="checkAll()"></i>
      <p style="flex: 1;">序号</p>
      <p style="flex: 1.8;padding-left: 10px;">产品名称</p>
      <p style="flex: 1.4;padding-left: 10px;">规格型号</p>
      <p style="flex: 1.2;padding-left: 10px;">供应商</p>
      <p style="flex: 1.2;">单价</p><p style="flex: 1.3;">数量</p><p style="flex: 1.3;">单位</p>
      <p style="flex: 1.3;">交货时间</p><p style="flex: 1.4;">合计</p>
    </div>
    <div class="order-cont" v-for="(item,index) in orderList" :key="index">
      <i class="iconfont" :class="item.check?'icon-xuanzhong':'icon-fangkuang'" @click="toCheck(index)"></i>
      <p style="flex: 1;padding-left: 10px;">{{index+1}}</p>
      <p class="goodSelect">{{item.goodsNm}}</p>
      <p class="skuSelect">{{item.skuNm}}</p>
      <p style="flex: 1.2;">{{item.supplier}}</p>
      <p v-if="item.origPrice!=price" style="flex: 1.2;">￥{{item.origPrice?fmoney(item.origPrice):0.00}}</p>
      <p v-else style="flex: 1.2;">价格面议</p>
      <p class="numP">
        <el-input-number v-model="item.num" @change="handleChange(index)" style="flex: 1.3;" :min="0.01" :max="item.stock" :precision="2" :step="1"></el-input-number>
        <span>库存 ：{{item.stock}}</span>
      </p>
      <p style="flex: 1.3;">{{item.unitNm}}</p>
      <p style="flex: 1.2;">{{item.leadTime}}个工作日</p>
      <p style="flex: 1.4;" v-if="item.origPrice!=price">￥{{item.origPrice?fmoney(item.origPrice*item.num):0.00}}</p>
      <p style="flex: 1.4;" v-else>价格面议</p>
    </div>
  </div>
  <div class="btnBox">
    <button v-show="orderList.length>0" class="delBtn" @click="toDel">删除</button>
    <button v-show="orderList.length>0" style="background-color: #E4393C;" @click="toSub">立即下单</button>
    <button v-show="orderList.length>0" style="background-color: #2778BE;" @click="addCart">加入购物车</button>
    <button style="background-color: #FB8A13;" @click="addShow=true">添加</button>
  </div>
  <!-- <div class="footPage">
       <el-pagination
         background
         @current-change="handleCurrentChange"
         :current-page.sync="currentPage"
         :page-size="pageSize"
         layout="prev, pager, next, jumper"
         :total="total">
       </el-pagination>
  </div> -->

</div>
</template>

<script>
  import close from "@/assets/img/close.png"
  import {mapState} from "vuex";

    export default {
      layout:'person',
      name: "quicklyOrder",
      components: { },
      data(){
        return{
          close,
          addShow:false,//新增弹窗显示
          allCheck:false,//全选
          orderList:[],
          orderCheckList:[],//勾选上的订单列表
          // currentPage:1,//订单列表的页码
          // pageSize:15,//订单列表的单页数量
          // total:0,//订单列表的总数

          searchNm:'',
          allCheck1:false,
          addSkuList:[],//准备新增的数据列表
          goodsList:[],//商品列表
          currentPage1:1, //商品列表的页码
          pageSize1:20,//商品列表的单页数量
          goodsTotal:0,//商品列表的总数

        }
      },
      computed:{
        ...mapState([
          'bWidth',
          'width',
          'currentRole',
          'price'
        ])
      },
      watch:{

      },

      mounted() {
        console.log('快速下单')
        this.getList()
        this.getGoodsList()
      },
      methods:{
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
        //确认新增设备
        confirmAdd(){
          console.log(this.addSkuList)
          this.addSkuList.forEach(item=>{
            let param={
              goodsId: item.goodsId,
              skuId: item.id,
              supplierId: item.supplierId,
              num: 1
            }
            this.api.addQuickOrder(param)
          })
          this.$message({
            message: '新增成功',
            type: 'success',
            duration:'1500',
            customClass:'mzindex'
          });
          this.addSkuList=[]
          this.addShow=false
          setTimeout(()=>{
            this.searchNm=""
            this.currentPage1=1
            this.goodsList=[]
            this.getGoodsList()
            // this.currentPage=1
            this.orderList=[]
            this.getList()
          },1000)
        },

        //获取设备列表
        getGoodsList(){
          this.allCheck1=false
          let qry = this.query.new()
          if(this.searchNm){
            this.query.toW(qry,'goodsNm',this.searchNm,'LK')
          }
          this.query.toP(qry,this.currentPage1,this.pageSize1)
          this.api.getQuickGoodsSku(this.query.toEncode(qry)).then(res=>{
            if(res){
              this.goodsTotal=res.page.total
              let flag=0
              res.data.list.forEach(item=>{
                item.check=false
                this.addSkuList.forEach(i=>{
                  if(i.id==item.id){
                    item.check=true
                    flag++
                  }
                })
                this.goodsList.push(item)
              })
              if(flag==res.data.list.length && res.data.list.length>0){
                this.allCheck1=true
              }
              console.log(this.goodsList)
            }
          })
        },

        //全选
        checkAll1(){
          this.allCheck1=!this.allCheck1
          if(!this.allCheck1){
            this.goodsList.forEach((item,index)=>{
                item.check=false
                this.$set(this.goodsList,index,this.goodsList[index])
                for(var i = 0 ;i<this.addSkuList.length;i++){
                  if(this.addSkuList[i].id==item.id){
                    this.addSkuList.splice(i,1);
                    i= i-1;
                  }
                }
            })
          }else{
            this.goodsList.forEach((item,index)=>{
              item.check=true
              this.$set(this.goodsList,index,this.goodsList[index])
              let flag=0
              this.addSkuList.forEach(i=>{
                if(i.id==item.id){
                  flag++
                }
              })
              if(flag==0){
                this.addSkuList.push(item)
              }
            })
          }
          console.log(this.addSkuList)
        },

        //选择
        toCheck1(index){
          this.goodsList[index].check=!this.goodsList[index].check
          if(this.goodsList[index].check){//被勾选
            let flag=0
            this.goodsList.forEach(item=>{
              if(!item.check){
                flag++
              }
            })
            if(flag==0){
              this.allCheck1=true
            }
            this.addSkuList.push(this.goodsList[index])
          }else{
            this.allCheck1=false
            console.log(this.goodsList[index])
            let checkIndex = this.addSkuList.findIndex(item=> item.id==this.goodsList[index].id)
            this.addSkuList.splice(checkIndex,1)
          }
          console.log(this.addSkuList)
          this.$set(this.goodsList,index,this.goodsList[index])
        },

        async getList(){
          this.allCheck=false

          this.orderList=await this.api.getQuickOrder()
          // let qry = this.query.new()
          // this.query.toP(qry,this.currentPage,this.pageSize)

          // let data=await this.api.quickOrderPage(this.query.toEncode(qry)) //订单列表
          // this.total=data.page.total
          // let flag=0
          // data.data.list.forEach((item,index)=>{
            // item.check=false
            // this.orderCheckList.forEach(i=>{
              // if(i.id==item.id){
                // item.check=true
                // flag++
              // }
            // })
            // this.orderList.push(item)
          // })
          // if(flag==data.data.list.length && data.data.list.length>0){
          //   this.allCheck=true
          // }
          console.log(this.orderList)
        },

        //全选
        checkAll(){
          this.allCheck=!this.allCheck
          if(!this.allCheck){
            this.orderList.forEach((item,index)=>{
                item.check=false
                this.$set(this.orderList,index,this.orderList[index])
                // let checkIndex = this.orderCheckList.findIndex(i=> i?i.skuId==item.skuId:false)
                for(var i = 0 ;i<this.orderCheckList.length;i++){
                  if(this.orderCheckList[i].skuId==item.skuId){
                    this.orderCheckList.splice(i,1);
                    i= i-1;
                  }
                }
                // delete this.orderCheckList[checkIndex]
            })
          }else{
            this.orderList.forEach((item,index)=>{
              item.check=true
              this.$set(this.orderList,index,this.orderList[index])
              let flag=0
              this.orderCheckList.forEach(i=>{
                if(i.id==item.id){
                  flag++
                }
              })
              if(flag==0){
                this.orderCheckList.push(item)
              }
            })
          }
          console.log(this.orderCheckList)
        },

        //选择
        toCheck(index){
          this.orderList[index].check=!this.orderList[index].check
          if(this.orderList[index].check){//被勾选
            let flag=0
            this.orderList.forEach(item=>{
              if(!item.check){
                flag++
              }
            })
            if(flag==0){
              this.allCheck=true
            }
            this.orderCheckList.push(this.orderList[index])
          }else{
            this.allCheck=false
            console.log(this.orderList[index])
            let checkIndex = this.orderCheckList.findIndex(item=> item?item.skuId==this.orderList[index].skuId:false)
            this.orderCheckList.splice(checkIndex,1)
            // delete this.orderCheckList[checkIndex]
          }
          console.log(this.orderCheckList)
          this.$set(this.orderList,index,this.orderList[index])
        },

        //删除
        toDel(){
          let idList=[]
          let ids=""
          this.orderCheckList.forEach(item=>{
              idList.push(item.id)
          })
          ids=idList.join(',')
          console.log(this.orderCheckList)
          console.log(ids)
          this.api.delQuickOrder(ids).then(res=>{
            this.$message({
              message: '删除成功',
              type: 'success',
              duration:'1500',
              customClass:'mzindex'
            });
            setTimeout(()=>{
              this.orderList=[]
              this.orderCheckList=[]
              // this.currentPage=1
              this.getList()
            },1000)
          })
        },

        //立即下单
        toSub(){
          let proArr = []
          this.orderCheckList.forEach(item=>{
              // item.skuList.forEach(i=>{
              //   if(i.id==item.skuId){
              //     imgUrl=i.imgUrl
              //   }
              // })
              console.log(item)
              proArr.push({
                nm:item.shopNm,
                supplierId:item.supplierId,
                ship: item.ship,
                skuId: item.skuId,
                goodsId: item.goodsId,
                quickId:item.id,
                items: [{
                  img: item.imgUrl,
                  goodsNm: item.goodsNm,
                  attrDesc: item.skuNm,
                  origPrice: item.origPrice,
                  qty: item.num,
                  leadTime:item.leadTime,
                  skuUnitNm: item.unitNm
                }]
              })
          })
          console.log(proArr)
          this.until.seSave('cartList', JSON.stringify(proArr))
          this.until.href('/order/payment?payType=quickly')
        },

        //加入购物车
        addCart(){
          let flag=0
          this.orderCheckList.forEach(item=>{
            // if(item.check){
              flag++
              let param = {
                goodsId: item.goodsId,
                goodsSkuId: item.skuId,
                qty: item.num
              }
              console.log(param)
              this.api.addCart(param)
            // }
          })
          if(flag>0){
            this.$message({
              message: '加入购物车成功!',
              type: 'success',
              duration:'1500',
              customClass:'mzindex'
            });
            setTimeout(()=>{
              this.orderList=[]
              this.orderCheckList=[]
              // this.currentPage=1
              this.getList()
            },1000)
          }
        },
        //搜索
        toSeacrh(){
          this.currentPage1=1
          this.goodsList=[]
          this.getGoodsList()
        },
        //分页跳转
        // handleCurrentChange(val){
        //   this.currentPage=val
        //   this.orderList=[]
        //   this.getList()
        // },
        //分页跳转
        handleCurrentChange1(val){
          this.currentPage1=val
          this.goodsList=[]
          this.getGoodsList()
        },
        //数量改变
        handleChange(index){
          let param={
            id:this.orderList[index].id,
            goodsId: this.orderList[index].goodsId,
            skuId: this.orderList[index].skuId,
            supplierId: this.orderList[index].supplierId,
            num: this.orderList[index].num
          }
          console.log(param)
          this.api.updQuickOrder(param)
        },

        back(){
          this.$router.go(-1)
        },
      },
    }
</script>

<style lang="less">

</style>
<style lang="less" scoped>
  @import url("../../assets/css/init.less");
  #home{
    #addBoxBg{
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
      .addBox{
        width: 650px;
        height: 80%;
        background-color: #FFFFFF;
        display: flex;
        flex-direction: column;
        border-radius:8px;
        .head{
          display: flex;
          justify-content: space-between;
          align-items: center;
          height: 50px;
          width: 100%;
          padding: 0 30px;
          border-bottom: 1px solid rgba(0,0,0,0.3);
          >p{
            margin: 0 auto;
            font-size: 24px;
            padding-left: 16px;
            // width: 37.4%;
          }
          >img{
            width:16px;
            height:16px;
            cursor: pointer;
          }
        }
        .addBody{
          flex: 1;
          display: flex;
          flex-direction: column;
          padding: 20px 30px;
          overflow: auto;
          >p{
            display: flex;
            align-items: center;
            padding: 0 30px 10px 20px;
            >input{
              flex: 1;
              padding-left: 15px;
              border: 1px solid #dedede;
              height: 100%;
            }
            >button{
              background: @themeColor;
              padding: 9px 0;
              width: 50px;
              height: 100%;
              border-radius: 0 5px 5px 0;
              border: none;
              color: #ffffff;
              cursor: pointer;
            }
          }
          >div{
            display: flex;
            align-items: center;
            padding: 10px 20px;
            >i{
              margin-right: 15px;
            }
            .icon-xuanzhong{
              color: #2778BE;
            }
          }
        }
        >button{
          display: block;
          margin: 20px auto;
          padding: 9px 0;
          width: 124px;
          border-radius: 5px;
          border: none;
          color: #ffffff;
          cursor: pointer;

        }
      }
    }
    .title{
      height: 59px;
      border-bottom: 1px solid #F3F3F3;
      width: 100%;
      display: inline-flex;
      justify-content: space-between;
      line-height: 59px;
      p:nth-child(1){
        color: #333333;
        font-size: 20px;
        display: flex;
        align-items: center;
        margin-left: 30px;
        .lineC{
          display: block;
          height: 18px;
          width: 3px;
          background-color: #2778BE;
          margin-right: 8px;
        }
      }
      p:nth-child(2) {
        color: #666666;
        font-size: 14px;
        margin-right: 24px;
        cursor: pointer;
      }
    }
    .quicklyOrder{
      margin-top: 20px;
      overflow: auto;
      .order-title{background-color: #F7F7F7;}
      .order-cont{border-bottom: 1px solid #F3F3F3;}
      .order-title,.order-cont{
        display: flex;
        align-items: center;
        padding: 22px 0;
        width: 130%;
        >i{
          color: #C8C8C8;
          font-size: 20px;
          margin: 0 20px;
          cursor: pointer;
        }
        .icon-xuanzhong{
          color: #2778BE;
        }
        >p{
          flex: 1;
        }
        >p:nth-child(n+5){
          text-align: center;
        }
        .goodSelect{
          flex:1.8;
          padding-right: 20px;
          box-sizing: border-box;
        }
        .skuSelect{
          flex:1.4;
          padding-right: 20px;
          box-sizing: border-box;
        }
        .numP{
          display: flex;
          flex-direction: column;
          position: relative;
          >span{
            font-size: 12px;
            position: absolute;
            bottom: -20px;
            left: 50%;
            transform: translateX(-50%);
            white-space: nowrap;
          }
        }
      }
    }
    .btnBox{
      margin: 30px 30px 30px 0;
      // margin-top: 30px;
      // margin-bottom: 30px;
      // padding-right: 30px;
      display: flex;
      justify-content: flex-end;
      >button{
        margin-right: 20px;
        padding: 9px 0;
        width: 124px;
        border-radius: 5px;
        border: none;
        color: #ffffff;
        cursor: pointer;
      }
      .delBtn{
        border: 1px solid #CECECE;
        background-color: #ffffff;
        color: #999999;
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
  }

</style>
