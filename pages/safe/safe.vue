<template>
<div id="home" :style="{width:bWidth + 'px'}">
  <div class="header">
    <div class="main" :style="{width:width+'px'}">
      <div class="apply">
        <p>申请提交</p>
        <form  :model="form">
          <div>
            <img src="@/assets/img/safe/单位名称.png"/>
            <input v-model="form.company" placeholder="单位名称" >
          </div>
          <div>
            <img src="@/assets/img/safe/融资金额.png"/>
            <input v-model="form.amt" placeholder="融资金额 / 万元">
          </div>
          <div>
            <img src="@/assets/img/safe/联系人.png"/>
            <input v-model="form.linkman" placeholder="联系人">
          </div>
          <div>
            <img src="@/assets/img/safe/联系电话.png"/>
            <input v-model="form.phone" placeholder="联系电话">
          </div>
        </form>
        <button @click="submit">快速申请</button>
      </div>
    </div>

  </div>
  <!--<div class="content">-->
    <!--<div id="jrfw">-->
      <!--<p>金融服务</p>-->
      <!--<p>金融服务是新材料船舶产业的短板和痛点。本平台发挥规模集采、政策优势和资源聚合优势， 帮助客户解决收款难、贷款难和保额低等难题。</p>-->
    <!--</div>-->
    <!--<img  alt="" src="@/assets/img/safe/流程图.png" :style="{width:bWidth + 'px'}"/>-->
  <!--</div>-->
  <!--<img  alt="" src="@/assets/img/safe/服务介绍.png" :style="{width:bWidth + 'px'}"/>-->
  <!--<img  alt="" src="@/assets/img/safe/申请流程.png" :style="{width:bWidth + 'px'}"/>-->
  <!--<img  alt="" src="@/assets/img/safe/准备资料.png" :style="{width:bWidth + 'px'}"/>-->
  <div class="bg1" :style="{width:bWidth+'px'}">
    <h3>金融服务</h3>

    <div class="adertBox main" :style="{width:width+'px'}">
      <div class="adertNav point"
           v-for="(item,index) in adertList1"
           @mouseenter="changeLiTrans(index,1,1)"
           @mouseleave="changeLiTrans(index,0,1)"
           @click="toPage(item.locUrl)">
        <el-image
          v-show="index==0 && item.ifShow"
          style="width: 100%;height: 100%"
          :src="lc[0].imgUrl"
          fit="cover"></el-image>
        <el-image
          v-show="index!=0 || (index==0 && !item.ifShow)"
          style="width: 100%;height: 100%"
          :class="{toScale:item.ifShow}"
          :src="item.imgUrl"
          fit="cover"></el-image>
        <p><span>——</span> <span class="textS">{{item.nm}}</span> <span>——</span></p>
      </div>
    </div>
    <div class="Cooperation main" :style="{width:width + 'px'}">
      <h3>合作金融机构</h3>
      <ul>
        <li v-for="(item,m) in list1" :key="m"><img :src="item.imgUrl"/> </li>
      </ul>
    </div>


  </div>

  <div class="bg2" :style="{width:bWidth+'px'}">
    <h3>保险服务</h3>
    <div class="adertBox main" :style="{width:width+'px'}">
      <div class="adertNav point"
           v-for="(item,index) in adertList2"
           @mouseenter="changeLiTrans(index,1,2)"
           @mouseleave="changeLiTrans(index,0,2)"
           @click="toPage(item.locUrl)">
        <el-image
          style="width: 100%;height: 100%"
          :class="{toScale:item.ifShow}"
          :src="item.imgUrl"
          fit="cover"></el-image>
        <p><span>——</span> <span class="textS">{{item.nm}}</span> <span>——</span></p>
      </div>
    </div>
    <div class="Cooperation main" :style="{width:width + 'px'}">
      <h3>合作保险机构</h3>
      <ul>
        <li v-for="(item,m) in list2" :key="m"><img :src="item.imgUrl"/></li>
      </ul>
    </div>
  </div>
</div>
</template>

