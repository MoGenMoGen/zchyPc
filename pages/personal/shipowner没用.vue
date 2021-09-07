<template>
<!--船东首页 未使用-->
<div id="home" :style="{width:bWidth+'px'}">
  <Role  :show="show" :title="title" @hideModal="hideModal" @submit="submit"><p>111111</p></Role>
  <div class="container" :style="{width:width+'px'}">
    <div class="personMain">
      <div id="left">
        <left-menu1></left-menu1>
      </div>
      <div id="right">
        <!--首部个人信息-->
        <div class="header">
          <!--左部-->
          <div class="left">
            <div class="up">
              <div class="img">
                <img :src="info.imgUrl" alt=""/>
              </div>
              <p>{{info.realNm}}<span>({{info.mob}})</span></p>
              <p>{{info.company}}&nbsp;<span>[&nbsp;船东&nbsp;]</span></p>

            </div>
            <div class="down">
              <p @click="toPage('./persInfo')">个人信息完善<i class="el-icon-arrow-right"></i></p>
              <button @click="open"><i class="el-icon-refresh"></i>角色转换</button>
            </div>
          </div>
          <!--右部-->
          <div class="right">
            <div class="upp">
              <button><img src="../../assets/img/personal/首页/签到.png" alt=""/>我要签到</button>
              <button><img src="../../assets/img/personal/首页/分享.png" alt=""/>复制链接分享</button>
              <p><img src="../../assets/img/personal/首页/客房用.png" alt=""/>客服电话：0580-30980000</p>
            </div>
            <div class="downn">
              <!--我的收藏-->
              <div class="part">
                <p>20</p>
                <p>我的收藏</p>
              </div>
              <!--我的积分-->
              <div class="part">
                <p>{{info.scoresAmt}}</p>
                <p>我的积分</p>
              </div>
              <!--我的优惠券-->
              <div class="part">
                <p>20</p>
                <p>我的优惠券</p>
              </div>
              <!--我的推荐码-->
              <div class="part">
                <vue-qr :text="url" :size="150" :margin="8" :logoSrc="src1"></vue-qr>
                <p>我的推荐码</p>
              </div>
            </div>
          </div>
        </div>
        <!--下部分内容-->
        <div class="order">
          <div class="left1">
            <!--产品订单-->
            <div class="productOrder">
              <div class="title">
                <p><img src="../../assets/img/personal/首页/产品订单.png" alt=""/>产品订单</p>
              </div>
              <div class="content">
                <div class="section">
                  <img src="../../assets/img/personal/首页/待付款.png" alt=""/>
                  <span class="span">{{number}}</span>
                  <p>待付款</p>
                </div>
                <div class="section">
                  <img src="../../assets/img/personal/首页/待发货.png" alt=""/>
                  <span class="span">{{number}}</span>
                  <p>待发货</p>
                </div>
                <div class="section">
                  <img src="../../assets/img/personal/首页/待收货.png" alt=""/>
                  <span class="span">{{number}}</span>
                  <p>待收货</p>
                </div>
                <div class="section">
                  <img src="../../assets/img/personal/首页/已完成.png" alt=""/>
                  <span class="span">{{number}}</span>
                  <p>已完成</p>
                </div>
                <div class="section">
                  <img src="../../assets/img/personal/首页/全部订单.png" alt=""/>
                  <span class="span">{{number}}</span>
                  <p>全部订单</p>
                </div>
              </div>
            </div>
            <!--船舶订单-->
            <div class="shipOrder">
              <div class="title">
                <p><img src="../../assets/img/personal/首页/船舶订单.png" alt=""/>船舶订单</p>
              </div>
              <div class="content">
                <div class="section">
                  <img src="../../assets/img/personal/首页/待付款.png" alt=""/>
                  <span class="span">{{number}}</span>
                  <p>待付款</p>
                </div>
                <div class="section">
                  <img src="../../assets/img/personal/首页/待发货.png" alt=""/>
                  <span class="span">{{number}}</span>
                  <p>待发货</p>
                </div>
                <div class="section">
                  <img src="../../assets/img/personal/首页/待收货.png" alt=""/>
                  <span class="span">{{number}}</span>
                  <p>待收货</p>
                </div>
                <div class="section">
                  <img src="../../assets/img/personal/首页/已完成.png" alt=""/>
                  <span class="span">{{number}}</span>
                  <p>已完成</p>
                </div>
                <div class="section">
                  <img src="../../assets/img/personal/首页/全部订单.png" alt=""/>
                  <span class="span">{{number}}</span>
                  <p>全部订单</p>
                </div>
              </div>
            </div>
            <!--在线留言-->
            <div class="message">
              <div class="title">
                <p><img src="../../assets/img/personal/首页/留言.png" alt=""/>在线留言</p>
              </div>
              <div class="content1">
                <div class="format">
                  <p>留言内容：</p><textarea row="2" v-model="input" placeholder="可在线提交您的宝贵意见，我们会尽力做到更好。"></textarea>
                </div>
                <div class="format">
                  <p>联系人：</p><input v-model="input2" placeholder="请输入联系人"></input>
                </div>
                <div class="format">
                  <p>联系电话：</p><input v-model="input3" placeholder="请输入联系电话"></input>
                </div>
                <div class="format">
                  <p>联系邮箱：</p><input v-model="input4" placeholder="请输入联系邮箱"></input>
                </div>
                <div class="button1">
                  <button>取消</button>
                  <button>提交</button>
                </div>
              </div>
            </div>
          </div>
          <div class="right1">
            <!--其他服务-->
            <div class="serve">
              <div class="title">
                <p><img src="../../assets/img/personal/首页/更多服务.png" alt=""/>其他服务</p>
              </div>
              <div class="content1">
                <div v-for="(item,index) in serveList" :key="index" class="serveT" @click="toPage(item.href)">
                  <img :src="item.img" alt=""/>
                  <p>{{item.nm}}</p>
                </div>
              </div>
            </div>
            <!--浏览记录-->
            <div class="note">
              <div class="title">
                <p><img src="../../assets/img/personal/首页/浏览.png" alt=""/>浏览记录</p>
              </div>
              <div class="content1">
                <el-carousel indicator-position="outside" class="noteT">
                  <el-carousel-item v-for="item in 4" :key="item">
                    <h3 class="small">{{ item }}</h3>
                  </el-carousel-item>
                </el-carousel>
