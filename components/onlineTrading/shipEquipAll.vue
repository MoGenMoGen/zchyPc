<template>
  <div class="shipEquipAll" ref="shipEquip">
    <div>
      <div class="title">
          <el-select v-model="index" class="eqiupName">
            <el-option
              v-for="item in nmList"
              :key="item.index"
              :label="item.nm"
              :value="item.index">
            </el-option>
          </el-select>
      </div>
       <div class="list">
          <ul>
            <li v-for="(v,i) in listNow.equipList" :class="{active:v.check}" :style="{height:(width*0.30-10*3)/3+'px',width:(width*0.30)/3-10+'px'}" :key="i" @click="toCheck(index,i)">
              <span v-show="v.check"><img :src="check"/> </span>
              <p>
                <span>{{v.nm}}</span>
                <img :src="v.imgUrl"/>
              </p>
            </li>
          </ul>
         <div class="pageList">
           <el-table
             height="500"
             :data="list[index].tableList"
             class="table">
             <el-table-column
               label="序号"
               type="index"
               align="center"
               width="50">
             </el-table-column>
             <el-table-column
               label="图片"
               align="center"
               width="100">
               <template slot-scope="scope">
                 <img :src="scope.row.imgUrl"/>
               </template>
             </el-table-column>
             <el-table-column
               prop="nm"
               label="名称"
               width="140">
             </el-table-column>
             <el-table-column
               prop="deviceType"
               label="型号"
               width="120">
             </el-table-column>
             <el-table-column
               prop="num"
               label="数量"
               width="80">
             </el-table-column>
             <!-- <el-table-column
               prop="price"
               label="价格(￥)"
               width="100">
             </el-table-column> -->
             <el-table-column
               prop="deviceSupply"
               label="供应商"
               width="150">
             </el-table-column>
             <el-table-column
               v-if="ifPerson"
               fixed="right"
               label="操作"
               align="center"
               width="90">
               <template slot-scope="scope">
                 <div class="set">
                   <img :src="cart"/>
                   <img :src="repair"/>
                 </div>
               </template>
             </el-table-column>
             <el-table-column
               v-if="ifAddShip"
               fixed="right"
               label="操作"
               align="center"
               width="90">
               <template slot-scope="scope">
                 <el-button @click='toDel(scope.row)' >删除</el-button>
               </template>
             </el-table-column>
           </el-table>
           <!--<el-pagination-->
             <!--layout="prev, pager, next"-->
             <!--@current-change="handleCurrentChange($event,index)"-->
             <!--:page-size="pageSize"-->
             <!--:total="item.total">-->
           <!--</el-pagination>-->
         </div>

       </div>
    </div>
<!--    <div class="footer" v-if="ifPerson">
      <p>合计： <span class="red">10 件</span>  设备 加入了购物车，<span class="red">2 件 </span> 设备 加入了维保单。</p>
      <button>去维保单</button>
      <button>去购物车</button>
    </div> -->
  </div>
</template>

