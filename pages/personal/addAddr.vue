<template>
<!--  新增地址-->
    <div class="main">
      <div style="padding:0 30px">
        <tobbar :title="title" :showAll="false"></tobbar>
      </div>
      <p style="border-bottom: 1px solid rgba(0,0,0,0.1);"></p>

      <div class="line4">
        <div class="item">
          <span>收货人:</span>
            <p class="right">
              <input placeholder="请输入收货人姓名" v-model="info.receNm" >
            </p>

        </div>
        <div class="item">
          <span>手机号码:</span>
          <p class="right">
            <input placeholder="请输入手机号码" v-model="info.receMob" type="tel">
          </p>
        </div>
        <div class="item">

          <span>所属地区:</span>
          <div class="right" style="display: flex">
            <addr @changeAddr="changeAddr" ref="addrChoose"></addr>

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
    import tobbar from "../../components/person/tobbar";
    import searchIc from '../../assets/img/personal/search.png'
    import deleteIc from '../../assets/img/personal/delete.png'
    import successIc from '../../assets/img/success.png'
    import addr from "../../components/addr";
    export default {
        name: "allShip",
        components:{
            tobbar,
            addr,
        },
        methods:{
            //取消
            cancel(){
              this.info={
                  receNm:'',
                  receMob:'',
                  addrNm:this.info.addrNm,
                  addrDtl:'',
                  hasDef:0,
                  addrCd: this.info.addrCd
              }
            },
            //保存地址
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

                if(this.type==1){
                    this.api.addrAdd(this.info).then(()=>{
                        this.$message({
                            message: '提交成功',
                            type: 'success'
                        });
                        this.$emit('save')
                        setTimeout(()=>{
                          this.$router.push('./addrList')
                        },1500)
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

            //获取地区信息
            changeAddr(info){
                let data = JSON.parse(info)
                this.info.addrNm = data.province+'-'+data.city+'-'+data.region
                this.info.addrCd = data.provinceId+'-'+data.cityId+'-'+data.regionId
            },
        },
        mounted(){
            this.type=this.$route.query.type
          if(this.$route.query.type==1){
              this.title='新增地址'
              this.$refs.addrChoose.getProvice()
          }else{
              this.title='修改地址'
              this.info=JSON.parse(this.$route.query.info)
              console.log(this.info.addrNm)
            console.log(this.$refs.addrChoose)
            this.$refs.addrChoose.getProvice(this.info.addrNm)
          }
        },
        data(){
            return{
                title:'新增地址',
                searchIc,
                deleteIc,
                successIc,
                info:{
                    receNm:'',
                    receMob:'',
                    addrNm:'',
                    addrDtl:'',
                    hasDef:0,
                },
                value: '',
                type:1,


            }
        },
        layout:'person',

    }
</script>

<style scoped lang="less">
  .main{
    width: 100%;


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
