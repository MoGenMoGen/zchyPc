<template>
  <div class="shipEquip" ref="shipEquip">
    <!--新增维保服务-->
    <task-note-add v-if="showAdd" :shipId="id" :shipInfo="repairInfo" @close="showAdd=false" @confirm="showAdd=false"></task-note-add>
    <div v-for="(item,index) in list" :key="index">
      <div class="title">{{item.nm}}</div>
       <div class="list">
          <ul>
            <li v-for="(v,i) in item.equipList" :class="{active:v.check}" :style="{height:(width*0.30-10*3)/3+'px',width:(width*0.30)/3-10+'px'}" :key="i" @click="toCheck(index,i)">
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
             :data="item.tableList"
             @row-click="toProDetail"
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
               prop="model"
               label="型号"
               width="120">
             </el-table-column>
             <el-table-column
               prop="num"
               label="数量"
               width="80">
             </el-table-column>
             <el-table-column
               prop="price"
               label="价格(￥)"
               width="100">
             </el-table-column>
             <el-table-column
               prop="supplierNm"
               label="供应商"
               width="150">
             </el-table-column>
             <el-table-column
               v-if="ifPerson"
               fixed="right"
               label="操作"
               align="center"
               width="50">
               <template slot-scope="scope">
                 <div class="set">
                   <!--<img :src="cart"  @click.stop="toAddCart(scope.row)"/>-->
                   <img :src="repair" v-if="info.statusCd=='DOCS_STATUS.03'" @click.stop="toRepair(scope.row)"/>
                 </div>
               </template>
             </el-table-column>
             <el-table-column
               v-if="ifAddShip"
               fixed="right"
               label="操作"
               align="center"
               width="50">
               <template slot-scope="scope">
                 <el-button>删除</el-button>
               </template>
             </el-table-column>
           </el-table>
           <el-pagination
             layout="prev, pager, next"
             @current-change="handleCurrentChange($event,index)"
             :page-size="pageSize"
             :total="item.total">
           </el-pagination>
         </div>

       </div>
    </div>
    <div class="footer" v-if="ifPerson">
      <p>合计： <span class="red">{{cartNum}} 件</span>  设备 加入了购物车。</p>
      <button @click="toPage('./repair')">去维保单</button>
      <button @click="toPage('../order/cart')">去购物车</button>
    </div>
  </div>
</template>

