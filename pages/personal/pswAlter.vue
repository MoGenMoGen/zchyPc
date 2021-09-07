<template>
<!--  修改密码-->
    <div class="main">
      <div style="padding:0 30px">
        <tobbar :title="title" :showAll="false"></tobbar>
      </div>
      <p style="border-bottom: 1px solid rgba(0,0,0,0.1);"></p>

      <div class="line4">
        <div><span>原始密码:</span>
            <p>
              <input placeholder="请输入原始密码" v-model="form.password" type="password">
            </p>

        </div>
        <div><span>新密码:</span>
          <p>
            <input placeholder="请输入新密码" v-model="form.newPassword" type="password">
          </p>

        </div>
        <div><span>确认密码:</span>
           <p> <input placeholder="请再次输入新密码" v-model="form.rePassword" type="password"></p>
        </div>
        <div style="margin-top: 60px">
            <span></span>
            <p >
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
    export default {
        name: "allShip",
        components:{
            tobbar,
        },
        methods:{
            save(){
                if(!this.form.password){
                    this.$message({
                        message: '请输入原密码',
                        type: 'warning'
                    });
                    return
                }
                if(!this.form.newPassword){
                    this.$message({
                        message: '请输入新密码',
                        type: 'warning'
                    });
                    return
                }
                if(!this.form.rePassword){
                    this.$message({
                        message: '请再次输入新密码',
                        type: 'warning'
                    });
                    return
                }

                if(this.form.rePassword!==this.form.newPassword){
                    this.$message({
                        message: '两次输入密码不同',
                        type: 'warning'
                    });
                    return
                }

                this.api.changePWD2(this.form).then(res=>{
                  this.$message.success("修改密码成功")
              })
            },
            cancel(){
              this.form={
                  password:'',
                  rePassword:'',
                  newPassword:'',
              }
            }
        },
        data(){
            return{
                title:'消息详情',
                searchIc,
                deleteIc,
                successIc,
                value: '',
                form:{
                    password:'',
                    rePassword:'',
                    newPassword:'',
                }


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