<script>
  import {mapState} from "vuex";
  import service1 from '../img/service1.png'
  import check from '../img/check.png'
  import cart from '../img/toCart.png'
  import repair from '../img/toRepair.png'
  export default {
    name: "productList",
    // id:设计方案id  ways:是新增还是修改  listA:新增的设备列表   delG:修改中被删除的设备列表
    props:['width','ifPerson','id','ifAddShip','ways','listA','delGDeviceList'],
    data(){
      return{
        check,
        cart,
        repair,
        list:[{nm:'船体设备',equipList:['','','','','']}],
        listNow:{},
        nmList:[],
        index:0,
        pageSize:5,
      }
    },
    computed:{
      ...mapState([


      ]),

    },
    watch:{
      // id:{
      //   handler:function () {
      //     if(this.id){
      //       this.getClassify()
      //     }
      //   },
      //   immediate:true
      // },
      index(){
        console.log('变了 : '+this.index)
        this.listNow=this.list[this.index]
      },
      delGDeviceList(){
        this.addGoodsC(this.delGDeviceList[this.delGDeviceList.length-1])
        // console.log(this.delGDeviceList)
      },
      ways(){
        if(this.ways=='copy'){
          this.getClassifyAll()
        }
      }
      // listA(){
      //   console.log('listA变化')
      //   // this.toCheck(index,)
      // },
    },
    mounted(){
      if(this.ifAddShip){   //如果是从新增设计方案进入，调取所有设备列表
        this.getClassifyAll()
      }
    },
    methods:{
      // //选择页码
      // async handleCurrentChange(val,index){
      //   // console.log(val)
      //   // console.log(index)
      //   this.list[index].pageNo = val
      //   let qry = this.query.new()
      //   // this.query.toW(qry,'cids',this.list[index].id,'LK')
      //   this.query.toP(qry,this.list[index].pageNo,this.pageSize)
      //   // let data = await this.api.fileDeviceList2(this.query.toEncode(qry),{docsId:this.id,cId:this.list[index].id})
      //   let data = null
      //   if(this.type==1){ //个人中心
      //     data = await this.api.fileDeviceList2(this.query.toEncode(qry),{docsId:this.id,cId:this.list[index].id})
      //   }else if(this.type==2){ //船舶方案
      //     this.query.toW(qry,'cids',this.list[index].id,'LK')
      //     data = await this.api.designGoodsPage(this.query.toEncode(qry))
      //   }else if(this.type==3){ //船舶详情
      //     // this.query.toW(qry,'cids',this.list[i].id,'LK')
      //     data = await this.api.shipDetailEqupList({id:this.id,cid:this.list[index].id},this.query.toEncode(qry))
      //   }
      //   data.data.list.forEach(item=>{
      //     item.imgUrl = item.imgUrl?item.imgUrl.split(',')[0] : ''
      //     item.model = item.model?item.model:item.deviceType
      //     item.price = item.price ? item.price : item.origMinPrice
      //     item.supplierNm = item.supplierNm?item.supplierNm:item.deviceSupply
      //   })
      //
      //   this.list[index].tableList = data.data.list
      //   this.list[index].total = data.page.total
      //   this.$set(this.list,index,this.list[index])
      // },
      addGoodsC(item){  //父组件添加了设备
        console.log('子组件中要跳转')
        this.list.forEach((v,k)=>{
          if(v.id==item.cid1){
            this.index=k
            this.listNow=this.list[k]
            this.listNow.equipList.forEach((x,y)=>{
              if(x.id==item.cid2)
              this.toCheck(k,y)
            })

          }
        })
      },
      delList(){
          this.toCheck(this.index,0)
      },
      toDel(item){//删除设备
        // console.log(skuId)
        this.$emit('toDel', JSON.stringify(item))
      },
      async getClassifyAll(){
        console.log('获取')
        this.nmList=[]
        let qry = this.query.new()
        // this.query.toW(qry,'pid','4942035725390848','EQ')
        // this.query.toO(qry,'seq','asc')
        let data=await this.api.shipCatList(this.query.toEncode(qry))
        // console.log('分类列表')
        // console.log(data.data)
        this.list = []
        for(let [k,v] of Object.entries(data.data)){
          if(v.length>0){
            this.list.push({
              nm:k,
              equipList:v,
              id:v[0].tid
            })
          }
        }
        this.list.forEach((item,index)=>{
          this.nmList.push({
            nm:item.nm,
            index:index
          })
        })
        console.log('nmList')
        console.log(this.nmList)
        let copyList=[]
        for(let i=0;i<this.list.length;i++){
          if(this.list[i].equipList.length>0){
            this.list[i].equipList[0].check=true
            if(!this.ways){
              let qry = this.query.new()
              this.query.toW(qry,'solutionId',this.id,'EQ')
              this.query.toW(qry,'cid2',this.list[i].equipList[0].id,'EQ')
              this.list[i].tableList=await this.api.designGoodsList(this.query.toEncode(qry))
              this.list[i].tableList.forEach((v,k)=>{
                v.type='jiekou'
              })
            }else if(this.ways=='copy'){

              // this.list[i].equipList.forEach((item,index)=>{
              //   let param1 = {
              //     id:this.id,
              //     cid:this.list[i].equipList[index].id
              //   }
              //   this.api.shipDetailEqupList(param1).then(res=>{
              //     res.forEach(v=>{
              //       v.type='xinzen'
              //       copyList.push(v)
              //     })
              //   })

              // })
              let param = {
                id:this.id,
                cid:this.list[i].equipList[0].id
              }
              let data = await this.api.shipDetailEqupList(param)
              this.list[i].tableList = data.data.list
              this.list[i].tableList.forEach((v,k)=>{
                v.type='xinzen'
              })

            }

            this.$set(this.list,i,this.list[i])
          }
        }
        // console.log(copyList)
        // this.$emit('copyDevice', JSON.stringify(copyList))
        this.listNow=this.list[0]
        // this.toCheck(0,0)
        console.log('左侧列表')
        console.log(this.list)
        console.log('当前列表')
        console.log(this.listNow)
      },
      // async getClassify(){
      //   let data = await this.api.shipDetailEqupClassify(this.id)
      //   console.log('分类列表')
      //   console.log(data)
      //   this.list = []
      //   for(let [k,v] of Object.entries(data)){
      //     if(v.length>0){
      //       this.list.push({
      //         nm:k,
      //         equipList:v
      //       })
      //     }
      //   }
      //   console.log('左侧列表')
      //   console.log(this.list)
      //   console.log(this.id)
      //   for(let i=0;i<this.list.length;i++){
      //     if(this.list[i].equipList.length>0){
      //       this.list[i].equipList[0].check=true
      //       // let param = {
      //       //   id:this.id,
      //       //   cid:this.list[i].equipList[0].id
      //       // }
      //       // this.list[i].tableList  = await this.api.designGoodsList(param)
      //       let qry = this.query.new()
      //       this.query.toW(qry,'solutionId',this.id,'EQ')
      //       this.query.toW(qry,'cid2',this.list[i].equipList[0].id,'EQ')
      //       this.list[i].tableList=await this.api.designGoodsList(this.query.toEncode(qry))
      //       this.$set(this.list,i,this.list[i])
      //     }
      //   }
      //   // this.toCheck(0,0)
      //   console.log()
      //   console.log(this.list)
      // },
      async toCheck(m,n){
        console.log('选择')
        this.list[m].equipList.forEach(item=>{
          item.check=false
        })
        this.list[m].equipList[n].check=true
        this.listNow.equipList[n].check=true
        this.$set(this.listNow.equipList,n,this.listNow.equipList[n])

        let listB=[]

        if(!this.ways){ //修改设计方案，有可能已经有设备
          let qry = this.query.new()
          this.query.toW(qry,'solutionId',this.id,'EQ')
          this.query.toW(qry,'cid2',this.list[m].equipList[n].id,'EQ')
          let data=await this.api.designGoodsList(this.query.toEncode(qry))
          data.forEach(item=>{
            let flag=0
            this.delGDeviceList.forEach(v=>{
              if(v.id==item.id){
                flag++
              }
            })
            if(flag==0){
              item.type='jiekou'
              listB.push(item)
            }

          })
        }

        console.log(this.listA)
        console.log('m :'+m+'n :'+n)
        console.log(this.list[m].equipList[n].id)
        this.listA.forEach(item=>{
          if(this.list[m].equipList[n].id==item.cid2){
            item.type='xinzen'
            listB.push(item)
          }
        })
        console.log('listB')
        console.log(listB)

        console.log(this.listNow)
        this.list[m].tableList = listB
        this.$set(this.list,m,this.list[m])
        this.listNow = this.list[m]

        // this.listNow.tableList.forEach((item,index)=>{
        //   this.$set(this.listNow.tableList,index,this.listNow.tableList[index])
        // })
        // this.$set(this.listNow,'tableList',listB)

        console.log(this.listNow)

      }
    }
  }
