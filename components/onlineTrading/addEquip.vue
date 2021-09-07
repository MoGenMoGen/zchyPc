<template>
<!-- 新增设备——弹窗-->
  <div id="bg" v-show="show">
    <div  class="mainEquip">
      <div class="head">
        <p>
          <!-- <img class="iconZ" :src="marks" alt=""> -->
        </p>
        <img :src="close" @click="toClose" class="point">
      </div>
      <div class="bodyer">
        <div class="equipSele">
          <span>设备类型:</span>
          <el-select v-model="cid1" placeholder="请选择设备一级类型" class="equipSelect">
            <el-option
              v-for="item in list"
              :key="item.id"
              :label="item.nm"
              :value="item.id">
            </el-option>
          </el-select>
        </div>
        <!-- <div class="equipSele">
          <span>设备类型:</span>
          <el-select v-model="cid2" placeholder="请选择设备二级类型" class="equipSelect">
            <el-option
              v-for="item in cid2List"
              :key="item.id"
              :label="item.nm"
              :value="item.id">
            </el-option>
          </el-select>
        </div> -->
        <div class="equipSele">
          <span>商品名称:</span>
          <el-select v-model="cid3" placeholder="请选择设备具体商品" class="equipSelect">
            <el-option
              v-for="item in cid3List"
              :key="item.skuId"
              :label="item.skuName"
              :value="item.skuId">
            </el-option>
          </el-select>
        </div>
        <div class="equipSele">
          <span>商品数量:</span>
          <el-input-number v-model="num" :min="1" @change='changeNum'></el-input-number>
        </div>

        <!-- <div class="equipSele">
          <span>商品名称:</span>
          <el-select v-model="goodsId" placeholder="请选择船舶类型" class="equipSelect">
            <el-option
              v-for="item in goodsList"
              :key="item.id"
              :label="item.nm"
              :value="item.id">
            </el-option>
          </el-select>
        </div> -->
        <p class="equipBtn"><button @click="confirm">确认添加</button></p>
      </div>
    </div>
  </div>
</template>

<script>
    import {mapState} from "vuex";
    import close from "@/assets/img/close.png"
    import headRight from "@/assets/img/personal/右.png"
    import headLeft from "@/assets/img/personal/左.png"
    import unchecked from "@/assets/img/unchecked.png"
    import checked from "@/assets/img/checked.png"
    import marks from '@/assets/img/shipDisplay/角标.png'
    import message from "@/pages/personal/message";
    export default {
      layout:'person',
        name: "designAppo",
      components: {

      },
      props: ['show','solutionId'],
      data(){
          return{
            close,
            headRight,
            headLeft,
            unchecked,
            checked,
            marks,
            cid1:'',
            cid2:'',
            cid3:'',
            goodsId:'',
            cid1List:[],
            cid2List:[],
            cid3List:[],
            goodsList:[],
            list:[],
            num:1,
          }
      },
      computed:{
        ...mapState([
          'bWidth',
          'width',
          'loading',
        ])
      },
      watch:{
        cid1(){
          console.log('cid1变了 : '+this.cid1)
          // this.cid2List=this.cid1
          // this.cid2=''

          this.cid3=''
          this.getCid3List()
        },
        // cid2(){
        //   console.log('cid2变了 : '+this.cid2)
        //   this.getCid3List()
        //   this.cid3=''
        // },
        cid3(){
          console.log('cid3变了 : '+this.cid3)
          // this.getGoodsList()
          // this.goodsId=''
        },
      },
      mounted(){
        this.getClassifyAll()
      },
      methods: {
        changeNum(){
          console.log(this.num)
          if(this.num%1!=0){
            this.$message({
              message: '请输入整数',
              type: 'warning',
              duration: '1000',
            });
            this.num=Math.round(this.num)
            console.log(this.num)
          }
        },
        async getClassifyAll(){ //获取一级和二级列表
          console.log('获取')
          let qry = this.query.new()
          // this.query.toW(qry,'pid','4942035725390848','EQ')
          // this.query.toO(qry,'seq','asc')
          let data=await this.api.shipCatList(this.query.toEncode(qry))
          console.log('子组件——分类列表')
          console.log(data.data)
          this.list = []
          for(let [k,v] of Object.entries(data.data)){
            this.list.push({
              nm:k,
              id:v[0].tid,

            })
          }
          // for(let [k,v] of Object.entries(data.data)){
          //   this.list.push({
          //     nm:k,
          //     equipList:v,

          //   })
          // }
          console.log('左侧列表')
          console.log(this.list)
        },
        async getCid3List(){  //获取三级列表
          let qry = this.query.new()
          // this.query.toW(qry,'pid',this.cid2,'EQ')
          console.log(this.cid1)
          let param={
            tid:this.cid1,
          }
          this.cid3List=await this.api.designCatList(this.query.toEncode(qry),param)
          this.cid3List.forEach((item,index)=>{
            item.skuName=item.nm+'['+item.attrDesc+']'
            this.$set(this.cid3List,index,this.cid3List[index])
          })

          console.log('子组件——三级列表')
          console.log(this.cid3List)
        },
        async getGoodsList(){ //获取商品列表
          this.goodsList=await this.api.getCatList(this.cid3)
          console.log('子组件——商品列表')
          console.log(this.goodsList)
        },
        confirm(){
          console.log('cid3 :'+this.cid3)
          let index
          if(!this.cid3){
            this.$message({
              message: '请选择商品',
              type: 'warning',
              duration: '1000',
            });
            return
          }

          this.cid3List.forEach((item,k)=>{
            if(item.skuId==this.cid3){
              index=k
            }
          })
          let param={

            solutionId: this.solutionId,  //船舶设计方案id
            goodsId: this.cid3List[index].goodsId,      //商品id
            skuId: this.cid3List[index].skuId,      //sku id
            deviceType: this.cid3List[index].attrDesc,    // 设备型号
            attrDesc:this.cid3List[index].attrDesc, //规格名称
            deviceSupply: this.cid3List[index].supplier,  // 供应商
            // deviceSupplyId: this.deviceSupplyId,  //供应商id
            // cid: "分类id",
            cid1:  this.cid3List[index].tid,
            cid2:  this.cid3List[index].sid,
            cid3: this.cid3List[index].catId,
            cids: this.cid3List[index].tid+','+this.cid3List[index].sid+','+this.cid3List[index].catId,
            cd: this.cid3List[index].cd,   //编码
            nm: this.cid3List[index].nm,
            num: this.num,
            imgUrl: this.cid3List[index].imgUrl,
            price:this.cid3List[index].mktPrice ,  //最低价格
            // origMinPrice:this.origMinPrice ,  //最低价格
            // origMaxPrice:this.origMaxPrice ,  //最高价格
            // status: 0,  //状态 [r](0：上架，1：下架)
            // rmks:this.rmks ,//备注
            // intro: this.intro, //详细描述
          }
          this.$emit('confirm',JSON.stringify(param))
        },
        toClose(){
          console.log('关闭')
          this.$emit('close')
        },
      },

    }
