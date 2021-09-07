<template>
    <div id="app">
        <div class="mainView">
            <div class="productQ" v-for="(item,index) in productList" :key="index" :class="{seen:item.isSeen}" @click="toProDetail(item)"
                 @mouseenter="enter(index)" @mouseleave="leave(index)">
                <div class="img" :style="{height:width*0.196*0.827*shipSize2+'px'}" v-if="ifShip">
                    <img :src="item.img" :alt="item.nm"/>
                </div>
              <div class="img" :style="{height:width*0.196*0.827*proSize+'px'}" v-else>
                <!--<img :src="item.img" :alt="item.nm"/>-->
                <!-- < el-image
                  style="width: 100%; height: 100%"
                  :src="item.img"
                  fit="cover"></el-image> -->
                  <img :src="item.img"
                       class="el-images"
                       :ref="'img'+index"
                  />
              </div>
                <div class="overall">
                  <el-tooltip :content="item.nm" placement="top" effect="light">
                    <p class="child1">{{item.nm}}</p>
                  </el-tooltip>
                    <div class="space">
                        <p class="child2" v-if="item.goodsMinPrice!=price && item.goodsMinPrice !== item.goodsMaxPrice">￥<span class="child3">{{item.goodsMinPrice}}-{{item.goodsMaxPrice}}</span></p>
                        <p class="child2" v-if="item.goodsMinPrice!=price && item.goodsMinPrice === item.goodsMaxPrice">￥<span class="child3">{{item.goodsMinPrice}}</span></p>
                        <p class="child2" v-if="item.goodsMinPrice==price">价格面议</p>
                        <button v-if="item.isCoupon0 === 1">满减</button>
                        <button v-if="item.isCoupon1 === 1">折扣</button>
                        <button v-if="item.isCoupon2 === 1">代金</button>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    import {mapState} from "vuex";

    export default {
        name: "productList",
        props: ['list','ifShip'],
        computed: {
            ...mapState([
                'width',
                'proSize',
                'shipSize2',
              'price'
            ])
        },
        watch: {
            list: {
                handler: function () {
                    this.productList = JSON.parse(JSON.stringify(this.list))
                    this.productList.forEach((item, index) => {
                      this.objectFitImages(this.$refs['img'+index])
                        item.origPrice = item.origPrice.toString()
                        if (item.origPrice) {
                            item.price1 = item.origPrice.split('.')[0]
                            item.price2 = item.origPrice.split('.')[1] ? '.' + item.origPrice.split('.')[1] : ''
                        }
                        this.$set(this.productList, index, item)
                    })
                },
                deep: true
            }
        },
        data() {
            return {
                productList: [],
            }
        },
        mounted() {
          // this.getData()
        },
        methods: {
            enter(index) {
                this.productList[index].isSeen = true
                this.$set(this.productList, index, this.productList[index])
            },
            leave(index) {
                this.productList[index].isSeen = false
                this.$set(this.productList, index, this.productList[index])
            },
            //到产品详情
            toProDetail(item) {
              let url =  './Trade/productDetail?id='+item.goodsId+''

               window.open(url)
                // this.$router.push({
                //     path: '../Trade/productDetail',
                //     query: {
                //         id: item.goodsId,
                //         type: item.ship ? 'ship' : 'product'
                //     }
                // })
            },
        }
    }
</script>

<style lang="less" scoped>
    @import url("../../assets/css/init.less");

    #app {

    }

    .mainView {
        margin-top: 10px;
        width: 100%;
        overflow: hidden;
        .seen {
            box-shadow: 0 0 3px 3px rgba(0, 0, 0, 0.08);
        }

        //商品具体的div
        .productQ {
            float: left;
            display: flex;
            flex-direction: column;
            /*justify-content: space-around;*/
            width: 19.6%;
            background-color: #ffffff;
            margin-bottom: 10px;
            cursor: pointer;
            margin-right: 0.5%;
            box-sizing: border-box;
            &:nth-of-type(5n+5) {
                margin-right: 0;
            }

            .img {
                width: 82.7%;
                margin: 20px auto;
                display: flex;
                display: -webkit-flex;

                > img {
                    margin: auto;
                  object-fit:contain;
                }
            }

            .overall {
                height: 100px;
                display: flex;
                flex-wrap: wrap;
                justify-content: flex-start;
                text-align: left;
                width: 190px;
                margin: 0 auto;

                > p {
                    padding-bottom: 20px;
                    color: #666666;
                    white-space:nowrap;
                    overflow:hidden;
                    text-overflow:ellipsis;
                }

                .space {
                    display: inline-flex;
                    justify-content: space-between;
                    width: 100%;
                    //￥符号
                    .child2 {
                        color: #FF3E3E;
                    }

                    //小数点前数字
                    .child3 {
                        color: #FF3E3E;
                        font-size: 20px;
                    }

                    //小数点后
                    .child4 {
                        color: #FF3E3E;
                        font-size: 18px;
                    }

                    button {
                        width: 42px;
                        height: 20px;
                        border: 1px solid #FF3E3E;
                        color: #FF3E3E;
                        background-color: #FFFFFF;
                        border-radius: 2px;
                        font-size: 14px;
                    }
                }
            }
        }
    }
</style>
