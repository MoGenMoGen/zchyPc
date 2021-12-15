<template>
<!--  个人资料-->
    <div class="main">
      <div style="padding:0 30px">
        <tobbar :title="title" :showAll="false"></tobbar>
      </div>
      <p style="border-bottom: 1px solid rgba(0,0,0,0.1);"></p>


      <div class="line2">

        <div>
          <p v-for="(item,index) in tabList" :key="item.id"  :class="{active2: tabId==item.id}" @click="chooseTab(item,index)">
            <span>{{item.nm}}</span>
          </p>
<!--          30*(tabIndex+1)加上一个间距-->
          <span class="btm" :style="{left:tabIndex*64+30*(tabIndex+1)+'px'}"></span>
        </div>
      </div>


      <div class="line4" v-if="tabId==1">
        <div><span>头像:</span>
            <p style="justify-content: flex-start;align-items: center">
              <img :src="info.imgUrl" v-if="info.imgUrl">
              <span style="width: 100px;height: 100px;border: 1px solid #F0F0F0;" v-else></span>
              <span  class="dianji"><input type="file" @change="upImg($event)"/>点击上传头像</span>
            </p>

        </div>

        <div>
          <span>姓名:</span>
           <p> <input placeholder="请输入姓名" v-model="info.realNm"></p>
        </div>

        <div>
          <span>性别:</span>
          <p >
            <el-radio v-model="info.gender" label="男"></el-radio>
            <el-radio v-model="info.gender" label="女"></el-radio>

          </p>
        </div>
        <div><span>联系电话:</span>
          <p style="color: #333333">{{info.mob}}</p>
        </div>
        <div style="margin-top: 60px">
            <span></span>
            <p >
              <span class="cancel" >取消</span>
              <span class="save" @click="toSave">保存</span>
            </p>
        </div>

      </div>
      <!--企业信息，审核状态-->
      <!--audit: "审核状态  [r]（1：待审核，2：审核通过，3：审核不通过）"-->
      <div v-if="tabId==2 && showShenhe" class="line6">
        <p v-if="roleInfo.audit==0 || roleInfo.audit==1"><img :src="shenhezhong">审核中</p>
        <p v-if="roleInfo.audit==2"><img :src="tongguo">审核通过</p>
        <p v-if="roleInfo.audit==3"><img :src="weitongguo">审核未通过</p>
        <p v-if="roleInfo.audit==3">未通过理由：{{roleInfo.options}}</p>
        <img class="delteIc" :src="deleteIc" @click="showShenhe=false">
      </div>
      <div class="line5" v-if="tabId==2">
        <p class="title">
          <span ></span>
          <span >基本信息</span>
        </p>
        <div><span>公司名称:</span>
          <p>{{roleInfo.company}}</p>
        </div>
        <div><span>公司地址:</span>
          <p>{{roleInfo.addrNm}}{{roleInfo.address}}</p>
        </div>
        <div>
          <span>联系人:</span>
          <p>{{roleInfo.linkMan}}<span @click="companyChange('linkMan')">修改</span></p>

        </div>
        <div>
          <span>联系手机:</span>
          <p>{{roleInfo.mob}} <span  @click="companyChange('mob')">修改</span></p>
        </div>
        <div>
          <span>电子邮箱:</span>
          <p>{{roleInfo.email}}<span  @click="companyChange('email')">修改</span></p>
        </div>
<!--企业资料-->
        <p class="title" style="margin-top: 60px">
          <span ></span>
          <span >企业资料</span>
        </p>

        <div style="align-items: flex-start">
          <span style="margin-right: 33px">企业营业执照：</span>
          <p> <img :src="roleInfo.businessLicense" style="width: 233px;"></p>
        </div>
        <div style="align-items: flex-start">
          <span style="margin-right: 33px;flex-shrink: 0">法人身份证：</span>
          <p style="display: flex"> <img :src="roleInfo.idCard" style="width: 177px;"> <img :src="roleInfo.idCardReverse" style="width: 177px;margin-left: 37px"></p>
        </div>
        <div style="align-items: flex-start" v-if="roleInfo.qualification">
          <span style="margin-right: 33px">企业资质证明：</span>
          <p> <img :src="item" v-for="(item,index) in roleInfo.qualification.split(',')" style="width: 233px;height: 162px; margin-right: 8px"></p>
        </div>
        <div style="align-items: flex-start;">
          <span style="margin: 10px 33px 0 0;flex-shrink: 0;">企业简介：</span>
        </div>
        <div>
          <div v-html="roleInfo.intro" style="overflow-x: auto"></div>

        </div>
      </div>

    </div>
