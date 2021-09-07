<template>
<!--首页-->
<div id="home" :style="{width:bWidth+'px'}">
  <Role  :show="show" :title="title" @hideModal="hideModal" @submit="submit" ></Role>
  <div class="container" :style="{width:width+'px'}">
    <div class="personMain">
      <div id="left">
        <left-menu1 v-if="!currentRole || !currentRole.identityCd" :msgNum="msgNum"></left-menu1>  <!-- 没有入驻 -->
        <left-menu1 v-if="currentRole &&currentRole.identityCd=='identity10'" :msgNum="msgNum"></left-menu1>  <!-- 船东 -->
        <left-menu2 v-if="currentRole &&currentRole.identityCd=='identity20'" :msgNum="msgNum"></left-menu2>  <!-- 设计院 -->
        <left-menu3 v-if="currentRole &&currentRole.identityCd=='identity70'" :msgNum="msgNum"></left-menu3>  <!-- 经销商 -->
        <left-menu4 v-if="currentRole &&currentRole.identityCd=='identity50'" :msgNum="msgNum"></left-menu4>  <!-- 检验检测机构 -->
        <left-menu5 v-if="currentRole &&currentRole.identityCd=='identity40'" :msgNum="msgNum"></left-menu5>  <!-- 监理监造机构 -->
        <left-menu6 v-if="currentRole &&currentRole.identityCd=='identity60'" :msgNum="msgNum"></left-menu6>  <!-- 供应商 -->
        <left-menu7 v-if="currentRole &&currentRole.identityCd=='identity80'" :msgNum="msgNum"></left-menu7>  <!-- 代理商 -->
        <left-menu8 v-if="currentRole &&currentRole.identityCd=='identity30'" :msgNum="msgNum"></left-menu8>  <!-- 船厂 -->
        <left-menu9 v-if="currentRole &&currentRole.identityCd=='identity90'" :msgNum="msgNum"></left-menu9> <!-- 服务商 -->
      </div>
      <div id="right" :style="{width:(width-20)*4.9/5.9+'px'}">
        <!--首部个人信息-->
        <div class="header">
          <!--左部-->
          <div class="left">
            <div class="up">
