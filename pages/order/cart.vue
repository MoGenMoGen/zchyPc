<template>
<!--  购物车-->
    <div id="home" :style="{width:bWidth+'px'}"  @click="attrClose">
      <div class="cont" :style="{width:width+'px'}">
        <div class="line1">
          <p>
            <img :src="car">
            <span class="red">购物车</span>
          </p>
          <p @click.stop="toPage('../')">
            < &nbsp;&nbsp;返回首页
          </p>
        </div>
        <div class="line2">
          <p class="line2-1">全部商品(<span>{{cartNum}}</span>)</p>
          <div class="line2-2">
            <p>
              <img :src="checked" class="checked cursor" v-if="allChecked" @click.stop="allChoose">
              <img :src="unchecked" class="checked cursor" v-else @click.stop="allChoose">
              <span >全选</span>
            </p>
            <p>产品信息</p>
            <p>单价</p>
            <p>数量</p>
            <p>小计</p>
            <p>操作</p>
          </div>
          <div class="line2-3">
            <div class="item-shop" v-for="(item,index) in list" :key="item.id" >
              <p class="shop-nm">
                <img :src="checked" class="checked cursor" v-if="item.shopChecked" @click.stop="chooseShop(item)">
                <img :src="unchecked" class="checked cursor" v-else @click.stop="chooseShop(item)">
                <el-link @click="toShop(item.id)" :underline="false">{{item.nm}}</el-link>
              </p>
              <div class="item-good" v-for="j in item.items" :key="j.id" :class="{active:j.attrShow}">


                <div>
                  <div class="d1">
                    <img :src="checked" class="checked cursor" v-if="j.goodChecked" @click.stop="chooseGood(index,j)">
                    <img :src="unchecked" class="checked cursor" v-else @click.stop="chooseGood(index,j)">
                    <img  :src="j.img" class="pre" @click="toGoods(j.goodsId)">
                    <span style="cursor: pointer" @click="toGoods(j.goodsId)"> {{j.goodsNm}}</span>
                  </div>
                   <div class="d2" v-if="j.gift === 1">
                        <div v-for="z in j.giftItems" :key="z.id">
                          <el-link @click="toGoods(z.id)"  style="color: #7F828B" :underline="false">[赠品] {{z.nm}} {{z.attrDesc}} </el-link>
                        </div>
                   </div>

                </div>
                <!--规格-->
                <div>
                  <div class="attr" @click.stop="">
                    {{j.attrDesc}}
                    <img :src="alter" @click.top="attrOpen(index,j)">
                    <div class="attrChoose" v-show="j.attrShow">
                      <img :src="arrowTop"/>
                      <attr-choose @cancel="attrClose" @confirm="attrChange" :info="currentInfo"></attr-choose>
                    </div>
                  </div>
                </div>
<!--                单价-->
                <div v-if="j.origPrice!=price">￥{{j.origPrice}}</div>
                <div v-else>价格面议</div>
<!--                数量-->
                <div>
                  <span @click.stop="toMinus(j)">-</span>
                  <el-input-number class="inputNumber" v-model="j.qty" @change="onchangeCart(j)" :precision="2" :step="1" :controls="false"></el-input-number>
                  <span @click.stop="toPlus(j)">+</span>
                </div>
                <div v-if="j.origPrice!=price">￥{{j.qty *j.origPrice}}</div>
                <div v-else>价格面议</div>
                <div @click.stop="toDel(j)">删除</div>


                </div>
              </div>


          </div>
<!--          结算-->
          <div class="line2-4">
            <p>
              <img :src="checked" class="checked cursor" v-if="allChecked" @click.stop="allChoose">
              <img :src="unchecked" class="checked cursor" v-else @click.stop="allChoose">
              <span>全选</span>
            </p>

            <p @click.stop="toDelMore">删除选中商品</p>
            <p>已选商品 <span>{{totalNum}}</span> 件</p>
            <p>总价：<span>￥{{totalPrice}}</span></p>
            <p @click.stop="settlement">结&nbsp;&nbsp;算</p>
          </div>
        </div>