</script>
<style lang="less">
  .eqiupName{
    height: 100%;

    .el-input__inner{
      background: #F7F7F7;
      border: none;
    }
  }
  .table thead th{
    background: #F7F7F7;
    border-bottom: 0!important;
  }

  ::-webkit-scrollbar {
    width: 8px;
    height: 7.6px;
    border-radius: 5px;
    background-color: #F4F4F4;
    /*border-bottom: 1px solid #ECECEC;*/
    /*border-top: 1px solid #ECECEC;*/
  }
  ::-webkit-scrollbar-thumb {
  /*// border-radius:5px;*/
    background-color: #DEDEE4;
    border-radius: 5px;
    margin: 5px 0;
  }
</style>
<style lang="less" scoped>
  @import url("../../assets/css/init.less");

  .shipEquipAll{
    color: #666666;
    .title{
      width: 100%;
      background: #F7F7F7;
      border: 1px solid #eeeeee;
      height: 40px;
      line-height: 40px;
      text-indent: 28px;
      color: #666666;
      margin: 30px 0;
    }
    .list{
      width: 100%;
      overflow: hidden;
      margin-bottom: 50px;
      ul{
        float: left;
        width: 33%;
        li{
          float: left;
          margin-right: 10px;
          border: 1px solid #DFDFDF;
          display: flex;
          display: -webkit-flex;
          cursor: pointer;
          position: relative;
          margin-bottom: 25px;
          box-sizing: border-box;
          span{
            position: absolute;
            z-index: 3;
            top:0;
            right: 0;
            img{
              width: 45px;
            }
          }
          p{
            width: 100%;
            position: relative;
            >span{
              position: absolute;
              bottom: 0;
              left:0;
              width: 100%;
              text-align: center;
              background: rgba(0,0,0,0.5);
              color: #FFFFFF;
              overflow: hidden;
              white-space: nowrap;
              text-overflow: ellipsis;
              height: 20px;
              font-size: 12px;
            }
            >img{
              width: auto;
              height: auto;
              max-width: 100%;
              max-height: 100%;
              margin: auto;
            }
          }
        }
        li.active{
          border: 1px solid @themeColor;
        }
      }
      .pageList{
        float: left;
        width: 67%;
        img{
          width: 76px;
          border: 1px solid #DFDFDF;
        }
        .set{
          img{
            width: 24px;
            border: 0;
            cursor: pointer;
          }
        }
      }
    }
    .footer{
      width: 100%;
      height: 50px;
      line-height: 50px;
      border: 1px solid #F0F0F0;
      box-sizing: border-box;
      display: flex;
      display: -webkit-flex;
      align-items: center;
      p{
        flex: 3.5;
        text-indent: 17px;
      }
      button{
        border: none;
        font-size: 20px;
        flex: 1;
        min-width: 101px;
        margin-left: 2%;
        color: #ffffff;
        height: 100%;
        cursor: pointer;
        &:first-of-type{
          background: #E64347;
        }
        &:last-of-type{
          background: @themeColor;
        }
      }
    }

  }


</style>
