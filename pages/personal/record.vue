<template>
<!--  选型记录-->
  <div id="home">
    <div class="title">
      <p><span class="lineC"></span>选型记录</p>
      <p @click="back"><i class="el-icon-arrow-left"></i>返回</p>
    </div>
    <div class="header">
      <el-select v-model="search.type" clearable  placeholder="船舶类型">
        <el-option
          v-for="item in typeList"
          :key="item.cd"
          :label="item.nm"
          :value="item.cd">
        </el-option>
      </el-select>
      <el-select v-model="search.material" clearable  placeholder="船舶材质">
        <el-option
          v-for="item in materialList"
          :key="item.cd"
          :label="item.nm"
          :value="item.cd">
        </el-option>
      </el-select>
      <el-select v-model="search.length" clearable  placeholder="公约船长m">
        <el-option
          v-for="item in lengthList"
          :key="item.cd"
          :label="item.nm"
          :value="item.cd">
        </el-option>
      </el-select>
      <div class="searched">
        <!--<el-date-picker-->
          <!--v-model="search"-->
          <!--type="daterange"-->
          <!--align="right"-->
          <!--unlink-panels-->
          <!--value-format="yyyy-MM-dd"-->
          <!--range-separator="至"-->
          <!--start-placeholder="开始日期"-->
          <!--end-placeholder="结束日期"-->
          <!--:picker-options="pickerOptions">-->
        <!--</el-date-picker>-->
        <button @click="toSearch"> <i class="el-icon-search"></i></button>
      </div>
    </div>
    <div class="content1">
      <div v-for="(item,index) in list" :key="index" class="part" :class="{seen:item.isSeen}" @mouseenter="enter(index)" @mouseleave="leave(index)" @click="toPage(item)">

        <div class="partA">
          <p>{{item.nm}}</p>
          <p>船舶户籍：{{item.registry}}<span></span>船舶类型：{{item.typesNm}}<span></span>船舶航区：{{item.sailingAreaNm}}<span></span>船舶材质：{{item.hullMaterialNm}}</p>
          <p><span>提交时间：</span>{{item.crtTm}}</p>
        </div>
        <div class="partB" >
          <p>
            <span v-if="currentRole.identityCd=='identity20'">￥{{item.budget/10000}}万元</span>
            <span v-else-if="item.price">￥{{item.price}}万元起</span>
            <span v-else @click.stop="toService">暂无报价，详情请咨询客服</span>
             <i class="el-icon-arrow-right" ></i>
          </p>
        </div>
        <div class="partC" v-if="currentRole.identityCd=='identity20'" >
          <el-button type="primary" icon="el-icon-copy-document" class="copy" @click.stop="toDo(item,'copy')">复制</el-button>
          <el-button type="primary" icon="el-icon-edit" class="edit" @click.stop="toDo(item,'edit')">编辑</el-button>
          <el-button type="primary" icon="el-icon-view" class="see" @click.stop="toDo(item,'detail')">查看</el-button>
        </div>
      </div>
    </div>
    <div class="Footer">
      <el-pagination
        background
        @current-change="handleCurrentChange"
        :current-page.sync="currentPage3"
        :page-size="pageSize"
        layout="prev, pager, next, jumper"
        :total="totalNm">
      </el-pagination>
    </div>
  </div>
</template>

<script>
  import {mapState} from "vuex";

  export default {
    layout:'person',
    name: "record",
    components: { },
    data(){
      return{
        pickerOptions: {
          shortcuts: [{
            text: '最近一周',
            onClick(picker) {
              const end = new Date();
              const start = new Date();
              start.setTime(start.getTime() - 3600 * 1000 * 24 * 7);
              picker.$emit('pick', [start, end]);
            }
          }, {
            text: '最近一个月',
            onClick(picker) {
              const end = new Date();
              const start = new Date();
              start.setTime(start.getTime() - 3600 * 1000 * 24 * 30);
              picker.$emit('pick', [start, end]);
            }
          }, {
            text: '最近三个月',
            onClick(picker) {
              const end = new Date();
              const start = new Date();
              start.setTime(start.getTime() - 3600 * 1000 * 24 * 90);
              picker.$emit('pick', [start, end]);
            }
          }]
        },
        currentPage3: 1,
        search:{
          type:'',
          material:'',
          length:'',
        },
        totalNm:0,
        id: 4992971867640832,
        pageSize:6,
        list:[],
        currentRole:{},
        typeList:[],//船舶类型
        materialList: [],//船舶材质
        lengthList:[], //船舶总长
      }
    },
    computed:{
      ...mapState([
        'bWidth',
        'width',
      ])
    },
   async mounted() {
      this.currentRole = JSON.parse(this.until.seGet('currentRole'))
      this.typeList=await this.api.dataDictionary('SHIP_TYPES')
      this.materialList=await this.api.dataDictionary('HULL_MATERIAL')
      this.lengthList=await this.api.dataDictionary('SHIP_LENGTH')
      this.getData()
    },
    methods:{
      //客服电话弹窗
      toService(){
        this.$alert('客服电话：'+this.until.seGet('tel'), '咨询客服', {
          confirmButtonText: '确定',
        });
      },
      async getData(){
        let qry = this.query.new()
        // if(this.search){
        //   this.query.toW(qry,"crtTm",this.search.join(','),'BT')
        // }
        this.query.toW(qry,'typesCd',this.search.type,'EQ')
        this.query.toW(qry,'hullMaterialCd',this.search.material,'EQ')
        this.query.toW(qry,'lengthCd',this.search.length,'EQ')
        // if(this.search.length){
        //   let lengthNum = this.lengthList.find(item=>item.cd==this.search.length)
        //   console.log(lengthNum)
        //   // console.log(lengthNum.nm.replace(/[^0-9]/ig,'-'))
        //   // let lengthArr = lengthNum.nm.replace('L','').replace(/[^0-9]/ig,'-').split('-')
        //     this.query.toW(qry,'totalLen',lengthNum.arg1,'GE') //在于等于
        //   this.query.toW(qry,'totalLen',lengthNum.arg2,'LT') //小于
        // }
        this.query.toO(qry,'crtTm','desc')
        this.query.toP(qry,this.currentPage3,this.pageSize)
        let data = null
        if(this.currentRole.identityCd=='identity20'){//设计院
           data = await this.api.xuanxingList(this.query.toEncode(qry))
        }else {
          data = await this.api.diyList(this.query.toEncode(qry))
        }

        data.data.list.forEach(item=>{
          item.crtTm = item.crtTm ? item.crtTm.split(' ')[0] : ''
        })
        this.list = data.data.list
        this.totalNm = data.page.total
      },
      //操作，复制、编辑、查看详情
      toDo(item,type){
        if(type=='copy'){
            this.api.xuanxingCopy(item.id).then(()=>{
              this.$message({
                message: '复制成功',
                type: 'success'
              });
               this.currentPage3 = 1
               this.getData()
            })
        }else if(type=='edit'){
          this.$router.push({
            path: './seleQuo',
            query:{
              id:item.id,
              type:'edit'
            }
          })
        }else if(type=='detail'){
          window.open('./personal/seleQuo?id='+item.id+'&type=detail')
        }
      },
      handleCurrentChange(val) {
        this.currentPage3 = val
        this.getData()
      },
      back(){
        this.$router.go(-1)
      },
      toSearch(){
        this.currentPage3 = 1
        this.getData()
      },
      toPage(item){
        if(this.currentRole.identityCd=='identity20'){ //设计院
          window.open('./personal/seleQuo?id='+item.id+'&type=detail')
        }else {
          this.$router.push({
            path: './recordDetail',
            query:{
              id:item.id,
            }
          })
        }

      },
      enter(index){
        this.list[index].isSeen = true
      },
      leave(index){
        this.list[index].isSeen = false
      }
    },
  }