<!--                <div class="noteT" v-for="item in 6" :key="item">-->
<!--                  <p>{{item}}</p>-->
<!--                </div>-->
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
  <!--救生衣专区-->
  <div class="ad">
    <div class="backgroundByAd" :style="{width:width+'px'}" >
    </div>
  </div>
  <!--看了又看-->
  <div class="featured">
    <div class="backgroundByFeatured" :style="{width:width+'px'}" >
      <div class="featuredHeader">
        <p>看了又看 </p>
        <p><i class="el-icon-refresh-right"></i>换一批</p>
      </div>
      <div class="featuredMain">
        <featured></featured>
      </div>
    </div>
  </div>
  <!--底部图片-->
  <div class="footerImg" :style="{width:bWidth+'px'}">
    <img src="../../assets/img/footer.png" alt=""/>
  </div>

</div>
</template>

<script>
  import {mapState} from "vuex";
  import Featured from "../../components/onlineTrading/featured";
  import LeftMenu1 from "../../components/person/leftMenu1";
  import src1 from "../../assets/img/alter.png";
  import im1 from "../../assets/img/personal/首页/推荐.png";
  import im2 from "../../assets/img/personal/首页/维保服务.png";
  import im3 from "../../assets/img/personal/首页/我的消息.png";
  import im4 from "../../assets/img/personal/首页/修改密码.png";
  import im5 from "../../assets/img/personal/首页/我的预约.png";
  import im6 from "../../assets/img/personal/首页/选型记录.png";
  import Role from "../../components/person/role";
  export default {
    name: "shipowner没用",
    components: {Role, LeftMenu1, Featured},
    data(){
      return{
        info:{},
        url:'111',
        number:'',
        src1,
        title: '角色切换',
        show: false,
        input:'',
        input2:'',
        input3:'',
        input4:'',
        serveList:[{
          nm:'我的推荐',img:im1,href:'./myRecommend'
        },{
          nm:'维保服务',img:im2,href:'/'
        },{
          nm:'我的消息',img:im3,href:'./message'
        },{
          nm:'修改密码',img:im4,href:'./pswAlter'
        },{
          nm:'我的预约',img:im5,href:'/'
        },{
          nm:'选型记录',img:im6,href:'./record'
        }],
      }
    },
    computed:{
      ...mapState([
        'bWidth',
        'width'
      ])
    },
    mounted(){
      this.getData()
    },
    methods:{
      async getData(){
        this.info = await this.api.personInfo()
      },
      toPage(url){
        this.$router.push(url)
      },
      hideModal() {
        // 取消弹窗回调
        this.show = false
      },

      submit() {
        // 确认弹窗回调
        this.show = false
      },
      open(){
        this.show = true
      }

    },
  }
