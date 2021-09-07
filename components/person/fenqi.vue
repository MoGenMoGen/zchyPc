<template>
  <div class="afterSale">
    <div class="container">
      <div class="header">
          分期付款
        <img :src="closeImg" @click="close"/>
      </div>
      <el-scrollbar class="main">
        <div class="info" v-if="info">
          <p>订单编号：{{info.orderCd}}</p>
          <p>收款名称：{{stageInfo.shopNm}}</p>
          <p>订单总额：￥{{stageInfo.orderPrice}}</p>
          <p>已付金额：<span class="red">￥{{stageInfo.payAmt ? stageInfo.payAmt : '0'}}</span></p>
        </div>
        <div class="proList">
          <p>阶段收款：</p>
          <ul>
            <li v-for="item in stageInfo.itms" :key="item.id">
              <p>
                {{item.nm}}<br/>
                到期时间： {{item.expireTm.slice(0,10)}}<br/>
                付款金额：<span class="red">￥{{item.amount}}</span>
              </p>
              <div class="img" v-if="!item.status">
                <div class="uploadImg">
                  <div>
                    <p>+</p>
                    <input :ref="'upload'+item.id" type="file" name="file"   multiple="multiple" @change="upImg($event,item)"/>
                  </div>
                </div>
                <div v-show="item.payVoucher" class="payVoucher">
                  <img :src="delImg" class="delImg" @click="deleteImg(item)"/>
                  <img :src="item.payVoucher" @click="toLink(item.payVoucher)"/>
                </div>
              </div>
              <img v-else :src="item.payVoucher"/>
              <p v-if="item.status==1"><img :src="check"/>付款审核中</p>
              <p v-if="item.status==2"><img :src="paySuccss"/>已确认付款</p>
              <p v-if="!item.status"><span @click="pay(item)">确认付款</span></p>
            </li>
          </ul>
        </div>

      </el-scrollbar>

    </div>
  </div>
</template>

<script>
    import closeImg from '@/assets/img/close.png'
    import delImg from '@/assets/img/personal/del.png'
    import paySuccss from '@/assets/img/order/成功.png'
    import check from '@/assets/img/order/审核中.png'
    export default {
        name: "role",
      data(){
          return{
            choose:'',
            closeImg,
            delImg,
            paySuccss,
            check,
            list:[],
            imgList:[],
            stageInfo:{},
          }
      },
      props: {
        info: {
          type: Object,
          default: null
        },
      },
     watch:{
       // info(){
       //   console.log('-=============')
       //   if(this.info){
       //      this.list = this.info.itms
       //     console.log('==================分期')
       //     this.getInfo()
       //   }
       // }
     },
      mounted(){
        this.list = this.info.itms
        console.log('==================分期')
        this.getInfo()
      },
      methods: {
          close(){
            this.$emit('close')
          },
        //分期信息
        async getInfo(){
          this.stageInfo =  await this.api.stageInfo(this.info.id)
        },
        async upImg(e,item){
          if(e.target.files.length===0){
            return
          }
            console.log('=================')
          let img = await this.api.uploadImg(e)
          item.payVoucher = img
        },
        //删除图片
        deleteImg(item){
          item.payVoucher = ''
          console.log(this.$refs['upload'+item.id])
          this.$refs['upload'+item.id][0].value = ''
        },
        toLink(url){
          window.open(url)
        },
        pay(item){
            if(!item.payVoucher){
              this.$message({
                message: '请上传支付凭证',
                type: 'warning'
              });
              return
            }
            let param = {
              id:item.id,
              voucher:item.payVoucher
            }
            this.api.stageConfirm(param).then(()=>{
              this.$message({
                message: '支付成功',
                type: 'success'
              });
              this.getInfo()
            })
        }

      }
    }
</script>

<style lang="less" scoped>
  @import url("../../assets/css/init.less");
  .afterSale {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: rgba(0,0,0,.23);
    z-index: 10;
    display: flex;
    display: -webkit-flex;
  }
  .container {
    background: #fff;
    border-radius: 8px;
    margin: auto;
    max-height: 80%;
    padding-bottom: 20px;
    width: 651px;
    display: flex;
    display: -webkit-flex;
    flex-direction: column;
    .info{
      border-bottom: 1px solid #DFDFDF;
      width: 100%;
      padding: 20px 30px;
      box-sizing: border-box;
      line-height: 40px;
    }
  }
  .header {
    height: 78px;
    font-size: 18px;
    color: #333333;
    width: 100%;
    line-height: 78px;
    text-align: center;
    border-bottom: 1px solid #DFDFDF;
    position: relative;
    img{
      position: absolute;
      width: 20px;
      right: 20px;
      top:20px;
      cursor: pointer;
    }
  }


  .main{
    flex: 1;
    overflow-y: auto;
    width: 100%;
    height: 100%;
    box-sizing: border-box;
    .img{
      //overflow: hidden;
      width: 85px;
      cursor: pointer;
      margin-right: 71px;
      height: 85px;
      position: relative;
      .payVoucher{
        position: absolute;
        left: 0;
        top:0;
        z-index: 88;
      }
      >div{
        width:100%;
        position: relative;
        height: 85px;
        img{
          width: 100%;
          height: 100%;
        }
        .delImg{
          position: absolute;
          top: -7.5px;
          right: -7.5px;
          z-index: 2;
          width: 15px;
          height: 15px;
        }
      }
      .uploadImg{
        display: flex;
        display: -webkit-flex;
        align-items: center;
        // width: 85px;
        >p{
          color: #333333;
          font-size: 15px;
          float: left;
          line-height: 85px;
          margin-left: 3%;
        }
        >div{
          display: flex;
          display: -webkit-flex;
          color: #C6C6C6;
          // width: 85px;
          width: 100%;
          // height: 85px;
          height: 100%;
          p{
            // width: 85px;
            width: 100%;
            text-align: center;
            // height: 85px;
            // line-height: 70px;
            border: 1px dashed #C6C6C6;
            font-size: 50px;
            line-height: 85px;
          }
          input{
            width: 100%;
            height: 100%;
            opacity: 0;
            position: absolute;
            left: 0;
            top: 0;
          }
        }
      }
    }
    .img::after{
      content: "";
      display: block;
      clear: both;
    }
    .proList{
      padding: 0 30px;
      >p{
        width: 100%;
        padding-right: 12px;
        box-sizing: border-box;
        padding-top: 10px;
        padding-bottom: 20px;
      }
      ul{
        li{
          display: flex;
          display: -webkit-flex;
          align-items: center;
          margin-bottom: 20px;
          color: #333333;
          background: #EFEFEF;
          padding: 19px;
          >img{
            width: 85px;
            margin-right: 71px;
          }
          >p{
            &:first-of-type{
              flex: 1;
              line-height: 36px;
            }
            &:last-of-type{
              width: 100px;
              display: flex;
              display: -webkit-flex;
              align-items: center;
              img{
                width: 18px;
                margin-right: 10px;
              }
              span{
                display: inline-block;
                width: 89px;
                height: 33px;
                line-height: 33px;
                text-align: center;
                background: @themeColor;
                color: #ffffff;
                border-radius: 3px;
                cursor: pointer;
              }
            }
          }
        }
      }
    }
  }
</style>
