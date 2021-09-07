<template>
  <!-- 补贴在线申请 -->
<div id="home" :style="{width:bWidth + 'px'}">
  <div class="header">
    <el-carousel :height="bannerHeight+'px'">
      <el-carousel-item v-for="item in adertList" :key="item.id">
        <img :src="item.imgUrl" style="width:100%">
      </el-carousel-item>
    </el-carousel>
    <el-breadcrumb separator-class="el-icon-arrow-right" :style="{width:width+'px'}" class="bc main">
      <el-breadcrumb-item :to="{ path: './pilotService' }">试点服务</el-breadcrumb-item>
      <el-breadcrumb-item ><p class="textRed">补贴申请</p></el-breadcrumb-item>
    </el-breadcrumb>
  </div>
  <div class="applyBox main" :style="{width:width+'px'}">
    <p class="headTitle">财政补助资金申请表</p>
    <div class="msgBox">
      <div class="inputBox"><span>企业名称: </span> <input type="text" placeholder="单行输入" v-model="form.company"></div>
      <div class="inputBox"><span>项目名称: </span> <input type="text" placeholder="单行输入" v-model="form.projNm"></div>
      <div class="inputBox"><span>所属县(区): </span> <input type="text" placeholder="单行输入" v-model="form.county"></div>
      <div class="inputBox"><span>所属街道: </span> <input type="text" placeholder="单行输入" v-model="form.street"></div>
      <div class="inputBox"><span>项目地址: </span> <input type="text" placeholder="单行输入" v-model="form.address"></div>
      <div class="inputBox"><span>产权性质: </span> <input type="text" placeholder="单行输入" v-model="form.propertyRight"></div>
      <div class="inputBox"><span>产权证号: </span> <input type="text" placeholder="原渔船产权证号" v-model="form.oldCertNo"></div>
      <div class="inputBox"><span>证书时间: </span>
       <el-date-picker
            v-model="form.certTm"
            type="date"
            placeholder="获得产权证书时间">
          </el-date-picker></div>
      <div class="inputBox"><span>建造时间: </span>
        <el-date-picker
             v-model="form.buildTm"
             type="date"
             placeholder="原渔船建造时间">
           </el-date-picker></div>
      <div class="inputBox"><span>已用年限: </span> <input type="number" placeholder="原渔船已经用年限" v-model="form.used"></div>
      <div class="inputBox"><span>联系人: </span> <input type="text" placeholder="单行输入" v-model="form.linkman"></div>
      <div class="inputBox"><span>开户名称: </span> <input type="text" placeholder="单行输入" v-model="form.accountNm"></div>
      <div class="inputBox"><span>联系电话: </span> <input type="number" placeholder="单行输入" v-model="form.mob"></div>
      <div class="inputBox"><span>开户银行: </span> <input type="text" placeholder="单行输入" v-model="form.accountBank"></div>
      <div class="inputBox" style="padding-right:0;">
        <span>补助金额: </span> <input type="number" placeholder="单行输入" v-model="form.subsidyAmt"><span style="width: 30px;">万元</span>
      </div>
      <div class="inputBox"><span>银行账号: </span> <input type="text" placeholder="补助资金转入银行账号" v-model="form.accountNo"></div>
    </div>
    <div class="title">
      <img :src="headLeft" alt=""><p>渔船改造后技术参数</p><img :src="headRight" alt="">
    </div>
    <div class="msgBox">
      <div class="inputBox"><span>总       长: </span> <input type="number" placeholder="单行输入" v-model="form.totalLen"></div>
      <div class="inputBox"><span>型       宽: </span> <input type="number" placeholder="单行输入" v-model="form.width"></div>
      <div class="inputBox"><span>吃       水: </span> <input type="number" placeholder="单行输入" v-model="form.loadDraught"></div>
      <div class="inputBox"><span>主机功率: </span> <input type="number" placeholder="单行输入" v-model="form.hostPower"></div>
      <div class="inputBox"><span>航       速 : </span> <input type="number" placeholder="单行输入" v-model="form.speed"></div>
      <div class="inputBox"><span>定       员 : </span> <input type="number" placeholder="单行输入" v-model="form.complement"></div>
      <p class="redText">* 企业法定代表人（船东）确认（船东对提交材料的真实性负责）</p>
      <div class="inputBox"><span>船舶证书号: </span> <input type="text" placeholder="船舶证产权证书号" v-model="form.newCertNo"></div>
      <div class="inputBox"><span>船东姓名: </span> <input type="text" placeholder="单行输入" v-model="form.shipowner"></div>
      <div class="inputBox"><span>联系电话: </span> <input type="number" placeholder="单行输入" v-model="form.shipownerMob"></div>
    </div>
    <div class="btnBox">
      <button class="cancel" @click="toCancel">取消</button>
      <button class="confirm" @click="toConfirm">确认</button>
    </div>
  </div>
  <div class="footerImg" :style="{width:bWidth+'px'}">
    <img src="@/assets/img/footer.png" alt=""/>
  </div>
