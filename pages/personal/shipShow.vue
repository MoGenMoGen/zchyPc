<template>
<!--  船舶展示-->
<div >
  <div class="head">
    <p><span class="lineC"></span>船舶展示</p>
    <span @click="back" class="point">< 返回</span>
  </div>
  <div class="search">
    <div class="shipNm">
      <input type="text" placeholder="搜索船舶名称" v-model="shipNm">
      <span class="point" @click="toSearch"><i class="iconfont icon-ziyuan"></i></span>
    </div>
    <button class="addDesign point" @click="toPage()">添加船舶设计</button>
  </div>
  <div class="mainShow">
     <div class="tableShow">
       <el-table
        :data="tableData"
        style="width: 100%">
          <el-table-column
            prop="no"
            align="center"
            width="100"
            label="序号">
          </el-table-column>
          <el-table-column
            prop="nm"
            width="250"
            label="船舶名称">
          </el-table-column>
          <el-table-column
            align="center"
            width="300"
            label="船舶图片">
            <template slot-scope="scope">
              <el-carousel class="imgLun">
                <el-carousel-item v-for="item in scope.row.imgList" :key="item" >
                  <img :src="item" alt="" style="width: 100%"/>
                </el-carousel-item>
              </el-carousel>
            </template>

          </el-table-column>
          <el-table-column
            width="160"
            align="center"
            prop="crtTm"
            label="上传时间">
          </el-table-column>
          <el-table-column
            fixed="right"
            align="center"
            width="120"
            prop="operations"
            label="操作">
            <template slot-scope="scope">
              <el-button type="text" size="small" @click="revise(scope.row)">修改</el-button>
              <span class="lineC"></span>
              <el-button type="text" size="small" @click="delFile(scope.row.id)">删除</el-button>
            </template>

          </el-table-column>
       </el-table>
     </div>
     <div class="Footer">
       <el-pagination
         background
         @size-change="handleSizeChange"
         @current-change="handleCurrentChange"
         :current-page.sync="currentPage3"
         :page-size="pageSize"
         layout="prev, pager, next, jumper"
         :total="total">
       </el-pagination>
     </div>
  </div>
</div>
</template>

<script>
    import {mapState} from "vuex";
    import pageInation from "@/components/pageInation.vue"
    export default {
      layout:'person',
        name: "shipowner",
      components: {
        pageInation
      },
      data(){
          return{
            orderNo:'',
            cd:'',
            shipNm:'',
            tabId:1,
            statusL:'全部',
            options: [{
                value: '0',
                label: '全部'
              }, {
                value: '1',
                label: '待付款'
              }, {
                value: '2',
                label: '待发货'
              }, {
                value: '3',
                label: '待收货'
              }, {
                value: '4',
                label: '已完成'
              }],
            value: '',
            tableData: [],
            currentRole:{},
            finishTm:'',
            total:0,
            pageSize:5,
            currentPage3: 1,
          }
      },
      computed:{
        ...mapState([
          'bWidth',
          'width'
        ])
      },
      mounted() {
        this.getInfo()
      },
      watch:{
        $route(){
          this.cd = this.until.getQueryString('cdType')
        },
      },
      methods: {
        revise(item){//修改
          console.log(item)
          this.$router.push({
            path:'./addShip?id='+item.id
          })
        },
        delFile(id){//删除
          console.log(id)
          this.api.delDesignFile(id).then(res=>{
            if(res.code==0){
              this.$message({
                message: '删除成功',
                type: 'success',
                duration:'1000'
              });
            }
            this.getInfo()
          })
        },
        toSearch(){
          this.getInfo()
        },
        async getInfo(){
          this.tableData=[]
          let qry = this.query.new()
          if(this.shipNm){
            this.query.toW(qry,'nm',this.shipNm,'LK')
          }
          this.currentRole=JSON.parse(this.until.seGet('currentRole'))
          this.query.toW(qry,'designId',this.currentRole.id,'EQ')
          this.query.toP(qry,this.currentPage3,this.pageSize)
          let data= await this.api.shipList(this.query.toEncode(qry))
          this.total=data.page.total
          data.data.list.forEach((item,index)=>{
            item.no=index+1
            item.imgList=item.imgUrl.split(',')
            item.crtTm=item.crtTm.split(' ')[0]
            this.tableData.push(item)
          })
          console.log(this.tableData)
        },
        handleSizeChange(val) {
          console.log(`每页 ${val} 条`);
        },
        handleCurrentChange(val) {
          console.log(`当前页: ${val}`);
          this.currentPage3=val
          this.getInfo()
        },
        toPage(){
          this.$router.push({
            path:'./addShip?ways=add'
          })
        },
        toDetail(item){//
        },
        back(){//返回
			this.$router.back()
        },
      },
    }