<script>
  import {mapState} from "vuex";
  import service1 from '../img/service1.png'
  import check from '../img/check.png'
  import cart from '../img/toCart.png'
  import repair from '../img/toRepair.png'
  import taskNoteAdd from "@/components/person/taskNoteAdd";
  export default {
    name: "productList",
    props:['width','ifPerson','id','ifAddShip','type'], //  type:'0'  1个人中心里的  2设计方案里的 3在线交易船舶详情里的

    components:{taskNoteAdd},
    data(){
      return{
        check,
        cart,
        repair,
        list:[{nm:'船体设备',equipList:['','','','','']}],
        pageSize:5,
        showAdd:false,
        repairInfo:null,
        info:null,
        cartNum:0,

      }
    },
    computed:{
      ...mapState([


      ]),

    },
    watch:{
      id:{
        handler:function () {
          if(this.id){
            this.getClassify()
          }
        },
        immediate:true
      },
      ifPerson:{
        handler:function () {
          if(this.ifPerson){
            this.api.shipInfoBasic(this.id).then(res=>{
              this.info= res
            })
          }
        },
        immediate:true
      },

    },
    mounted(){
      // console.log('mounted-----------------')
      if(this.ifAddShip){   //如果是从新增设计方案进入，调取所有设备列表
        // this.getClassifyAll()
      }
      // this.getClassify()
    },
    methods:{
      // async getClassifyAll(){
      //   console.log('获取')
      //   let qry = this.query.new()
      //   // this.query.toW(qry,'pid','4942035725390848','EQ')
      //   // this.query.toO(qry,'seq','asc')
      //   let data=await this.api.shipCatList(this.query.toEncode(qry))
      //   console.log('分类列表')
      //   console.log(data.data)
      //   this.list = []
      //   for(let [k,v] of Object.entries(data.data)){
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
      //     // if(this.list[i].equipList.length>0){
      //     //   this.list[i].equipList[0].check=true
      //     //   let param = {
      //     //     id:this.id,
      //     //     cid:this.list[i].equipList[0].id
      //     //   }
      //     //   this.list[i].tableList  = await this.api.shipDetailEqupList(param)
      //     // }
      //     //个人中心
      //     this.list[i].pageNo = 1
      //     let qry = this.query.new()
      //     this.query.toW(qry,'cids',this.list[i],'LK')
      //     this.query.toP(qry,this.list[i].pageNo,this.pageSize)
      //     this.list[i].tableList = await this.api.fileDeviceList(this.query.toEncode(qry),{docsId:this.id})
      //     this.$set(this.list,i,this.list[i])
      //
      //   }
      //   console.log(this.list)
      // },
      toPage(url){
        this.$router.push(url)
        // window.open(url)
      },
      //获取分类
      async getClassify(){
        let data = await this.api.shipDetailEqupClassify(this.id)
        console.log('分类列表')
        console.log(data)
        this.list = []
        for(let [k,v] of Object.entries(data)){
          if(v.length>0){
            this.list.push({
              nm:k,
              equipList:v,
              id:v.length>0?v[0].pid:'',
              pageNo:1,
              total:0,
            })
          }
        }
        // console.log('左侧列表')
        console.log(this.list)
        // console.log(this.type)
        for(let i=0;i<this.list.length;i++){
            let qry = this.query.new()
            // this.query.toW(qry,'cids',this.list[i].id,'LK')
            let data = null
            if(this.type==1){ //个人中心
              // this.query.toW(qry,'cids',this.list[i].id)
              this.query.toW(qry,'cid','5052238965953536')
              data = await this.api.fileDeviceList2(this.query.toEncode(qry),{docsId:this.id})
            }else if(this.type==2){ //船舶方案
				this.query.toP(qry,this.list[i].pageNo,this.pageSize)
              this.query.toW(qry,'cids',this.list[i].id,'LK')
              data = await this.api.designGoodsPage(this.query.toEncode(qry))
            }else if(this.type==3){ //船舶详情
				this.query.toP(qry,this.list[i].pageNo,this.pageSize)
              // this.query.toW(qry,'cids',this.list[i].id,'LK')
              data = await this.api.shipDetailEqupList({id:this.id,cid:this.list[i].id},this.query.toEncode(qry))
            }
            data.data.list.forEach(item=>{
              item.imgUrl = item.imgUrl?item.imgUrl.split(',')[0] : ''
              item.model = item.model?item.model:item.deviceType
              item.price = item.price ? item.price : item.origMinPrice
              item.supplierNm = item.supplierNm?item.supplierNm:item.deviceSupply
            })
            this.list[i].tableList = data.data.list
            this.list[i].total = data.page.total
            this.$set(this.list,i,this.list[i])
          // }
        }
        // console.log(this.list)
      },
      //选择页码
      async handleCurrentChange(val,index){
        // console.log(val)
        // console.log(index)
        this.list[index].pageNo = val
        let qry = this.query.new()
        // this.query.toW(qry,'cids',this.list[index].id,'LK')
        // this.query.toP(qry,this.list[index].pageNo,this.pageSize)
        // let data = await this.api.fileDeviceList2(this.query.toEncode(qry),{docsId:this.id,cId:this.list[index].id})
        let data = null
        if(this.type==1){ //个人中心
          data = await this.api.fileDeviceList2(this.query.toEncode(qry),{docsId:this.id,cId:this.list[index].id})
        }else if(this.type==2){ //船舶方案
          this.query.toW(qry,'cids',this.list[index].id,'LK')
		  this.query.toP(qry,this.list[index].pageNo,this.pageSize)
          data = await this.api.designGoodsPage(this.query.toEncode(qry))
        }else if(this.type==3){ //船舶详情
          // this.query.toW(qry,'cids',this.list[i].id,'LK')
		  this.query.toP(qry,this.list[index].pageNo,this.pageSize)
          data = await this.api.shipDetailEqupList({id:this.id,cid:this.list[index].id},this.query.toEncode(qry))
        }
        data.data.list.forEach(item=>{
          item.imgUrl = item.imgUrl?item.imgUrl.split(',')[0] : ''
          item.model = item.model?item.model:item.deviceType
          item.price = item.price ? item.price : item.origMinPrice
          item.supplierNm = item.supplierNm?item.supplierNm:item.deviceSupply
        })

        this.list[index].tableList = data.data.list
        // this.list[index].total = data.page.total
        this.$set(this.list,index,this.list[index])
      },
      //选择小类
      async toCheck(m,n){
        this.list[m].equipList.forEach(item=>{
          item.check=false
        })
        this.list[m].equipList[n].check=true
        this.list[m].pageNo = 1
        let qry = this.query.new()
        // this.query.toW(qry,'cids',this.list[m].equipList[n].id,'LK')
        this.query.toP(qry,this.list[m].pageNo,this.pageSize)

        let data = null
        // let data = await this.api.fileDeviceList2(this.query.toEncode(qry),{docsId:this.id,cId:this.list[m].equipList[n].id})

        if(this.type==1){ //个人中心
          data = await this.api.fileDeviceList2(this.query.toEncode(qry),{docsId:this.id,cId:this.list[m].equipList[n].id})
        }else if(this.type==2){ //船舶方案
          this.query.toW(qry,'cids',this.list[m].equipList[n].id,'LK')
          data = await this.api.designGoodsPage(this.query.toEncode(qry))
        }else if(this.type==3){ //船舶详情
          // this.query.toW(qry,'cids',this.list[i].id,'LK')
          data = await this.api.shipDetailEqupList({id:this.id,cid:this.list[m].equipList[n].id},this.query.toEncode(qry))
        }
        data.data.list.forEach(item=>{
          item.imgUrl = item.imgUrl?item.imgUrl.split(',')[0] : ''
          item.model = item.model?item.model:item.deviceType
          item.price = item.price ? item.price : item.origMinPrice
          item.supplierNm = item.supplierNm?item.supplierNm:item.deviceSupply
        })

        this.list[m].tableList = data.data.list
        // this.list[m].total = data.page.total
        this.$set(this.list,m,this.list[m])
      },
      //加入购物车
      toAddCart(item) {
        let param = {
          goodsId: this.id,
          goodsSkuId: item.skuId,
          qty: 1
        }
        this.api.addCart(param).then(() => {
          // console.log('掉接口')
          this.$message({
            message: '加入购物车成功！',
            type: 'success'
          });
          this.cartNum++
        })
      },
      //提交维保
      toRepair(info){
        this.repairInfo = info
        this.showAdd = true
      },
      //跳转设备详情
      toProDetail(info){
        let url = './Trade/productDetail?id='+info.id
        // this.toPage(url)
        window.open(url)
      }
    }
  }
</script>
<style>
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

  .shipEquip{
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
          >span{
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