</script>

<style lang="less" scoped>
  @import url("../../assets/css/init.less");
  #home{

  }
  .title{
    height: 59px;
    border-bottom: 1px solid #F3F3F3;
    width: 100%;
    display: inline-flex;
    justify-content: space-between;
    line-height: 59px;
    p:nth-child(1){
      color: #333333;
      font-size: 20px;
      display: flex;
      align-items: center;
      margin-left: 30px;
      .lineC{
        display: block;
        height: 18px;
        width: 3px;
        background-color: #2778BE;
        margin-right: 8px;
      }
    }
    p:nth-child(2) {
      color: #666666;
      font-size: 14px;
      margin-right: 24px;
      cursor: pointer;
    }
  }
  .header{
    height: 78px;
    display: flex;
    align-items: center;
    /*justify-content: flex-end;*/
    border-bottom: 1px solid #F3F3F3;
    padding-right: 20px;
    padding-left: 20px;
    /deep/.el-select{
      margin-right: 10px;
    }
    .searched{
      height: 38px;
      line-height: 38px;
      border-right: 0;
      display: flex;
      display: -webkit-flex;
      align-items: center;
      border-radius: 5px;
      /deep/.el-date-editor .el-range-separator{
        width: auto;
      }
      >input{
        flex: 1;
        height: 100%;
        text-indent: 15px;
        border: 1px solid #DFDFDF;
        border-right: 0;
        box-sizing: border-box;
        border-radius: 5px;
        width: 300px;
      }
      button{
        background: @themeColor;
        border: 1px solid @themeColor;
        color: #ffffff;
        height: 100%;
        padding: 0 20px;
        border-radius: 2px;
        cursor: pointer;
        box-sizing: border-box;
      }
    }
  }
  .content1{
    width: 100%;
    .seen{
      background-color: #F6F6F6;
      color: #2778BE;
    }
    .part{
      width: 100%;
      padding: 20px 28px;
      display: flex;
      display: -webkit-flex;
      align-items: center;
      justify-content: space-between;
      border-bottom: 1px solid #F3F3F3;
      cursor: pointer;
      .partA{
        flex: 1;
        >p{
          font-size: 15px;
        }
        p:nth-child(1){
          width: 100%;
          margin-bottom: 20px;
        }
        p:nth-of-type(2){
          width: 100%;
          height: 30px;
          display: -webkit-box;
          -webkit-box-orient: vertical;
          -webkit-line-clamp: 1;
          overflow: hidden;
          line-height: 30px;
          span{
            width: 30px;
            display: inline-block;
          }
        }
        p:last-of-type{
          margin-top: 20px;
          color: #999999;
          font-size: 14px;
        }
      }
      .partB{
        width: 18%;
        padding-left: 50px;
        box-sizing: content-box;
        p{
          display: flex;
          justify-content: space-between;
          color: #E4393C;
          font-size: 18px;
          cursor: pointer;
          >i{
            color: #666666;

          }
        }
      }
      .partC{
        display: flex;
        flex-direction: column;
        >button{
          margin: 0 0 15px 12px;
          background-color: #FFFFFF;
          font-size:14px;
          border-radius:3px;
          height: 29px;
          padding: 9px 21px;
        }
        .copy{
          border:1px solid rgba(39, 120, 190, 1);
          color:rgba(39,120,190,1);
        }
        .edit{
          border:1px solid #E3882E;
          color:#E3882E;
        }
        .see{
          border:1px solid #2B891E;
          color:#2B891E;
        }
      }
    }
  }
  .Footer{
    margin-top: 40px;
    width: 100%;
    margin-bottom: 30px;
    display: inline-flex;
    justify-content: center;
  }

</style>