</script>
<style lang="less">
  .endTm {  //时间选择
    .el-date-editor.el-input{
      border-radius: 5px;
      border: 1px solid rgba(223, 223, 223, 1);
      width: 81.9%;
      height: 100%;
      cursor:pointer;
    }
    .el-input__inner{
      height: 100%;
      cursor:pointer;
    }
  }

  .status .el-input__inner{     //下拉框
          height: 36px;
          line-height: 36px;
  }
  .tableShow{
    .el-table__header thead{
      color: #666666;
      font-size:16px;
      font-weight:400;
    }
    .el-table__header th{
      background-color: #F7F7F7;

    }
    .el-table__body tr{
      color: #333333;
      font-size:15px;
    }
    .el-table__fixed-right{
      height: 100%!important;
    }
    .imgLun{
      height: 90px;
      .el-carousel__container{
        height: 100%;
        img{
          height: 100%;
        }
      }
    }
  }
</style>
<style lang="less" scoped>
  @import url("../../assets/css/init.less");
  .point{
    cursor:pointer;
  }
  .head{
    display: flex;
    justify-content: space-between;
    align-items: center;
    height: 59px;
    width: 100%;
    padding: 0 24px 0 30px;
    border-bottom: 1px solid rgba(0,0,0,0.1);;
    >p{
      display: flex;
      align-items: center;
      font-size:20px;
      font-family:Microsoft YaHei;
      font-weight:400;
      color:#333333;
      height: 18px;
      .lineC{
        display: block;
        height: 18px;
        width: 3px;
        background-color: #2778BE;
        margin-right: 8px;
      }
    }
    >span{
      display: block;
      height:12px;
      font-size:14px;
      font-family:Microsoft YaHei;
      font-weight:400;
      color: #666666;
    }
  }
  .search{
    width: 100%;
    height: 78px;
    padding: 20px 22px;
    display: flex;
    // justify-content: space-between;
    align-items: center;
    >div{
      display: flex;
      align-items: center;
      height: 38px;
      box-sizing: content-box;
      >span{
        white-space: nowrap;
        display: inline-block;
        height: 12px;
        font-size: 14px;
        margin-right: 20px;
        font-family: Microsoft YaHei;
        font-weight: 400;
        line-height: 12px;
        color: #333333;
      }
    }
    .shipNm{
      position: relative;
      width: 30.6%;
      background:rgba(255,255,255,1);
      border:1px solid rgba(223, 223, 223, 1);
      border-radius:5px;
      padding-left: 12px;
      >input{
        width: 77.6%;
        height: 100%;
        border: none;
      }
      >span{
        position: absolute;
        display: flex;
        justify-content: center;
        align-items: center;
        margin: 0;
        right: 0;
        width: 18.3%;
        height:100%;
        background:rgba(39,120,190,1);
        border-bottom-right-radius: 5px;
        border-top-right-radius: 5px;
        .icon-ziyuan{
          color: #ffffff;
        }
      }
    }
    .addDesign{
      width: 11.8%;
      height:38px;
      background:rgba(39,120,190,1);
      border: none;
      border-radius:3px;
      font-size:14px;
      color:rgba(255,255,255,1);
      margin-left: 20px;
    }
  }
  .mainShow{
    width: 100%;
    .tableShow{
      width: 100%;
      .btnList{
        display: flex;
        justify-content: center;
        align-items: center;
        // flex-direction: column;
        >button{
          flex: 1;
        }
        .lineC{
          width: 1px;
          height:15px;
          background:rgba(0,0,0,1);
          opacity:0.22;
          // margin: 0 5px;
        }
      }

    }
    .Footer{
      margin-top: 40px;
      width: 100%;
      display: inline-flex;
      justify-content: center;
    }
  }

</style>