</script>
<style lang="less">
    .equipSelect{
      height: 100%;
      .el-input {
        height: 100%;
      }
      .el-input__inner{
        height: 100%;
      }
    }
</style>
<style lang="less" scoped>
  // @import url("@/assets/css/init.less");
  #bg {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: rgba(0,0,0,.5);
    z-index: 500;
    display: flex;
    justify-content: center;
    align-items: center;

    .mainEquip{
      /*width: 54.8%;*/
      width: 550px;
      height: 70%;
      background-color: #FFFFFF;
      display: flex;
      flex-direction: column;
      border-radius:8px;
      .head{
        display: flex;
        justify-content: space-between;
        align-items: center;
        height: 59px;
        width: 100%;
        padding: 0 24px 0 30px;
        // border-bottom: 1px solid rgba(0,0,0,0.1);;
        >p{
          margin: -12px auto 0;
          width: 37.4%;
        }
        >img{
          width:20px;
          height:20px;
        }
      }
      .bodyer{
        display: flex;
        flex-direction: column;
        flex: 1;
        margin-top: 20px;
        overflow-y: auto;
        .equipSele{
          width: 100%;
          display: flex;
          align-items: center;
          padding: 0px 40px;
          height: 52px;
          margin-bottom: 30px;
          >span{
            width: 90px;
          }
          .equipSelect{
            flex: 1;
            margin-right: 30px;
          }
        }
        .equipBtn{
          height: 52px;
          text-align: center;
          >button{
            height: 100%;
            width: 40%;
            border: none;
            background: #2778be;
            font-size: 16px;
            color: #ffffff;
            cursor: pointer;
          }
        }
      }

    }
  }


  input::-webkit-input-placeholder {
    /* Chrome/Opera/Safari */
        color: #999;
  }
  input::-moz-placeholder {
      /* Firefox 19+ */
      color: #999;
  }
  input:-ms-input-placeholder {
      /* IE 10+ */
      color: #999;
  }
  input:-moz-placeholder {
      /* Firefox 18- */
      color: #999;
  }
  .point{
    cursor:pointer;
  }
  .floatL{
    float: left;
  }
  .floatR{
    float: right;
  }
</style>