<!--        标签页-->
        <div class="line3" >
          <div class="box">
            <div class="item" v-for="item in tabList" :key="item.id" @click.stop="toChooseTab(item)" >
              <p :class="{active:item.id==selectedId}" > {{item.nm}}</p>
            </div>
          </div>

        </div>


        <product-list :list="proList"></product-list>
<!--        底部边距-->
        <div style="height: 78px"></div>
      </div>


    </div>
</template>

<script>
    import car from '../../assets/img/car.png'
    import checked from '@/assets/img/order/check.png'
    import unchecked from '@/assets/img/order/noCheck.png'
    import alter from '../../assets/img/alter.png'
    import arrowTop from '../../assets/img/order/arrowTop.png'
    import productList from "@/components/onlineTrading/productList";
    import attrChoose from "@/components/attrChoose";
    import {mapState} from "vuex";
    export default {
        name: "car",
        components: {productList,attrChoose},
        data(){
            return{
                car,
                checked,
                unchecked,
                alter,
                arrowTop,
                list:[],
                chooseNum: 0, //全部商品数量为
                allChecked:false,//选中所有
                tabList:[
                    {
                        id: 1,
                        nm: '猜你喜欢'
                    },
                ],
                selectedId:1,//选中tab
                totalNum:0,
                totalPrice: 0,
              currentInfo:{},//正在进行规格选择的商品信息
              proList:[],
            }
        },
      computed: {
        ...mapState([
          'bWidth',
          'width',
          'loading',
          'cartNum',
          'price'
        ])
      },
      watch:{
        list:{
          handler:function () {
            this.totalNum = 0
            this.totalPrice = 0
            this.list.forEach(item=>{
              item.items.forEach(v=>{
                this.chooseNum = v.qty
                if(v.goodChecked){
                  this.totalNum += v.qty
                  this.totalPrice += v.qty * v.origPrice
                }
              })
            })
          },
          deep:true,
        },
      },
      mounted(){
        this.getList()
      },
      methods:{
        async getList(){
          this.list = await this.api.cartList()
          console.log(this.list)

          let qry = this.query.new()
          this.proList = await this.api.proRank(this.query.toEncode(qry),5)
        },
        toPage(url){
          this.$router.push(url)
        },
        toShop(id){
          window.open('../sinovat/Trade/storeHome?shopId='+id)
        },
        //跳转产品详情
        toGoods(id) {
          let url = '../sinovat/Trade/productDetail?id='+ id + "&type=product"
          window.open(url)
        },
        //关闭弹出的规格选项
        attrClose(){
          this.list.forEach((item,i)=>{
            let ifChange = false
            item.items.forEach(v=>{
              if(v.attrShow){
                ifChange = true
                v.attrShow = false
              }
            })
            if(ifChange){
              this.$set(this.list,i,item)
            }
          })
        },
        //打开规格选项
        attrOpen(index,items){
          if(!items.attrShow){
            this.list.forEach((item,i)=>{
              item.items.forEach(v=>{
                v.attrShow = false
              })
              this.$set(this.list,i,item)
            })
            items.attrShow = true
            this.$set(this.list,index,this.list[index])
            this.currentInfo = items
          }

        },
        //更换规格选项
        attrChange(param){

            this.store.commit('changeLoading',true)
            this.api.cartChangeSku(param).then(()=>{
              this.attrClose()
              this.getList()
            })
        },
        //结算
        settlement(){
           let proArr=[]
            this.list.forEach(item=>{
              let proArr2 = JSON.parse(JSON.stringify(item))  //所有信息保存，除了商品信息
              proArr2.items = []
              item.items.forEach(v=>{
                if(v.goodChecked){
                  proArr2.items.push(v)  //选中的商品信息保存
                }
              })
              if(proArr2.items.length>0){ //如果有选中的商品信息，则这个店的信息保存
                proArr.push(proArr2)
              }
            })
            if(proArr.length==0){
              this.$message({
                message: '您未选择商品，请选择商品结算',
                type: 'warning'
              });
            }else {
             console.log(proArr)
               this.until.seSave('cartList',JSON.stringify(proArr))
               this.$router.push('./payment?payType=cart')
            }
        },

        //删除一项
        toDel(j){
            this.$confirm('确定删除当前商品?', '提示', {
                confirmButtonText: '确定',
                cancelButtonText: '取消',
                type: 'warning'
            }).then(() => {
              this.cartDelet(j.cartId)

            }).catch(() => {

            });

        },
        //删除所选中的
        toDelMore(){
          let idArr=[]
          this.list.forEach(item=>{
            item.items.forEach(v=>{
               if(v.goodChecked){
                 idArr.push(v.cartId)
               }
            })
          })
          this.cartDelet(idArr.join(','))
        },
        cartDelet(id){
          let param = {
              ids:id
          }
          this.store.commit('changeLoading',true)
          this.api.cartDelet(param).then(()=>{
            this.$message({
              type: 'success',
              message: '删除成功!'
            });
            this.getList()
          })
        },
          //购物车减
          toMinus(j){
             if(j.qty>1){
               j.qty--
               let param = {
                 cartId:j.cartId,
                 qty:j.qty
               }
               this.api.cartChangeNum(param)
             }
          },
          //购物车加
          toPlus(j){
              let param = {
                cartId:j.cartId,
                qty:j.qty+1
              }
              this.api.cartChangeNum(param).then(()=>{
                j.qty++
              })
          },

          //购物车输入框修改数量触发
          onchangeCart(j){
            let param = {
              cartId:j.cartId,
              qty:j.qty
            }
            this.api.cartChangeNum(param).then(()=>{

            }).catch(()=>{ //数量修改失败则重新调取购物车接口，使数量显示正确
               this.getList()
            })
          },



          //全选
          allChoose(){
              this.allChecked=!this.allChecked
              if(this.allChecked){
                  this.list.forEach((item,index)=>{
                      item.shopChecked=true
                      item.items.forEach(j=>{
                          j.goodChecked=true
                      })
                     this.$set(this.list,index,item)
                  })
              }else{
                  this.list.forEach((item,index)=>{
                      item.shopChecked=false
                      item.items.forEach(j=>{
                          j.goodChecked=false
                      })
                      this.$set(this.list,index,item)
                  })
              }
          },
          //选中商城
          chooseShop(item){
            item.shopChecked=!item.shopChecked
              this.chsShopAll(item)
          },


          //选中商品
          chooseGood(index,j){
              window.console.log(j)
              j.goodChecked=!j.goodChecked
              this.$set(this.list,index,this.list[index])
              this.isShopAll(this.list[index])

          },
          //选中猜你喜欢标签
          toChooseTab(item){
              if(item.id!=this.selectedId){
                  this.selectedId=item.id
              }
          },


          //判断是否全选
          isCheckAll(){
              window.console.log("调用全选")
              let t=0
              for(let i of this.list){
                  if(i.shopChecked){
                      t++
                  }
              }
              if(t==this.list.length){
                  this.allChecked=true
              }else{
                  this.allChecked=false
              }
          },

          //    判断一个商城到商品是否全部选中

          isShopAll(shop){
              let t=0;//选中数量
            for(let i of shop.items){
                if(i.goodChecked){
                   t++
                }
            }
            if(t==shop.items.length){
                shop.shopChecked=true
            }else{
                shop.shopChecked=false
            }
            this.isCheckAll()

          },
      //    选中商城下所有商品，或者反选
          chsShopAll(shop){
             let index = this.list.findIndex(item=> item==shop)
              if(shop.shopChecked){

                  for(let i of shop.items){
                      i.goodChecked=true
                  }
              }else{
                  for(let i of shop.items){
                      i.goodChecked=false
                  }
              }
              this.$set(this.list,index,shop)
              this.isCheckAll()
          }

      }
    }