</template>

<script>
    import tobbar from "../../components/person/tobbar";
    import searchIc from '../../assets/img/personal/search.png'
    import deleteIc from '../../assets/img/personal/delete.png'
    import successIc from '../../assets/img/success.png'
    import tes1 from '../../assets/img/personal/test1.png'
    import myRadio from "../../components/person/myRadio/myRadio";

    import checked from "../../assets/img/personal/checked.png"
    import shenhezhong from "../../assets/img/personal/审核中.png"
    import weitongguo from "../../assets/img/personal/未通过.png"
    import tongguo from "../../assets/img/personal/通过.png"
    import {mapState} from "vuex";
    export default {
        name: "allShip",
        components:{
            tobbar,
            myRadio,
        },

        data(){
            return{
                title:'个人资料',
                tongguo,
                searchIc,
                deleteIc,
                successIc,
                tes1,
                checked,
                shenhezhong,
                weitongguo,
                showShenhe:true,//显示审核
                value: '',
                tabId:1,
                tabIndex:0,
                info:{}, //个人信息
                roleInfo:{},//机构信息
                sex:'男',
                tabList:[
                    {
                        id:1,
                        nm:'个人信息',
                    },
                    {
                        id:2,
                        nm:'企业信息',
                    },

                ],


            }
        },
        layout:'person',
        async mounted(){
          this.info = await this.api.personInfo() //个人信息
        },
      computed: {
        ...mapState([
          'currentRole'
        ])
      },
      watch:{
        currentRole:{
          handler:function (e) {
            if(e){
              this.getInfo()

            }

          },
          immediate:true
        }
      },
      methods:{
          async getInfo(){ //入驻信息
            this.roleInfo = await this.api.getRoleDetail(this.currentRole.id)
          },
          //保存信息
        toSave(){
          let param = {
            realNm: this.info.realNm,
            gender: this.info.gender,
            imgUrl: this.info.imgUrl,
          }
          this.api.personInfoChange(param).then(()=>{
            this.$message({
              message: '保存成功！',
              type: 'success'
            });
          })
        },
        companyChange(type){
          if(type=='email'){
            this.$prompt('请输入邮箱', '提示', {
              confirmButtonText: '确定',
              cancelButtonText: '取消',
              inputPattern: /[\w!#$%&'*+/=?^_`{|}~-]+(?:\.[\w!#$%&'*+/=?^_`{|}~-]+)*@(?:[\w](?:[\w-]*[\w])?\.)+[\w](?:[\w-]*[\w])?/,
              inputErrorMessage: '邮箱格式不正确'
            }).then(({ value }) => {
              let param  = this.roleInfo
              param.email = value
              this.companySave(param)
            })
          }
          if(type=='linkMan'){
            this.$prompt('请输入联系人', '提示', {
              confirmButtonText: '确定',
              cancelButtonText: '取消',
            }).then(({ value }) => {
              let param  = this.roleInfo
              param.linkMan = value
              this.companySave(param)
            })
          }
          if(type=='mob'){
            this.$prompt('请输入联系手机', '提示', {
              confirmButtonText: '确定',
              cancelButtonText: '取消',
              inputPattern: /^(((13[0-9]{1})|(14[0-9]{1})||(15[0-9]{1})|17[0-9]{1}|(18[0-9]{1}))+\d{8})$/,
              inputErrorMessage: '手机号码格式不正确'
            }).then(({ value }) => {
              let param  = this.roleInfo
              param.mob = value
              this.companySave(param)
            })
          }
        },
        companySave(param){
          this.api.settleInUpd(param).then(()=>{
            this.getInfo()
          })
        },
        //图片上传
       async upImg(e){
         if(e.target.files.length===0){
           return
         }
          this.store.commit('changeLoading',true)
          this.info.imgUrl = await this.api.uploadImg(e)
        },
        chooseTab(item,index){
          if(item.id!=this.tabId){
            this.tabId=item.id
            this.tabIndex=index
          }
        }
      },
    }
</script>

<style scoped lang="less">
  .main{
    width: 100%;

    .line2{

      margin-top: 15px;
      width: 100%;

      div{
        /*padding: 0 30px;*/
        border-bottom:1px solid rgba(0,0,0,0.1);
        display: flex;
        height: 42px;
        align-items: center;
        position: relative;
        p{
          margin-left: 30px;
          width: 64px;
          display: flex;
          align-items: center;
          justify-content: center;
          height: 100%;
          /*对所有属性过渡0.2s*/
          &:hover{
            cursor: pointer;
          }
        }
        .btm{
          position: absolute;
          width: 64px;
          height: 100%;
          border-bottom: 2px solid #2778BE;
          /*对left属性过渡5秒*/
          transition: left 0.5s;
        }
      }


    }


    .line4{
      width: 480px;
      margin: 20px auto;

      div{
        margin-top: 30px;
        width: 100%;
        display: flex;
        align-items: center;
        span{
          width: 20%;
          white-space: nowrap;
          color: #666666;
          font-size: 15px;
        }
        p{
          display: flex;
          /*justify-content: space-between;*/
          width: 80%;
          input{
            width: 398px;
            height: 42px;
            outline: none;
            border:1px solid rgba(221, 221, 221, 1);
            padding: 0 21px;
          }
          ::-webkit-input-placeholder { /* Chrome/Opera/Safari */
            color: #999999;
          }
          ::-moz-placeholder { /* Firefox 19+ */
            color: #999999;
          }
          :-ms-input-placeholder { /* IE 10+ */
            color: #999999;
          }
          :-moz-placeholder { /* Firefox 18- */
            color: #999999;
          }
          span{
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 16px;
            height:52px;
            &:hover{
              cursor: pointer;
            }
            /*按钮按下效果*/
            &:active{
              opacity: 0.5;
            }
          }
          /*头像*/
          img{
            width: 100px;
            height: 100px;
            border-radius: 50%;
          }

          .dianji{
            color: #2778BE;
            border-bottom: 1px solid #2778BE;
            margin-left: 21px;
            height: 14px;
            width: fit-content;
            font-size: 14px;
            position: relative;
            cursor: pointer;
            input{
              position: absolute;
              width: 100%;
              height: 100%;
              opacity: 0;
              cursor: pointer;
            }
          }

          .cancel{

            flex: 1;

            border:1px solid rgba(221, 221, 221, 1);
            margin-right: 10px;
          }
          .save{
            margin-left: 10px;
            flex:1;

            background:rgba(39,120,190,1);
            color: white;
          }

        }
      }

    }

    .line6{
      width: 100%;
      position: relative;
      border-bottom: 1px solid rgba(0,0,0,0.1);
      padding-top: 51px;
      padding-bottom: 51px;
      p:nth-of-type(1){
        display: flex;
        align-items: center;
        justify-content: center;
        color: #333333;
        font-size: 20px;
        img{
          width: 34px;
          margin-right: 9px;
        }
      }
      p:nth-of-type(2){
        text-align: center;
        color: #666666;
        margin-top: 15px;
      }
      .delteIc{
        position: absolute;
        top:20px;
        right: 25px;
        width: 25px;
        &:hover{
          cursor: pointer;
        }

      }


    }

    .line5{
      width: 640px;
      margin: 20px auto;
      .title{

        display: flex;
        align-items: center;
        width: 100%;
        span:nth-of-type(1){
          width: 4px;
          background: #2778BE;
          height: 17px;
          margin-right:7px;
        }
        span:nth-of-type(2){
          font-size:20px;


          font-weight:bold;
          color:rgba(51,51,51,1);
          line-height:24px;
        }
      }

      >div{
        margin-top: 30px;
        width: 100%;
        display: flex;
        align-items: center;
        font-size: 15px;
        >span{
          width: 20%;
          white-space: nowrap;
          color: #666666;

        }
        >p{
          span{
            margin-left: 22px;
            color: #2778BE;
            &:hover{
              cursor: pointer;
            }
          }
        }
      }

    }



  }
  .active2{
    color: #2572B5;
    /*border-bottom: 2px solid #2572B5;*/
  }



</style>