<script>
  import {mapState} from "vuex";
  export default {
    name: "safe",

    data(){
        return{
          lc:[],
          adertList1:[],
          adertList2:[],
          list1:[],
          list2:[],
          form:{
            company:'',
            amt:'',
            linkman:'',
            phone:''
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
    async asyncData(context){
      let arr = await Promise.all([
        context.app.api.getAdert('JRBX'),
        context.app.api.getAdert('financial'),
        context.app.api.getAdert('insurance')
      ])
      return {
        arr:arr
      }
    },
    mounted(){
      if(this.until.getQueryString('pos')){
        document.getElementById(this.until.getQueryString('pos')).scrollIntoView()
      }
      this.getData()
    },
    methods:{
        async getData(){
          let list = await this.api.getAdert('JRBX')
          this.lc = list.splice(0,1)
          console.log('hhh')
          console.log(this.lc)
          this.adertList1 = list.splice(0,4)
          this.adertList2 = list.splice(0,3)

           this.list1 = await this.api.getAdert('financial')
          this.list2 = await this.api.getAdert('insurance')
        },
      //鼠标滑动效果  索引，类型（1滑入，0滑出）
      changeLiTrans(index,type,nm){
        if(nm==1){
          this.adertList1[index].ifShow = type ? true : false
          this.$set(this.adertList1, index, this.adertList1[index])
        }
        if(nm==2){
          this.adertList2[index].ifShow = type ? true : false
          this.$set(this.adertList2,index,this.adertList2[index])
        }
      },
      toPage(url){
        if(url){
          let myUrl = url.split('^')
          window.location.href = myUrl[0]
        }
      },
      submit(){
        if(!this.form.company){
          this.$message({
            message: '请输入单位名称',
            type: 'warning'
          });
        }else if(!this.form.amt){
          this.$message({
            message: '请输入融资金额',
            type: 'warning'
          });
        }else if(!this.form.linkman){
          this.$message({
            message: '请输入联系人',
            type: 'warning'
          });
        }else if(this.reg.checkPhone(this.form.phone)!='ok'){
          this.$message({
            message: this.reg.checkPhone(this.form.phone),
            type: 'warning'
          });
        }else {
          console.log(this.form)
          this.store.commit('changeLoading',true)
          this.api.financeApplic(this.form).then(res=>{
            this.$message({
              message: '提交成功',
              type: 'success',
              duration:'1500'
            });
          })

        }

      },
    },
    }
</script>
<style lang="less">

</style>
<style lang="less" scoped>
  @import url("../../assets/css/init.less");
#home{
  background-color: #FFFFFF;
  .bg1{
    background-image: url("https://sinovat.oss-cn-shanghai.aliyuncs.com/181a1fdbcf3942b8ae65f07dd96d9abc-1.png");
    background-size: cover;
    background-repeat: no-repeat no-repeat;
    background-position: center;
  }
  .bg2{
    overflow: hidden;
    background-image: url("https://sinovat.oss-cn-shanghai.aliyuncs.com/910bfa66d38848d8b0096ebcaa06ddda-2.png");
    background-size: cover;
    background-repeat: no-repeat no-repeat;
    background-position: center;

  }
  h3{
    width: 100%;
    text-align: center;
    color: #333333;
    font-size: 20px;
    margin-top: 50px;
    margin-bottom: 30px;
  }
  .header{
    width: 100%;
    height: 450px;
    background-image: url("https://sinovat.oss-cn-shanghai.aliyuncs.com/9cbd676e1b4749e5a6c8a731eaa66101-%E9%87%91%E8%9E%8D%E4%BF%9D%E9%99%A9%E5%A4%B4%E9%83%A8.png");
    background-size: cover;
    background-repeat: no-repeat no-repeat;
    background-position: center;
    .main{
      position: relative;
      .apply{
        position: absolute;
        top: 54px;
        right: 100px;
        width: 307px;
        height: 352px;
        background: rgba(255,255,255,0.9);
        display: flex;
        flex-direction: column;
        align-items: center;
        >p{
          color: #333333;
          font-size: 18px;
          line-height: 18px;
          margin: 30px 0;
        }
        >form{
          display: flex;
          flex-direction: column;
          align-items: center;
          width: 100%;
          >div{
            position: relative;
            width: 100%;
            display: flex;
            justify-content: center;
            >img{
              height: 18px;
              position: absolute;
              left: 39px;
              top: 10px;
            }
            /deep/input{
              background: none;
              margin-bottom: 10px;
            }
            >input{
              height: 40px;
              padding-left: 57px;
              border-radius: 0;
              width: 80.4%;
              border: none;
              border-bottom: 1px solid rgba(0,0,0,0.1);
              >input::-webkit-input-placeholder{
                color: #929292;
              }
            }
          }
        }
        >button{
          border-radius: 5px;
          background-color: #2778BE;
          color: #FFFFFF;
          width: 80%;
          height: 42px;
          border: 0;
          cursor: pointer;
          margin-top: 10px;
        }
      }
    }

  }
  .point{
    cursor: pointer;
  }
  .adertBox{
    display: flex;
    justify-content: space-between;
    margin:10px auto;
    .adertNav{
      position: relative;
      overflow: hidden;
      /*display: inline-block;*/
      margin-right: 10px;
      >img{
        width: 100%;
        height: 100%;
        max-width: 200%;
        max-height: 200%;
      }

      >p{
        width: 100%;
        background-color: rgba(0,0,0,0.35);
        position: absolute;
        padding: 8px 0;
        bottom: 0;
        color:rgba(255,255,255,1);
        text-align: center;
        .textS{
          margin: 0 10px;
          opacity: 1;
          font-size:20px;
        }
        >span{
          opacity: 0.2;
        }
      }
    }
    .adertNav:last-child{
      margin-right: 0;
    }
    .cloumnBox{
      display: flex;
      flex-direction: column;
      justify-content: space-between;
    }
  }
  .content{
    width: 100%;
    position: relative;
    margin-top: 39px;
    margin-bottom: 58px;
    >div{
      position: absolute;
      width: 100%;
      align-items: center;
      display: flex;
      flex-wrap: wrap;
      flex-direction: column;
      p:nth-child(1){
        font-size: 32px;
        line-height: 70px;
        color: #333333;
      }
      p:nth-child(2){
        font-size: 15px;
        color: #666666;
        width: 41.9%;
        line-height: 25px;
        text-align: center;
      }
    }

  }
  .Cooperation{
    overflow: hidden;

    >ul{
      width: 100%;
      display: flex;
      flex-wrap: wrap;
      >li{
        width: 18%;
        border: 1px solid #D9D9D9;
        margin-right: 2.5%;
        margin-bottom: 30px;
        img{
          width: 100%;
          display: block;
        }
        &:nth-of-type(5n){
          margin-right: 0;
        }
      }
    }
  }
  .footerImg{
    width: 100%;
  }
}
</style>