</script>
<style lang="less">
  .inputNumber{
    height: 100%;
    input{
      height: 100%;
      border: none;
      background: none;
      padding-left: 5px !important;
      padding-right: 5px !important;
    }
  }
</style>
<style scoped lang="less">
  #home {
    background: #F3F3F3;

    .cont {
      margin: 0 auto;
      .line1{
        display: flex;
        height: 72px;
        align-items: center;
        justify-content: space-between;
        padding-left: 42px;
        padding-right: 23px;
        p{
          display: flex;
          align-items: center;
          color: #2778BE;
          &:last-of-type{
            cursor: pointer;
          }
          img{
            width: 21px;
            margin-right: 10px;
          }
          span{
            font-size: 22px;
          }
        }
      }
      .line2{
        padding: 20px;
        background: white;
        .line2-1{
          color: #2778BE;
          font-size: 16px;
        }
        .line2-2{
          display: flex;
          align-items: center;
          height: 50px;
          background: #F3F3F3;
          margin-top: 19px;
          padding-left: 28px;
          padding-right: 69px;
          p{
            color: #666666;
          }
          /*  多选框*/
          p:nth-of-type(1){
            flex: 1;
            display: flex;
            align-items: center;
          }

          /*产品信息*/
          p:nth-of-type(2){
            flex: 7;
            padding-left: 100px;
            box-sizing: border-box;
          }
          /*单价*/
          p:nth-of-type(3){
            flex: 1;
            text-align: center;
          }
          /*数量*/
          p:nth-of-type(4){
             flex: 1;
             text-align: center;
           }
          /*小计*/
          p:nth-of-type(5){
              flex:1;
              text-align: center;
           }
          /*操作*/
          p:nth-of-type(6){
            flex: 1;
            text-align: right;
          }



        }
        .line2-3{
          .item-shop{
            border-bottom: 1px solid #F0F0F0;
            /*商店名称*/
            .shop-nm{
              padding-left: 28px;
              padding-right: 69px;
              display: flex;
              align-items: center;
              margin-top: 28px;
              margin-bottom: 11px;
            }
            /*产品列表*/
            .item-good{
              padding-left: 27px;/*因为有一个边距*/
              padding-right: 69px;
              padding-top: 25px;
              padding-bottom: 25px;
              display: flex;
              border-top: 1px solid #F0F0F0;
              border-left: 1px solid #F0F0F0;
              border-right: 1px solid #F0F0F0;
              >div:nth-of-type(1){
                .d1{
                  width: 400px;
                  display: flex;
                  flex: 5;
                  color: #333333;
                  /*预览图*/
                  .pre{
                    width: 90px;
                    height: 100%;
                    /*height: 90px;*/
                    margin-right: 10px;
                    cursor: pointer;
                  }
                }
                .d2{
                  margin-left: 30px;
                  margin-top: 20px;
                }


              }

              /*>div:nth-of-type(2){*/
              /*  flex: 2;*/
              /*}*/

              /*规格*/
              >div:nth-of-type(2){
                flex: 4;
                color: #999999;
                .attr{
                  width: 80%;
                  padding: 10px 20px;
                  height: 100%;
                  display: block;
                  position: relative;
                  /*修改规格图标*/

                  >img{
                    position: absolute;
                    width: 22px;
                    height: 22px;
                    right: -1px;
                    top:-1px;
                    display: none;

                    cursor: pointer;
                  }
                  .attrChoose{
                    z-index: 99;
                    width: 489px;
                    position: absolute;
                    top:78px;
                    left: -1px;
                    background: #ffffff;
                    border:1px solid rgba(204, 204, 204, 1);
                    box-shadow:0px 2px 2px 0px rgba(13,4,8,0.14);
                    >img{
                      position: absolute;
                      top:-9px;
                      left:30px;
                    }
                  }
                }
                /*display: flex;*/
                /*justify-content: center;*/


              }
              /*单价*/
              >div:nth-of-type(3){
                color: #666666;
                flex: 1;
                text-align: center;
              }
              /*数量*/
              >div:nth-of-type(4){
                color: #666666;
                flex: 1;
                display: flex;
                span{
                  width: 20px;
                  height: 20px;
                  border: 1px solid #CCCCCC;
                  background: #EDEDED;
                  display: flex;
                  align-items: center;
                  justify-content: center;
                  &:hover{
                    cursor: pointer;
                  }
                }
                /deep/.el-input-number{
                  width: 60px;
                  height: 20px;
                  text-align: center;
                  outline: none;
                  border:none;
                  border-top: 1px solid #CCCCCC;
                  border-bottom: 1px solid #CCCCCC;
                  line-height: inherit;
                  /*去掉上下箭头*/
                  &::-webkit-outer-spin-button,
                  &::-webkit-inner-spin-button { -webkit-appearance: none; }
                  &[type="number"]{ -moz-appearance: textfield; }
                  /deep/.el-input{
                    height: 100%;
                    /deep/.el-input__inner{
                      height: 100%;
                      padding: 0 5px;
                      border: none;
                    }
                  }
                }
                text-align: center;
              }
              /*小计*/
              >div:nth-of-type(5){
                color: #666666;
                flex: 1;
                text-align: center;
                font-weight: bold;
              }
              /*删除*/
              >div:nth-of-type(6){
                color: #999;
                flex: 1;
                text-align: right;
                &:hover{
                  cursor: pointer;
                }
              }


              &:hover{
                background: #F3F7FF;
                /*规格hover*/
                >div:nth-of-type(2){
                  .attr{
                    border: 1px solid #E4393C ;
                    background: white;
                    >img{
                      display: block;
                    }
                  }
                }
                >div:last-of-type{
                  color: #E4393C;
                }
              }
            }
            .active{
              background: #F3F7FF;
              /*规格hover*/
              >div:nth-of-type(2){
                .attr{
                  border: 1px solid #E4393C ;
                  background: white;
                  >img{
                    display: block;
                  }
                }
              }
              >div:last-of-type{
                color: #E4393C;
              }
            }
          }


        }
        /*结算*/
        .line2-4{
          margin-top: 20px;
          display: flex;
          padding-left: 28px;
          justify-content: space-between;
          align-items: center;
          height: 50px;
          border: 1px solid #F0F0F0;

          /*全选*/
          p:nth-of-type(1){
            flex: 1;
            display: flex;
            align-items: center;

          }
          /*删除*/
          p:nth-of-type(2){
            flex: 1;
            text-align: left;
            &:hover{
              cursor: pointer;
            }
          }
          /*选中数量*/
          p:nth-of-type(3){
            flex: 4;
            text-align: center;
            color: #303030;
            span{
              color: #E64347;

            }
          }
          /*总价*/
          p:nth-of-type(4){
            flex: 2;
            text-align: center;
            color: #303030;
            span{
              color: #E64347;
              font-size: 19px;
            }
          }
          /*结算按钮*/
          p:nth-of-type(5){
            flex-shrink: 0;
            text-align: center;
            width: 201px;
            height: 100%;
            color: white;
            background: #E64347;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 20px;
            &:hover{
              cursor: pointer;
            }
          }



        }

      }
      .line3{
        margin-top: 16px;margin-bottom: 16px;margin-left: 21px;

        .box{
          display: flex;
          width: 100%;
          color: #333333;
          .item{
            padding:  0 10px;
            p{
              padding: 10px 0;
            }
            &:hover{
              color: #E64347;
              cursor: pointer;

            }
          }
        }

        .active{
          color: #E64347 !important;
          border-bottom: 2px solid #E64347;
        }
      }


    }
  }
  /*多选框*/
  .checked{
    width: 18px;
    height: 18px;
    margin-right: 11px;
  }
</style>
