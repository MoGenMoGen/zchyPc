<template>
  <div class="pageMain">
    <div class="pageBtn">
      <p class="pageTo" :class="{noToPage:pageNo=='1'||pageSum<=1}" @click="pageReduce">< 上一页</p>
      <p v-for="item in pageArr" :class="{pageThis:pageNo==item,seeMore:item=='. . .'}" @click="pageChange(item)">{{item}}</p>
      <p class="pageTo" style="margin-left: 15px;" :class="{noToPage:pageNo==pageSum||pageSum<=1}" @click="pageAdd">下一页 ></p>
    </div>
    <div class="pageGo">
      <p >共{{pageSum}}页，到第<input type="number" v-model="goPage"  min="1" :max='pageSum'> 页</p>
    </div>
    <button @click="toPageNo">跳转</button>
  </div>
</template>

<script>
    export default {
        name: "page-ination",
        //total:总数  pageSize:每页显示的数量
        props:['total','pageSize'],
        data(){
            return{
              pageSum:0,   //总页码
              pageArr:[],  //页码数组,在页码过度时显示  '...'
              pageNo:1,    //当前页码
              goPage:1,    //跳转用的页码
            }
        },
        watch:{
          pageNo(e){
            console.log(e)
            if(e>0 && e<=this.pageSum){
              this.$emit('changeP',JSON.stringify(e))
            }else{
              if(e<=0){
                this.$message({
                  message: '输入的页数小于1，请重新输入',
                  type: 'warning',
                  duration:'1500'
                });
              }else{
                this.$message({
                  message: '输入的页数大于总页数，请重新输入',
                  type: 'warning',
                  duration:'1500'
                });
              }

            }
            this.getPageArr()
          },
          total(){
             this.getPageArr()
          },
        },
        created(){
        },
        mounted() {
           this.getPageArr()
        },
        methods:{
          getPageArr(){//设置分页数组
            this.pageSum=parseInt(this.total/this.pageSize)
            if(this.total%this.pageSize!=0){
              this.pageSum++
            }
            console.log('总数:'+this.total)
            console.log('总页数:'+this.pageSum)
            this.pageArr=[]
            let falg=0; //用于去除重复的  '...'
            for(let i=1;i<=this.pageSum;i++){
              if(this.pageSum>=7&&(((i<=this.pageNo+2)&&(i>=this.pageNo-2))||i==this.pageSum||i=='1')){
                this.pageArr.push(i)
                if(falg==1){
                  falg--
                }

              }else if(this.pageSum>=7&&falg==0){
                this.pageArr.push('. . .')
                falg++
              }else if(this.pageSum<7){
                this.pageArr.push(i)
              }
            }
            console.log(this.pageArr)
          },
          pageChange(item){//点击页码
            if(item!='...'){
              this.pageNo=item
            }
          },
          pageReduce(){ //前一页
            if(this.pageNo!=1&&this.pageSum!=0){
              this.pageNo--
            }
          },
          pageAdd(){  //后一页
            if(this.pageNo!=this.pageSum&&this.pageSum!=0){
              this.pageNo++
            }
          },
          toPageNo(){  //跳转
            if(this.goPage){   //this.goPage<=this.pageSum
              this.pageNo=this.goPage
            }
          }
        }
    }
</script>

<style scoped lang="less">
  .pageMain{
    display: flex;
    align-items:center;
    .pageBtn{
      display: flex;
      justify-content: space-between;
      >p{
        background-color:#EDEDED;
        border:1px solid #CACACA;
        font-size:16px;
        font-family:Microsoft YaHei;
        font-weight:400;
        color:#999999;
        padding: 10px 13px;
        cursor:pointer;
        margin-right: 10px;
      }
      .pageTo{      //前一页、后一页的普通
        color: #333333;
        background-color:#FFFFFF;
        border:1px solid #CACACA;
        // padding: 11px 15px;
      }
      .noToPage{  //前一页、后一页的不能点击时
        background-color:#EDEDED;
        cursor:default;
      }
      .pageThis{ //页码为当前
        background-color: #FFF;
        color: #2778BE;
        border: none;
        cursor:default;
      }
      .seeMore{
        background-color: #FFF;
        padding-top: 5px;
        border: none;
        cursor:default;
      }
    }
    .pageGo{
      color: #333333;
      font-size:14px;
      font-family:Microsoft YaHei;
      font-weight:400;
      margin-left: 10px;
      >p{
        >input{
          background: rgba(166,166,238,0);
          border: 1px solid rgba(202, 202, 202, 1);
          width: 50px;
          margin: 0 5px;
          padding: 11px 0;
          text-align: center;
        }
      }
    }
    >button{
      cursor:pointer;
      margin-left: 43px;
      background:rgba(255,255,255,1);
      font-size:16px;
      font-family:Microsoft YaHei;
      font-weight:400;
      color:#333333;
      padding: 11px 23px;
      border: 1px solid rgba(202, 202, 202, 1);
    }

  }
</style>
