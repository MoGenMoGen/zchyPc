<template>
  <div class="specs">
    <div v-for="(item,index) in specs" :index="index" class="item">
      <p>{{item.nm}}</p>：
      <ul>
        <li v-for="(v,i) in item.vals" :key="i" :class="{active:currentSpecArr[index].split(':')[1]==v.id}" @click="toChangeSpecs(index,i)">{{v.nm}}</li>
      </ul>
    </div>
    <div class="btn">
       <button @click="confirm">确定</button>
       <button @click="cancel">取消</button>
    </div>
  </div>
</template>
<script>

  export default {
    props:['info'],
    data(){
      return{
        currentSpecArr:[],
        currentSpecs:'',
        proInfo:{},
        specs:[],
        currentInfo:{},
      }
    },
    components: {
    },
    watch:{
      info:{
        handler:function () {
          if(this.info && this.info.goodsId){
             this.getSku()
          }
        },
        immediate:true
      },
      currentSpecArr:{
        handler:function () {
          this.currentSpecs = this.currentSpecArr.join(',')
          this.currentInfo = this.proInfo.skus.find(item=>item.skuAttr==this.currentSpecs)

        },
        deep:true
      },
    },
    computed: {

    },
    mounted(){

    },
    methods:{
      //更改属性
      toChangeSpecs(index,i){
        this.currentSpecArr[index] = this.specs[index].id+':'+this.specs[index].vals[i].id
        this.$set(this.currentSpecArr,index,this.currentSpecArr[index])
      },
      confirm(){
        let data = {
          id:this.info.cartId,
          goodsSkuId:this.currentInfo.skuId
        }
        this.$emit('confirm',data)
      },
      cancel(){
        this.$emit('cancel')
      },
      //获取详情
      async getSku(){
        console.log(this.info)
        this.proInfo = await this.api.productDetail(this.info.goodsId)
        this.specs = this.proInfo.attrs
        this.currentSpecArr = this.info.skuAttr.split(',')

      }
    }
  }
</script>

<style lang="less" scoped>
  @import url("../assets/css/init.less");
  .specs{
    width: 100%;
    padding-top: 20px;
    .item{
      display: flex;
      display: -webkit-flex;
      padding-top: 10px;
      line-height: 31px;
      padding-left: 40px;
      p{
        color: #999999;
        width: 70px;
        text-align: justify;
        text-align-last: justify;
      }
      ul{
        flex: 1;
        padding-left: 5px;
        li{
          display: inline-block;
          height: 31px;
          border: 1px solid #CCCCCC;
          margin-right: 20px;
          margin-bottom: 20px;
          padding: 0 10px;
          cursor: pointer;
        }
        .active,li:hover{
          border: 1px solid @redColor;
        }
      }
    }
    .btn{
      padding: 40px;
      overflow: hidden;
      button{
        width: 76px;
        height: 32px;
        line-height: 32px;
        border-radius: 4px;
        margin-right: 20px;
        font-size: 16px;
        text-align: center;
        border: none;
        float: left;
        cursor: pointer;
        &:first-of-type{
          background: @themeColor;
          color: #ffffff;
        }
        &:last-of-type{
          border: 1px solid #D1D1D1;
          box-sizing: border-box;
          color: #666666;
          background: #ffffff;
        }
      }
    }
  }
</style>