<!--              会员角标，暂时注释-->
<!--              <img class="yellow" src="@/assets/img/personal/首页/角标.png"/>-->
              <div class="img">
                <img :src="info.imgUrl" alt="" :style="{height:(width-20)*4.9/5.9*0.255*0.36 + 'px'}" />
              </div>
              <p>{{info.realNm}}<span>({{info.mob}})</span></p>
              <p>{{currentRole ? currentRole.company : info.company}}&nbsp;<span v-if="currentRole">[{{currentRole.identityNm}}]</span></p>
            </div>
            <div class="down">
              <p @click="toPage('./personal/persInfo')">个人信息完善<i class="el-icon-arrow-right"></i></p>
              <button @click="open"><i class="el-icon-refresh"></i>角色转换</button>
            </div>
          </div>
          <!--右部-->
          <div class="right">
            <div class="upp">
              <button @click="toPage('./personal/signIn')"><img src="../../assets/img/personal/首页/签到.png" alt=""/>我要签到</button>
              <button @click="copyUrl(url)"><img src="../../assets/img/personal/首页/分享.png" alt=""/>复制链接分享</button>
              <p ><img src="../../assets/img/personal/首页/客房用.png" alt=""/>客服电话：{{tel}}</p>
            </div>
            <div class="downn">
              <!--我的收藏-->
              <div class="part" @click="toPage('./personal/collect')">
                <p>{{coll}}</p>
                <p>我的收藏</p>
              </div>
              <!--我的预约-->
              <div class="part" @click="toPage('./personal/subscribe')">
                <p>{{subscrN}}</p>
                <p>我的预约</p>
              </div>
              <!--选型记录-->
              <div class="part" @click="toPage('./personal/record')">
                <p>{{recordN}}</p>
                <p>选型记录</p>
              </div>
              <!--我的积分-->
              <!-- <div class="part" @click="toPage('./integral')">
                <p>{{info.scoresAmt ? info.scoresAmt : 0}}</p>
                <p>我的积分</p>
              </div> -->
              <!--我的优惠券-->
              <!-- <div class="part" @click="toPage('./coupon')">
                <p>0</p>
                <p>我的优惠券</p>
              </div> -->
              <!--我的推荐码-->
              <div class="part">
                <vue-qr :text="url" :size="150" :margin="8" :logoSrc="qrImg"></vue-qr>
                <p>我的推荐码</p>
                <P class="share" @click="copyUrl(info.inviCd)" ><span>{{info.inviCd}}</span>点击复制</P>
              </div>
            </div>
          </div>
        </div>
        <!--下部分内容-->
        <div class="order">
          <!--船东-->
          <index1  v-if="(currentRole && currentRole.identityCd == 'identity10') || !currentRole"></index1>
          <!--设计院-->
          <index2  v-if="currentRole && currentRole.identityCd=='identity20'"></index2>
          <!-- 经销商 -->
          <index3 v-if="currentRole && currentRole.identityCd=='identity70'" ></index3>
          <!-- 检验检测机构 -->
          <index4 v-if="currentRole &&currentRole.identityCd=='identity50'"></index4>
          <!-- 监理监造机构 -->
          <index5 v-if="currentRole && currentRole.identityCd=='identity40'"></index5>
          <!-- 供应商 -->
          <index6 v-if="currentRole && currentRole.identityCd=='identity60'"></index6>
          <!-- 代理商 -->
          <index7 v-if="currentRole && currentRole.identityCd=='identity80'"></index7>
          <!-- 船厂 -->
          <index8 v-if="currentRole && currentRole.identityCd=='identity30'"></index8>
          <!--服务商-->
          <index9 v-if="currentRole && currentRole.identityCd=='identity90'"></index9>
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
                <el-carousel indicator-position="outside" class="noteT" :autoplay="false">
                  <el-carousel-item v-for="(item,index) in noteList" :key="index">
                    <div class="noteList">
                      <!-- <p v-for="(v,i) in item" :key="i">
                        <img
                            :src="v.goodsImg"
                            :alt="v.goodsNm"
                             :title="v.goodsNm"
                             :ref="'img'+index+i"
                             @click="toProDetail(v)"/>
                      </p> -->
                      <img :src="v.goodsImg"
                           :alt="v.goodsNm"
                           v-for="(v,i) in item"
                           :key="i"
                           :title="v.goodsNm"
                           :ref="'img'+index+i"
                           @click="toProDetail(v)"/>
                    </div>
                  </el-carousel-item>
                </el-carousel>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
  <!--广告专区-->
  <div class="ad" :style="{width:width+'px'}">
    <el-carousel :height="bannerHeight+'px'" class="adert">
      <el-carousel-item v-for="item in adertList" :key="item.aiid">
        <div class="bannerImg" :style="{backgroundImage:'url('+item.imgUrl+')'}"  @click="toPage(item.locUrl)"/>
      </el-carousel-item>
    </el-carousel>
  </div>
  <!--看了又看-->
  <div class="featured">
    <div class="backgroundByFeatured" :style="{width:width+'px'}" >
      <div class="featuredHeader">
        <p>看了又看 </p>
        <p  @click="getLook()"><i class="el-icon-refresh-right"></i>换一批</p>
      </div>
      <div class="featuredMain">
        <featured :list="lookList" type="product"></featured>
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
  import featured from "@/components/onlineTrading/featured";
  const leftMenu1 = resolve => require(['@/components/person/leftMenu1'],resolve); //船东
  const leftMenu2 = resolve => require(['@/components/person/leftMenu2'],resolve); //设计院
  const leftMenu3 = resolve => require(['@/components/person/leftMenu3'],resolve); //经销商
  const leftMenu4 = resolve => require(['@/components/person/leftMenu4'],resolve); //检验检测机构
  const leftMenu5 = resolve => require(['@/components/person/leftMenu5'],resolve); //监理监造机构
  const leftMenu6 = resolve => require(['@/components/person/leftMenu6'],resolve); //供应商
  const leftMenu7 = resolve => require(['@/components/person/leftMenu7'],resolve); //代理商
  const leftMenu8 = resolve => require(['@/components/person/leftMenu8'],resolve); //船厂
  const leftMenu9 = resolve => require(['@/components/person/leftMenu9'],resolve); //船东

  const index1 = resolve => require(['@/components/person/index1'],resolve); //船东
  const index2 = resolve => require(['@/components/person/index2'],resolve); //设计院
  const index3 = resolve => require(['@/components/person/index3'],resolve); //经销商
  const index4 = resolve => require(['@/components/person/index4'],resolve); //检验检测机构
  const index5 = resolve => require(['@/components/person/index5'],resolve); //监理监造机构
  const index6 = resolve => require(['@/components/person/index6'],resolve); //供应商
  const index7 = resolve => require(['@/components/person/index7'],resolve); //代理商
  const index8 = resolve => require(['@/components/person/index8'],resolve); //船厂
  const index9 = resolve => require(['@/components/person/index9'],resolve); //服务商


  import src1 from "../../assets/img/alter.png";
  import im1 from "../../assets/img/personal/首页/推荐.png";
  import im2 from "../../assets/img/personal/首页/维保服务.png";
  import im3 from "../../assets/img/personal/首页/我的消息.png";
  import im4 from "../../assets/img/personal/首页/修改密码.png";
  import im5 from "../../assets/img/personal/首页/我的预约.png";
  import im6 from "../../assets/img/personal/首页/选型记录.png";
  import Role from "../../components/person/role";
  export default {
    name: "shipowner",
    components: {
      Role,
      leftMenu1,leftMenu2,leftMenu3,leftMenu4,leftMenu5,leftMenu6,leftMenu7,leftMenu8,leftMenu9,
      index1,index2,index3,index4,index5,index6,index7,index8,index9,
      featured
    },
    data(){
      return {
        coll:0, //收藏总数
        subscrN:0,//我的预约总数
        recordN:0,//选型报价总数
        info: {},
        url: '',
        qrImg:'',
        number: 0,
        src1,
        title: '角色切换',
        show: false,

        serveList: [{
          nm: '我的推荐', img: im1, href: './personal/myRecommend'
        }, {
          nm: '维保服务', img: im2, href: './personal/repair'
        }, {
          nm: '我的消息', img: im3, href: './personal/message'
        }, {
          nm: '修改密码', img: im4, href: './personal/pswAlter'
        }, {
          nm: '我的预约', img: im5, href: './personal/subscribe'
        }, {
          nm: '选型记录', img: im6, href: './personal/record'
        }],
        adertList:[],
        bannerHeight:0,
        noteList: [],
        lookList:[],
      }

    },
    computed:{
      ...mapState([
        'bWidth',
        'width',
        "currentRole",
        'loading',
        'tel',
        'msgNum'
      ])
    },
    watch:{

      width(){
        if(this.adertList.length>0){
          let myImg = new Image()
          myImg.src = this.adertList[0].imgUrl
          myImg.onload = () =>{
            this.bannerHeight = this.width*0.5*myImg.height/myImg.width
          }
        }
      },
      currentRole:{
        handler:function (v){
          if(!v){

            return
          }else {
            if(v.identityCd=='identity90'){//服务商
              // console.log('服务商')
              this.serveList[1].href = './personal/repairService'
            }else {
              this.serveList[1].href = './personal/repair'
            }
          }


          // if(v.identityCd=='identity20'){//设计
          //   // console.log('服务商')
          //   this.serveList[1].href = './personal/seleQuo'
          // }else {
          //   this.serveList[1].href = './personal/record'
          // }
        },
        immediate:true
      }
    },
    mounted(){
      this.getData()
    },
    methods:{
      async getData(){

        this.info = await this.api.personInfo()
        //收藏总数
        this.coll = await this.api.favoriteTotal()
        //我的预约总数
        let qry1=this.query.new()
        this.query.toP(qry1,'1','1')
        let subscr = await this.api.subscribeList(this.query.toEncode(qry1))
        this.subscrN=subscr.page.total
        //选型记录总数
        let qry2=this.query.new()
        this.query.toP(qry2,'1','1')
        let record={}
        if(this.currentRole.identityCd=='identity20'){//设计院
           record = await this.api.xuanxingList(this.query.toEncode(qry2))
        }else {
          record = await this.api.diyList(this.query.toEncode(qry2))
        }
        this.recordN=record.page.total
        // console.log('我的预约:'+this.subscrN)
        // console.log('选型记录:'+this.recordN)
        this.api.msgUnRead()
        // console.log('未读消息：'+this.msgNum)
        this.url = window.location.origin+'/sinovat/?inviCd='+this.info.inviCd
        this.adertList = await this.api.getAdert('personAdert')
        if(this.adertList.length>0){
          let myImg = new Image()
          myImg.src = this.adertList[0].imgUrl
          myImg.onload = ()=>{
            this.bannerHeight = myImg.height
          }
        }
        //浏览记录
        let qry = this.query.new()
        this.query.toP(qry,1,18)
        let data = await this.api.viewHistory(this.query.toEncode(qry))
        while (data.length>0){
          this.noteList.push(data.splice(0,6))
        }
        this.noteList.forEach((item,index)=>{
          item.forEach((v,i)=>{
              this.objectFitImages(this.$refs['img'+index+i])
          })
        })
        this.getLook()

      },
      //看了又看
      async getLook(){
        this.lookList = await this.api.personLook()

      },
      //到产品详情
      toProDetail(item) {
        this.$router.push({
          path: '../Trade/productDetail',
          query: {
            id: item.goodsId,
            type: item.ship ? 'ship' : 'product'
          }
        })
      },
      toPage(url){
        if(url){
          // this.$router.push(url)
          window.location.href = url
        }
      },
      //角色转换弹窗
      open(){
        this.show = true
      },
      //角色转换关闭
      hideModal() {
        // 取消弹窗回调
        this.show = false
      },

      //角色关闭
      submit() {
        // 确认弹窗回调
        this.show = false
      },
      //复制
      copyUrl(url){
        var oInput = document.createElement('input');     //创建一个隐藏input
        console.log(oInput);
        oInput.value = url;    //赋值
        console.log(oInput.value);
        document.body.appendChild(oInput);
        oInput.select(); // 选择对象
        document.execCommand("Copy"); // 执行浏览器复制命令
        oInput.className = 'oInput';
        oInput.style.display='none';
        this.$message({
          message: '复制成功！',
          type: 'success',
          duration:1500
        });
      }
    },
  }