</script>
<style lang="less">
  .el-carousel__item h3 {
    color: #475669;
    font-size: 14px;
    opacity: 0.75;
    line-height: 100%;
    margin: 0;
  }

  .el-carousel__item:nth-child(2n) {
    /*background-color: #99a9bf;*/
  }

  .el-carousel__item:nth-child(2n+1) {
    /*background-color: #d3dce6;*/
  }
  .el-carousel__container{
    height: 90%;
  }
</style>

<style lang="less" scoped>
  @import url("../../assets/css/init.less");
#home{
  background-color: #F3F3F3;
  height: 100%;
  width: 100%;
  display: flex;
  display: -webkit-flex;
  flex-direction: column;
  .container{
    flex: 1;
    margin: 0 auto;
    .personMain{
      display: flex;
      display: -webkit-flex;
      padding-top: 20px;
    }
    #left{
      flex: 1;
      margin-right: 20px;
      background: #ffffff;
    }
    #right{
      background: #F3F3F3;
      flex: 4.9;
      .header{
        width: 100%;
        height: 278px;
        background-color: #ffffff;
        display: inline-flex;
        .left{
          width: 25.5%;
          box-shadow: 0 0 3px 3px rgba(0,0,0,0.10);
          .up{
            height: 208px;
            background-image: url("../../assets/img/personal/首页/头像bg.png");
            background-size: 100% 100%;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            .img{
              width: 90px;
              height: 90px;
              cursor: pointer;
              border-radius: 50%;
              overflow: hidden;
              display: flex;
              display: -webkit-flex;
              >img{
                  margin: auto;
              }
            }

            >p{
              color: #FFFFFF;
              padding-top: 19px;
            }
          }
          .down{
            height: 70px;
            width: 100%;
            display: inline-flex;
            align-items: center;
            justify-content: space-around;
            >p{
              color: #2778BE;
              font-size: 14px;
              cursor: pointer;
            }
            >button{
              border-radius: 14.5px;
              border: 0;
              width: 98px;
              height: 30px;
              color: #FFFFFF;
              background-color: #2778BE;
              cursor: pointer;
            }
          }
        }
        .right{
          width: 74.4%;
          .upp{
            height: 93px;
            width: 100%;
            border-bottom: 1px solid #F3F3F3;
            display: inline-flex;
            align-items: center;
            justify-content: space-around;
            button:nth-child(1){
              border: 1px solid #BCBCBC;
              border-radius: 5px;
              background-color: #FFFFFF;
              width: 116px;
              height: 33px;
              cursor: pointer;
              >img{
                padding-right: 9px;
                height: 16px;
                vertical-align: top;
              }
            }
            button:nth-child(2){
              border: 1px solid #BCBCBC;
              border-radius: 5px;
              background-color: #FFFFFF;
              width: 148px;
              height: 33px;
              margin-right: 18.9%;
              cursor: pointer;
              >img{
                padding-right: 9px;
                height: 15px;
                vertical-align: top;
              }
            }
            img{
              height: 19px;
              vertical-align: middle;
              margin-right: 5px;
            }
          }
          .downn{
            height: 184px;
            width: 100%;
            padding: 37px 0;
            display: inline-flex;
            .part{
              width: 25%;
              border-left: 1px solid #F3F3F3;
              display: flex;
              align-items: center;
              flex-direction: column;
              text-align: center;
              justify-content: center;
              >img{
                height: 89px;
                width: 89px;
                margin-bottom: 11px;
                cursor: pointer;
              }
              p:nth-child(1){
                font-size: 30px;
                color: #E4393C;
                font-weight: bold;
                margin-bottom: 21px;
                cursor: pointer;
              }
              p:nth-child(2){
                font-size: 12px;
                color: #333333;
                cursor: pointer;
              }
            }
          }
        }
      }
      .order{
        width: 100%;
        display: inline-flex;
        justify-content: space-between;
        .left1{
          width: 67.3%;
          .productOrder{
            width: 100%;
            height: 200px;
            margin-top: 31px;
            background-color: #FFFFFF;
            .title{
              width: 100%;
              height: 56px;
              border-bottom: 1px solid #F3F3F3;
              >p{
                font-size: 16px;
                color: #333333;
                line-height: 56px;
                margin-left: 20px;

              }
              img{
                padding-right: 10px;
                height: 19px;
                vertical-align: middle;
              }
            }
            .content{
              width: 100%;
              display: inline-flex;
              height: 142px;
              .section{
                width: 20%;
                display: flex;
                flex-direction: column;
                justify-content: center;
                align-items: center;
                height: 142px;
                /*position: absolute;*/
                .span{
                  position: relative;
                  width: 14px;
                  height: 14px;
                  background-color: #E4393C;
                  border-radius: 50%;
                  left: 17px;
                  bottom: 35px;
                  color: #FFFFFF;
                  font-size: 12px;
                  text-align: center;
                  line-height: 14px;
                }
                >img{
                  cursor: pointer;
                  height: 35px;
                }
                >p{
                  font-size: 12px;
                  color: #333333;
                  cursor: pointer;
                }
              }
            }
          }
          .shipOrder{
            width: 100%;
            height: 200px;
            margin-top: 24px;
            background-color: #FFFFFF;
            .title{
              width: 100%;
              height: 56px;
              border-bottom: 1px solid #F3F3F3;
              >p{
                font-size: 16px;
                color: #333333;
                line-height: 56px;
                margin-left: 20px;
              }
              img{
                padding-right: 10px;
                height: 21px;
                vertical-align: middle;
              }
            }
            .content{
              width: 100%;
              display: inline-flex;
              .section{
                width: 20%;
                display: flex;
                flex-direction: column;
                justify-content: center;
                align-items: center;
                height: 142px;
                .span{
                  position: relative;
                  width: 14px;
                  height: 14px;
                  background-color: #E4393C;
                  border-radius: 50%;
                  left: 17px;
                  bottom: 35px;
                  color: #FFFFFF;
                  font-size: 12px;
                  text-align: center;
                  line-height: 14px;
                }
                >img{
                  cursor: pointer;
                  height: 35px;
                }
                >p{
                  font-size: 12px;
                  color: #333333;
                }
              }
            }
          }
          .message{
            width: 100%;
            background-color: #FFFFFF;
            margin-top: 24px;
            .title{
              width: 100%;
              height: 56px;
              border-bottom: 1px solid #F3F3F3;
              >p{
                font-size: 16px;
                color: #333333;
                line-height: 56px;
                margin-left: 20px;
              }
              img{
                padding-right: 10px;
                height: 19px;
                vertical-align: sub;
              }
            }
            .content1{
              height: 400px;
              .format{
                width: 100%;
                display: inline-flex;
                justify-content: center;
                flex-direction: row;
                align-items: baseline;
                text-align: center;
                cursor: pointer;
                >p{
                  font-size: 15px;
                  color: #666666;
                  width: 93px;
                }
                input{
                  width: 348px;
                  height: 42px;
                  border: 1px solid #DDDDDD;
                  margin-top: 20px;
                  padding-left: 21px;
                  color: #999999;
                }
                textarea{
                  width: 348px;
                  height: 92px;
                  border: 1px solid #DDDDDD;
                  margin-top: 20px;
                  resize: none;
                  padding: 21px;
                  color: #999999;
                }
              }
              .button1{
                margin-top: 30px;
                width: 100%;
                padding-left: 83px;
                display: inline-flex;
                justify-content: center;
                button:nth-child(1){
                  width: 159px;
                  height: 42px;
                  border: 1px solid #DDDDDD;
                  color: #666666;
                  font-size: 16px;
                  background-color: #FFFFFF;
                  cursor: pointer;
                }
                button:nth-child(2){
                  width: 159px;
                  height: 42px;
                  border: 1px solid #2778BE;
                  color: #FFFFFF;
                  font-size: 16px;
                  background-color: #2778BE;
                  margin-left: 20px;
                  cursor: pointer;
                }
              }
            }
          }
        }
        .right1{
          width: 30.6%;
          height: 100%;
          .serve{
            width: 100%;
            background-color: #FFFFFF;
            height: 326px;
            margin-top: 31px;
            .title{
              width: 100%;
              height: 56px;
              border-bottom: 1px solid #F3F3F3;
              >p{
                font-size: 16px;
                color: #333333;
                line-height: 56px;
                margin-left: 20px;

              }
              img{
                padding-right: 10px;
                height: 19px;
                vertical-align: sub;
              }
            }
            .content1{
              width: 100%;
              display: inline-flex;
              flex-wrap: wrap;
              .serveT{
                width: 33.3%;
                height: 117px;
                border: 1px solid #F3F3F3;
                cursor: pointer;
                display: flex;
                flex-wrap: wrap;
                align-items: center;
                text-align: center;
                justify-content: center;
                >p{
                  width: 100%;
                  text-align: center;
                  margin-bottom: 25px;
                }
                >img{
                  height: 24px;
                  margin-top: 25px;
                }
              }
            }
          }
          .note{
            width: 100%;
            background-color: #FFFFFF;
            margin-top: 24px;
            height: 554px;
            .title{
              width: 100%;
              height: 56px;
              border-bottom: 1px solid #F3F3F3;
              >p{
                font-size: 16px;
                color: #333333;
                line-height: 56px;
                margin-left: 20px;
              }
              img{
                padding-right: 10px;
                height: 20px;
                vertical-align: sub;
              }
            }
            .content1{
              width: 100%;
              display: inline-flex;
              flex-wrap: wrap;
              align-items: center;
              justify-content: space-around;
              padding: 20px;
              height: 88%;
              .noteT{
                width: 100%;
                cursor: pointer;
                height: 100%;
              }
            }
          }
        }

      }
    }
  }
  .featured{
    background-color: #F3F3F3;
    margin-top: 21px;
    .backgroundByFeatured{
      background-color: #FFFFFF;
      margin: 0 auto;
      height: 394px;
      border: 1px solid #DEDEDE;
      .featuredHeader{
        display: inline-flex;
        justify-content: space-between;
        padding: 20px 20px 0 20px;
        width: 100%;
        p:nth-child(1){
          color: #343434;
          font-size: 20px;
          line-height: 19px;
        }
        p:nth-child(2){
          color: #333333;
          font-size: 14px;
          line-height: 19px;
          >i{
            color: #AAAAAA;
            margin-right: 9px;
          }
        }
      }
      .featuredMain{
      }
    }
  }
  .ad{
    margin-top: 31px;
    .backgroundByAd{
      margin: 0 auto;
      background-color: #FFFFFF;
      height: 102px;
      border: 1px solid #DEDEDE;
    }
  }
  .footerImg{
    width: 100%;
    margin-top: 53px;
  }


}



</style>