</div>
</template>

<script>
  import {mapState} from "vuex";
    import headRight from "@/assets/img/personal/右.png"
    import headLeft from "@/assets/img/personal/左.png"
  export default {
        name: "vision",
      data(){
          return{
            headRight,
            headLeft,
            bannerHeight:'300',
            navImgHeight:'160',
            tabImgHeight:'285',
            adertList:[],
            form:{
              company:"",
              projNm:"",
              county:"",
              street:"",
              address:"",
              propertyRight:"",
              oldCertNo:"",
              certTm:"",
              buildTm:"",
              used:"",
              linkman:"",
              accountNm:"",
              mob:"",
              accountBank:"",
              subsidyAmt:"",
              accountNo:"",
              totalLen:"",
              width:"",
              loadDraught:"",
              hostPower:"",
              speed:"",
              complement:"",
              newCertNo:"",
              shipowner:"",
              shipownerMob:"",
              // attachment:"",
            },
          }
      },
      computed: {
        ...mapState([
          'bWidth',
          'width',
          'currentRole'
        ])
      },
    mounted() {
      this.getAdert()
    },
    methods:{
      toCancel(){
        console.log('取消了！！！！')
        this.$confirm('确定取消补助资金申请？', '提示', {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning'
        }).then(() => {
          this.$router.push('../pilotService/pilotService')
        })
      },
      toConfirm(){
        console.log(this.form)
        for(let key  in this.form){
          if(!this.form[key]){
            console.log(this.form[key])
            this.$message({
              message: '请确认申请表是否填写完整',
              type: 'warning',
              duration: '1500',
            });
            return
          }
        }
        console.log('完整了')
        this.$confirm('是否确认提交申请表？', '提示', {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning'
        }).then(() => {
          this.api.subsidiesAdd(this.form).then(res=>{
            if(res.code==0){
              this.$message({
                message: '提交成功',
                type: 'success',
                duration: '1000',
              })
              setTimeout(() => {
                this.toPage('../pilotService/onlineInquiry')
              }, 1000)
            }
          })
        })
      },
      async getAdert(){
        this.adertList = await this.api.getAdert('subsidies')
        if(this.adertList.length>0){
          let myImg = new Image()
          myImg.src = this.adertList[0].imgUrl
          myImg.onload = ()=>{
            if(myImg.width>this.bWidth){
              this.bannerHeight = myImg.height/myImg.width*this.bWidth
            }else {
              this.bannerHeight = myImg.height
            }
          }
        }
        this.getHeight()
      },
      getHeight(){
        this.navImgHeight=this.width*0.24*9/16;
        this.tabImgHeight=this.width*0.24;
        console.log('nav : '+this.navImgHeight+'tab : '+this.tabImgHeight)
      },
      toPage(url){
        this.$router.push(url)
      }
    }
  }
</script>
<style lang="less">
  .inputBox{
    .el-date-editor{
      flex: 1;
      margin-right: 11px;
      .el-input__inner{
        border-radius: 0;
      }
    }
  }
</style>
<style lang="less" scoped>
  @import url("../../assets/css/init.less");
#home{
  background-color: #FFFFFF;
  .header{
    .bc{
      height: 52px;
      display: flex;
      align-items: center;
      .textRed{
        color: #0064B2;
      }
    }
    .ad{
      margin-top: 11px;
    }
  }
  .applyBox{
    .headTitle{
      font-size: 24px;
      color: #2F2F2F;
      text-align: center;
      padding: 29px 0;
      border-bottom: 1px solid #DFDFDF;
      margin-bottom: 29px;
    }
    .msgBox{
      display: flex;
      flex-wrap: wrap;
      .inputBox{
        width: 33.33%;
        display: flex;
        align-items: center;
        margin-bottom: 20px;
        padding-right: 30px;
        >span{
          width: 100px;
        }
        >input{
          height: 42px;
          flex: 1;
          border: 1px solid #DDDDDD;
          padding: 21px;
          margin-right: 11px;
        }
      }
      .redText{
        color: #FF3333;
        width: 100%;
        font-size: 15px;
        margin-bottom: 32px;
      }
    }
    .btnBox{
      padding-right: 30px;
      display: flex;
      justify-content: flex-end;
      margin-top: 60px;
      >button{
        width: 192px;
        height: 52px;
        cursor: pointer;
      }
      .cancel{
        border: 1px solid #2777BD;
        background-color: #ffffff;
        color: #2777BD;
        margin-right: 20px;
      }
      .confirm{
        background: #2777BD;
        color: #ffffff;
        border: none;
      }
    }
    .title{
      display: flex;
      align-items: center;
      justify-content: center;
      margin-bottom: 41px;
      padding-top: 40px;
      >p{
        font-size:20px;
        color:#2F2F2F;
        margin: 0 30px;
      }
      >img{
        width: 14.8%;
      }
    }
  }
  .footerImg{
    margin-top: 50px;
  }
}
</style>
