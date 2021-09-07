<template>
  <div class="bg" v-show="show">
    <div class="container">
      <div class="header">
          {{ title }}
      </div>
      <div class="main">
        <div class="main1" v-bind="currentRole">
          <span>当前账号：</span><p>{{currentRole ? currentRole.company : ''}}<span>[{{currentRole ? currentRole.identityNm : ''}}]</span><img src="@/assets/img/personal/首页/选中.png" alt=""/></p>
        </div>
        <div class="main2">
          <div class="partA">
            <span>切换账号：</span>
          </div>
          <div class="partB">
            <p v-if="currentRole.id != item.id" v-for="(item,index) in list" :key="index">{{item.company}}<span>[{{item.identityNm}}]</span><img @click="changeSt(index)" v-if="item.isActive" src="@/assets/img/personal/首页/选中.png" alt=""/><img @click="changeSt(index)" v-if=" ! item.isActive" src="@/assets/img/personal/首页/未选中.png" alt=""/></p>
          </div>
        </div>
      </div>
      <div class="footer">
        <el-button round @click="hideModal">取消</el-button>
        <el-button type="primary" round @click="submit">提交</el-button>
      </div>
    </div>
  </div>
</template>

<script>
    import {mapState} from "vuex";

    export default {
        name: "role",
      data(){
          return{
            choose:'',
            list:[],
          }
      },
      props: {
        show: {
          type: Boolean,
          default: false
        },
        title: {
          type: String,
          default: ''
        },
      },
      computed:{
        ...mapState([
          'bWidth',
          'width',
          "currentRole"
        ])
      },
      mounted(){
          this.getData()
      },
      methods: {
          async getData(){
            this.list = await this.api.roleList()
          },
          hideModal() {
            this.$emit('hideModal')
          },

          submit() {
            this.$emit('submit')
            console.log(this.choose)
            this.store.dispatch('save',this.choose)
            // this.store.commit("currentRole",this.choose)

          },
          changeSt(index){
            this.list.forEach(item =>{
              if(item.isActive === true){
                item.isActive = false
              }
            });
            this.list[index].isActive = true
            this.$set(this.list,index,this.list[index])
            this.choose = this.list[index]
            console.log('1111111',this.choose)
          },
      }
    }
</script>

<style lang="less" scoped>
  @import url("../../assets/css/init.less");
  .bg {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: rgba(0,0,0,.5);
    z-index: 10;
    display: flex;
    display: -webkit-flex;
  }
  .container {
    background: #fff;
    border-radius: 8px;
    /*flex: 1;*/
    overflow-y: scroll;
    /*position: fixed;*/
    margin: auto;
    height: 80%;
    padding-bottom: 20px;
    width: 571px;
  }
  .header {
    height: 78px;
    background-color: #FFFFFF;
    font-size: 18px;
    color: #333333;
    display: flex;
    align-items: center;
    justify-content: center;
    border-bottom: 1px solid #DFDFDF;
  }
  .footer {
    display: flex;
    align-items: center;
    justify-content: center;
    height: 130px;

    button:nth-child(1){
      width: 149px;
      height: 52px;
      margin-left: 87px;
      border-radius: 0;
      background-color: #FFFFFF;
      color: #666666;
      font-size: 16px;
      cursor: pointer;
    }
    button:nth-child(2){
      width: 228px;
      height: 52px;
      margin: 0 87px 0 20px;
      border-radius: 0;
      background-color: #2778BE;
      color: #FFFFFF;
      font-size: 16px;
      cursor: pointer;
    }
  }

  .main{
    padding: 15px 56px;
    .main1{
      display: inline-flex;
      border-bottom: 1px solid #DFDFDF;
      padding-bottom: 15px;
      color: #666666;
      width: 100%;
      flex: 1;
      padding-top: 15px;
      img{
        width: 24px;
        vertical-align: text-top;
        float: right;
        cursor: pointer;
      }
      span{
        font-size: 16px;
      }
      p{
        font-size: 16px;
        flex: 1;
        >span{
          margin-left: 5px;
        }
      }
    }
    .main2{
      color: #666666;
      font-size: 16px;
      display: inline-flex;
      flex-wrap: wrap;
      width: 100%;
      line-height: 40px;
      padding-top: 15px;
      flex: 1;
      .partA{
      }
      .partB{
        flex: 1;
        display: flex;
        flex-direction: column;
        img{
          width: 24px;
          vertical-align: text-top;
          float: right;
          cursor: pointer;
          padding-top: 10px;
        }
        p{
          >span{
            margin-left: 5px;
          }
        }
      }
    }
  }
</style>
