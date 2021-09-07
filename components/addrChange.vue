<template>
  <div id="addrChange">
    <div class="line4">
      <div class="item">
        <span>收货人:</span>
        <p class="right">
          <input placeholder="请输入收货人姓名" v-model="info.receNm">
        </p>

      </div>
      <div class="item">
        <span>联系电话:</span>
        <p class="right">
          <input placeholder="请输入联系电话" v-model="info.receMob" type="tel">
        </p>
      </div>
      <div class="item">

        <span>所属地区:</span>
        <div class="right" style="display: flex">
          <addr @changeAddr="changeAddr" ref="addr"></addr>

        </div>
      </div>
      <div class="item">
        <span>详细地址:</span>
        <p class="right"> <input placeholder="请填写详细地址 到门牌号码" v-model="info.addrDtl"></p>
      </div>
      <div class="item" style="margin-top: 60px">
        <span></span>
        <p class="right">
          <span class="cancel" @click="cancel">取消</span>
          <span class="save" @click="save">保存</span>
        </p>
      </div>

    </div>
  </div>
</template>
<script>
  import {mapState} from "vuex";
  import addr from './addr'
  export default {
    props:['addrInfo'],
    data(){
      return{
        ifAdd:true,
        info:{
          receNm:'',
          receMob:'',
          addrNm:'',
          addrDtl:'',
          hasDef:0,
        },

      }
    },
    components: {
      addr
    },
    watch:{
      addrInfo(){
        this.ifAdd = this.addrInfo ? false : true
        if(this.addrInfo){
          this.info = JSON.parse(JSON.stringify(this.addrInfo))
          this.$refs.addr.getProvice(this.addrInfo.addrNm)
        }else {
          this.$refs.addr.getProvice()
        }
      }
    },
    computed: {
      ...mapState([
        'bWidth',
        'width'
      ])
    },
    mounted(){

    },
    methods:{
      //获取地区信息
      changeAddr(info){
        let data = JSON.parse(info)
        this.info.addrNm = data.province+'-'+data.city+'-'+data.region
      },
      save(){
        if(!this.info.receNm){
          this.$message({
            message: '请输入收货人姓名',
            type: 'warning'
          });
          return
        }
        if(this.reg.checkPhone(this.info.receMob)!='ok'){
          this.$message({
            message: this.reg.checkPhone(this.info.receMob),
            type: 'warning'
          });
          return
        }
        if(!this.info.addrDtl){
          this.$message({
            message: '请输入详情地址',
            type: 'warning'
          });
          return
        }
        this.store.commit('changeLoading',true)
        console.log(this.ifAdd)
        if(this.ifAdd){
          this.api.addrAdd(this.info).then(()=>{
            this.$message({
              message: '提交成功',
              type: 'success'
            });
            this.$emit('save')
          })
        }else {
          this.api.addrUpd(this.info).then(()=>{
            this.$message({
              message: '提交成功',
              type: 'success'
            });
            this.$emit('save')
          })
        }

      },
      cancel(){
        this.$emit('cancel')
      }
    }
  }
</script>
<style lang="less" scoped>
  @import url("../assets/css/init.less");
  #addrChange{
    .line4{
      width: 480px;
      margin: 20px auto;
      .item{
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

        .right{
          display: flex;
          justify-content: space-between;
          width: 80%;
          input{
            width: 100%;
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
          .cancel{

            flex: 1;

            border:1px solid rgba(221, 221, 221, 1);
            margin-right: 20px;
          }
          .save{

            flex:2;

            background:rgba(39,120,190,1);
            color: white;
          }

        }
      }

    }
  }
</style>