</script>

<style lang="less">
  .el-carousel__item h3 {
    font-size: 14px;
    opacity: 0.75;
    line-height: 100%;
    margin: 0;
  }
  .el-carousel__item:nth-child(2n){
  }
  .el-carousel__item:nth-child(2n+1){
  }
  .el-carousel__container{
    height: 90%;
  }
  .status .el-input__inner{
    height: 36px;
    line-height: 36px;
  }
  .content1{
    .btnList{
      display: flex;
      justify-content: space-around;
      flex-direction: column;
      .button3{
        width: 74px;
        height: 31px;
        background-color: #2778BE;
        color: #FFFFFF;
        border: none;
        border-radius: 3px;
        margin-bottom: 15px;
        cursor: pointer;
      }
      .button4{
        border: none;
        width: 74px;
        height: 31px;
        color: #2778BE;
        font-size: 15px;
        background-color: #FFFFFF;
        margin-top: 5px;
        border-radius: 3px;
        cursor: pointer;
      }
      .button2{
        width: 74px;
        height: 31px;
        background-color: #FFFFFF;
        color: #2778BE;
        border-radius: 3px;
        border: 1px solid #2778BE;
        cursor: pointer;

      }
    }
    .el-table__header thead{
      color: #666666;
      font-size:14px;
    }
    .el-table__header th{
      background-color: #F7F7F7;

    }
    .el-table__body tr{
      color: #333333;
      font-size:15px;
      height: 117px;
    }
    .el-table__fixed-right{
      height: 100%!important;
    }
  }
  .content2{
    //选中时更换字体 背景颜色   需设置高亮才能生效
    .el-table__body tr.current-row>td{
      background-color: #FFFFFF !important;
      color: #2778BE;
    }
    .red{
      color: #E3393C;
    }
    .blue{
      color: #2778BE;
    }
    .btnList{
      display: flex;
      justify-content: space-around;
      flex-direction: column;
      .button3{
        width: 74px;
        height: 31px;
        background-color: #2778BE;
        color: #FFFFFF;
        border: none;
        border-radius: 3px;
      }
    }
    .el-table__header thead{
      color: #666666;
      font-size:14px;
    }
    .el-table__header th{
      background-color: #F7F7F7;
      height: 52px;
    }
    .el-table__body tr{
      color: #333333;
      font-size:15px;
      height: 77px;
    }
  }
  .message{
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
    textarea::-webkit-input-placeholder {
      /* Chrome/Opera/Safari */
      color: #999;
    }

    textarea::-moz-placeholder {
      /* Firefox 19+ */
      color: #999;
    }

    textarea:-ms-input-placeholder {
      /* IE 10+ */
      color: #999;
    }

    textarea:-moz-placeholder {
      /* Firefox 18- */
      color: #999;
    }
  }


