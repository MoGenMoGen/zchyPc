<template>
  <div class="out">
    <p><span></span>{{title}}</p>
    <p><span @click="toBack"> < 返回</span><span @click="toAll" v-if="showAll"> 全部船舶</span></p>

  </div>
</template>

<script>
    export default {
        name: "tobbar",
        props:{
            showAll:{
                default:true
            },
            title:{
                default: '船舶档案'
            }
        },
        data(){
            return{

            }
        },
        methods:{
            toBack(){
              this.$router.back()
            },
            toAll(){

            },
        }
    }
</script>

<style scoped lang="less">
  .out{
    width: 100%;
    display: flex;
    height: 78px;
    align-items: center;
    justify-content: space-between;
    p:nth-of-type(1){
      display: flex;
      align-items: center;

      color: #333333;
      font-size: 20px;
      span{
        border-left: 3px solid #2778BE;
        height: 18px;
        margin-right: 10px;
      }
    }
    p:nth-of-type(2){
      color: #666666;
      span:hover{
        cursor: pointer;
      }
      span:nth-of-type(2){
        margin-left: 20px;


      }
    }
  }
</style>
