<template>
  <!--投标报价弹窗-->
  <div id="offer" v-show="show">
    <div class="body">
      <div class="title">
        <p>接单</p>
        <img @click="toClose" src="@/assets/img/close.png"/>
      </div>
       <div class="foot">
         <p><span>服务单号：</span><span>{{info ? info.cd : ''}}</span></p>
         <span style="color: #ff0000">*</span> <span>服务人员：</span>
         <el-input v-model="handleUserNm" class="input" placeholder="请输入接单人员"></el-input>

         <!--<el-select v-model="id" placeholder="请选择派单人员">-->
           <!--<el-option-->
             <!--v-for="item in manList"-->
             <!--:key="item.memId"-->
             <!--:label="item.username"-->
             <!--:value="item.memId">-->
           <!--</el-option>-->
         <!--</el-select>-->
        </div>
        <div class="button">
          <button @click="toClose">取消</button>
          <button @click="submit">确认</button>
        </div>
      </div>
    </div>
</template>

<script>
  import {mapState} from "vuex";
  import delImg from '@/assets/img/personal/del.png'
  import excel from '@/assets/img/personal/excel.png'
  import ppt from '@/assets/img/personal/ppt.png'
  import word from '@/assets/img/personal/word.png'
  import pdf from '@/assets/img/personal/pdf.jpg'
  import del from '@/assets/img/personal/del.png'

  export default {
    name: "dispatch",
    data(){
      return{
        delImg,
        excel,
        ppt,
        word,
        pdf,
        del,
        id:'',
        handleUserNm:'',
      }
    },
    props: {
      show: {
        type: Boolean,
        default: false
      },
      info:{
        type:Object,
        default:()=>{}
      },
      manList:{
        type:Array,
        default:()=>[]
      }
    },
    computed:{
      ...mapState([
        'bWidth',
        'width',
        "currentRole"
      ])
    },
    watch:{

    },
    created(){


    },
    mounted(){


    },
    methods:{
      toClose() {
        this.$emit('toClose')
      },
      submit(){
        if(!this.handleUserNm){
          this.$message({
            message: '请输入接单人员',
            type: 'warning'
          });
          return
        }
        this.store.commit('changeLoading',true)
        let param = {
          taskId:this.info.id,
          handleRealNm:this.handleUserNm,
        }
        this.api.toDispatch(param).then(()=>{
          this.$message({
            message: '派单成功',
            type: 'success'
          });
          this.$emit('confirm')
        })
      }

    },
  }
</script>

<style lang="less">

</style>
<style lang="less" scoped>
  @import url("../../assets/css/init.less");
  #offer{
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: rgba(0,0,0,.5);
    z-index: 10;
    .input{
      display: inline-block;
      width: 60%;
    }
    .body{
      background: #fff;
      border-radius: 8px;
      position: fixed;
      top: 50%;
      left: 50%;
      transform: translate(-50%,-50%);
      max-height: 597px;
      padding-bottom: 20px;
      width: 440px;
      .title{
        height: 77px;
        display: flex;
        align-items: center;
        justify-content: center;
        position: relative;
        border-bottom: 1px solid #DFDFDF;
        >p{
          color: #333333;
          font-size: 18px;
        }
        >img{
          width: 20px;
          position: absolute;
          top: 19px;
          right: 21px;
          cursor: pointer;
        }
      }
    }
    .foot{
      padding: 0 35px;
      >p{
        line-height: 48px;
        height: 48px;
      }
    }
    .button{
      display: flex;
      padding: 0 27px;
      justify-content: space-between;
      margin-top: 30px;
      button:nth-child(1){
        width: 138px;
        height: 52px;
        background-color: #FFFFFF;
        border: 1px solid #DDDDDD;
        font-size: 16px;
        color: #666;
        cursor: pointer;
      }
      button:nth-child(2){
        width: 216px;
        height: 52px;
        background-color: #2778BE;
        font-size: 16px;
        border: 0;
        color: #FFF;
        cursor: pointer;
      }
    }
  }
  .fileList{
    width: 100%;
    display: flex;
    flex-direction: column;
    padding-top: 30px;
    padding-left: 30px;
    >p{
      color: #333333;
      height: 25px;
      line-height: 25px;
      font-size: 15px;
      >img{
        width: 16px;
        height: 18px;
        vertical-align: sub;
        margin-right: 10px;
      }
    }
  }
</style>