</style>
<style lang="less" scoped>
  @import url("../../assets/css/init.less");
#home{
  background-color: #F3F3F3;
  /*height: 100%;*/
  width: 100%;
  /*display: flex;*/
  /*display: -webkit-flex;*/
  /*flex-direction: column;*/
  .container{
    /*flex: 1;*/
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
      display: flex;
      display: -webkit-flex;
      flex-direction: column;
      //首部个人信息
      .header{
        width: 100%;
        height: 278px;
        background-color: #ffffff;
        display: flex;
        display: -webkit-flex;
        //左边部分
        .left{
          width: 25.5%;
          box-shadow: 0 0 3px 3px rgba(0,0,0,0.10);
          //上部背景头像、名称
          .up{
            width: 100%;
            height: 208px;
            background-image: url("../../assets/img/personal/首页/头像bg.png");
            background-size: 100% 100%;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            position: relative;
            .yellow{
              position: absolute;
              width: 32px;
              bottom: 183px;
              right: 10px;
            }
            .img{
              /*width: 90px;*/
              /*height: 90px;*/
              width: 36%;
              cursor: pointer;
              /*position: absolute;*/
              /*top: 30px;*/
              /*left: 80px;*/
              overflow: hidden;
              display: flex;
              display: -webkit-flex;
              >img{
                border-radius: 50%;
                overflow: hidden;
                display: flex;
                width: 100%;
                object-fit: cover;
              }
            }
            >p{
              color: #FFFFFF;
              padding-top: 19px;
            }
          }
          //后面身份转换
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
        //首部右部分内容
        .right{
          width: 74.4%;
          //上部签到、链接、客服电话
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
          //下部收藏、积分、优惠券、推荐码
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
              cursor: pointer;
              .share{
                font-size: 12px;
                color: #333333;
                >span{
                  font-size: 13px;
                  margin-right: 16px;
                }
              }
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
      //订单总
      .order{
        width: 100%;
        display: inline-flex;
        justify-content: space-between;

        .right1{
          width: 30.6%;
          /*height: 100%;*/
          display: flex;
          flex-direction: column;
          justify-content: space-between;
          //其他服务板块
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
          //浏览记录板块
          .note{
            width: 100%;
            background-color: #FFFFFF;
            margin-top: 24px;
            flex: 1;
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
              height: 90%;
              box-sizing: border-box;
              .noteT{
                width: 100%;
                height: 100%;
                .noteList{
                  // >p{
                  //   background-color: #ffffff;
                  //   width: 48%;
                  //   float: left;
                  //   height: 140px;
                  //   cursor: pointer;
                  //   margin-bottom: 10px;
                  //   img{
                  //     width: 100%;
                  //     max-width: 100%;
                  //     height: 100%;
                  //     display: block;
                  //     object-fit: contain;
                  //     &:nth-of-type(2n+1){
                  //       margin-right: 2%;
                  //     }
                  //   }
                  // }
                  img{
                    cursor: pointer;
                    width: 48%;
                    display: block;
                    float: left;
                    height: 140px;
                    margin-bottom: 10px;
                    object-fit: cover;
                    &:nth-of-type(2n+1){
                      margin-right: 2%;
                    }
                  }
                }
              }
            }
          }
        }

      }
    }
  }
  //精选好货
  .featured{
    background-color: #F3F3F3;
    margin-top: 21px;
    .backgroundByFeatured{
      background-color: #FFFFFF;
      margin: 0 auto;
      height: 394px;
      border: 1px solid #DEDEDE;
      .featuredHeader{
        display: flex;
        display: -webkit-flex;
        padding: 20px 20px 0 20px;
        width: 100%;
        p:nth-child(1){
          color: #343434;
          font-size: 20px;
          line-height: 19px;
          flex: 1;
        }
        p:nth-child(2){
          color: #333333;
          font-size: 14px;
          line-height: 19px;
          cursor: pointer;
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
  //广告
  .ad{
    margin: 25px auto 0 auto ;
    .adert{
      width: 100%;
      .bannerImg{
        height: 100%;
        background-position: center;
        cursor: pointer;
      }
    }
  }
  //底部花纹
  .footerImg{
    width: 100%;
    margin-top: 53px;
  }
}



</style>
