<template>
    <!--我的地址-->
    <div class="main">
      <div style="padding:0 30px">
        <tobbar :title="title" :showAll="false"></tobbar>

      </div>
      <p style="border-bottom: 1px solid rgba(0,0,0,0.1);"></p>
      <div class="line2">
        <p @click="toAdd">新增地址</p>

      </div>
      <div class="line3">
        <p>
          收货人
        </p>
        <p>
          收货地址
        </p>
        <p>
          联系电话
        </p>
        <p>
          操作
        </p>
        <p>

        </p>

      </div>
      <div class="line4">
        <div v-for="item in addrList" :key="item.id">
          <p>{{item.receNm}}</p>
          <p>{{item.addrNm}}-{{item.addrDtl}}</p>
          <p>{{item.receMob}}</p>
          <p>

            <span @click="toAlter(item)">修改</span>
            <span>|</span>
            <span @click="toDelete(item)">删除</span>

          </p>

          <p>

            <span v-if="item.hasDef==1" class="sail"  >默认地址</span>
            <span class="detail" v-else @click="toDefault(item)">设为默认</span>
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
        mounted(){
            this.getAddrList()
        },
        methods:{
            toDelete(item){
              let t={
                  id:item.id
              }
              this.api.delAddr(t).then(res=>{
                  this.getAddrList()
              })

            },
            toDefault(item){
                this.api.addrUpdDef({id:item.id}).then(()=>{
                    this.getAddrList()
                })
            },
            //跳转到新增页面
            toAdd(){
                this.$router.push({
                    path:'/personal/addAddr',
                    query:{
                        type:1,
                    }
                })
            },
            //跳转到修改页面
            toAlter(item){
                this.$router.push({
                    path:'/personal/addAddr',
                    query:{
                        type:2,
                        info:JSON.stringify(item)
                    }
                })
            },
            //地址列表
            async getAddrList(){
                let qry = this.query.new()
                this.query.toP(qry,1,100)
                this.addrList = await this.api.addrList(this.query.toEncode(qry))
            },
        },
        data(){
            return{
                searchIc,
                deleteIc,
                successIc,
                title:'我的地址',
                addrList:[

                ]

            }
        },
        layout:'person',

    }
</script>

<style scoped lang="less">
  .main{
    width: 100%;
    .line2{
      width: 100%;
      height: 74px;
      position: relative;
      display: flex;
      align-items: center;
      justify-content: flex-end;
      p{
        width:106px;
        height:34px;
        background:rgba(39,120,190,1);
        border-radius:4px;
        color: white;
        display: flex;
        align-items: center;
        justify-content: center;
        font-size:16px;
        margin-right: 18px;
        &:hover{
          cursor: pointer;
        }
        &:active{
          opacity: 0.5;
        }
      }
    }
    .line3{
      display: flex;
      align-items: center;
      height: 60px;
      padding: 0 31px;
      background:#F7F7F7;
      font-size: 16px;
      p:nth-of-type(1){
        flex:1;
      }
      p:nth-of-type(2){
        flex:2;
      }
      p:nth-of-type(3){
        flex:1;
      }
      p:nth-of-type(4){
        flex:0.5;
        text-align: center;
      }

      p:nth-of-type(5){
        flex:1;
        text-align: center;
      }

    }
    .line4{

      div{
        display: flex;
        height: 76px;
        align-items: center;
        border-bottom: 1px solid #DFDFDF;
        padding: 0 31px;
        color: #333333;
        font-size: 15px;
        p:nth-of-type(1){
          flex:1;
        }
        p:nth-of-type(2){
          flex:2;
        }
        p:nth-of-type(3){
          flex:1;
        }
        p:nth-of-type(4){
          flex:0.5;
          display: flex;
          justify-content: center;
          align-items: center;
          span:nth-of-type(2){
            margin: 8px;
            opacity: 0.2;
            font-size: 14px;
          }
          span:nth-of-type(1),span:nth-of-type(3){
            white-space: nowrap;
            &:hover{
              cursor: pointer;
            }
          }
        }
        p:nth-of-type(5){

          flex:1;
          display: flex;
          justify-content: center;
          align-items: center;

          span:hover{
            cursor: pointer;

          }
          span:active{
            opacity: 0.5;
          }
          /*默认地址*/
          .detail{
            margin: 0 auto;
            display: block;
            color: #333333;

          }
          /*设为默认*/
          .sail{
            width:106px;
            height:34px;
            border:1px solid  #2778BE;
            border-radius:4px;
            display: flex;
            color: #2778BE;
            align-items: center;
            justify-content: center;

          }

        }
      }


    }
  }




</style>
