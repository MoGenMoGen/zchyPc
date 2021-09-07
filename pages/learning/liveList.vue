<template>
  <div id="home" :style="{width:bWidth + 'px'}">
    <img alt="" src="@/assets/img/learning/渔业装备轮播图.png" :style="{width:bWidth + 'px'}"/>
    <div class="header main" :style="{width:width + 'px'}">
      <el-breadcrumb class="board"  separator-class="el-icon-arrow-right" >
        <el-breadcrumb-item :to="{path: '../learning/learning'}">在线直播</el-breadcrumb-item>
        <el-breadcrumb-item ><p class="textBlue">{{catNm}}</p></el-breadcrumb-item>
      </el-breadcrumb>
      <div class="searched">
        <input v-model="search" placeholder="请输入课程名称" />
        <button @click="toSearch"><i class="el-icon-search"></i></button>
      </div>
    </div>
    <div class="body main" :style="{width:width + 'px'}">
        <live-list :list="list"></live-list>
    </div>
    <div class="pageInfo main" :style="{width:width + 'px'}">
      <el-pagination
        background
        @current-change="handleCurrentChange"
        layout="prev, pager, next"
        :total="total">
      </el-pagination>
    </div>

    <div class="footerImg" :style="{width:bWidth+'px'}">
      <img src="../../assets/img/footer.png" alt=""/>
    </div>
  </div>
</template>

<script>
  import {mapState} from "vuex";
  import liveList from "@/components/learning/liveList";
    export default {
        name: "lesson",
      data(){
        return{
          search:'',
          cid:'',
          catNm:'',
          type:'',
          total:0,
          pageNo:1,
          pageSize:30,
          list:[],
        }
      },
      components:{liveList},
      computed: {
        ...mapState([
          'bWidth',
          'width',
          'currentRole'
        ])
      },
      mounted(){
        this.cid = this.until.getQueryString('cid')
        this.catNm = this.until.getQueryString('nm')
        this.getData()
      },
      methods:{
        handleCurrentChange(val){
          this.pageNo = val
          this.getData()
        },
        toSearch(){
          this.pageNo = 1
          this.getData();
        },
        toPage(url){
          this.$router.push(url)
        },
        async getData(){
          let qry = this.query.new()
          this.query.toW(qry,'cid',this.cid,'EQ')
          this.query.toW(qry,'name',this.search,'LK')
          this.query.toP(qry,this.pageNo,this.pageSize)
          let data = await this.api.studyLive(this.query.toEncode(qry))
          this.list = data.data.list
          this.total = data.page.total
          console.log('data',data)
        }

      },
    }
</script>

<style lang="less" scoped>
  @import url("../../assets/css/init.less");
  #home{
    background-color: #FFFFFF;
    .header{
      margin-top: 20px;
      height: 60px;
      display: flex;
      justify-content: space-between;
      flex: 1;
      .board{
        line-height: 35px;
        .textBlue{
          color: #2778BE;
        }
      }
      .searched{
        height: 38px;
        margin-right: 20px;
        line-height: 38px;
        border-right: 0;
        border-radius: 5px;
        display: flex;
        display: -webkit-flex;
        align-items: center;
        >input{
          flex: 1;
          height: 100%;
          text-indent: 15px;
          border: 1px solid #DFDFDF;
          border-right: 0;
          box-sizing: border-box;
          border-radius: 5px 0 0 5px;
          width: 370px;
          color: #999999;
        }
        button{
          background: @themeColor;
          border: 1px solid @themeColor;
          color: #ffffff;
          height: 100%;
          padding: 0 20px;
          border-radius: 0 5px 5px 0;
          cursor: pointer;
          box-sizing: border-box;
        }
      }
    }
    .body{
      width: 100%;
      display: flex;
      flex-wrap: wrap;
      .part{
        width: 18.6%;
        margin-right: 1.75%;
        background-color: #F1F2F4;
        margin-top: 30px;
        border-radius: 10px;
        position: relative;
        cursor: pointer;

        .ok{
          position: absolute;
          max-height: 50px;
          max-width: 40px;
          top: 55px;
          left: 90px;
          opacity:0.55;
          cursor: pointer;
        }
        .child1{
          font-size: 16px;
          color: #333333;
          padding: 11px 11px 0 11px;
          line-height: 30px;
          overflow: hidden;
          text-overflow:ellipsis;
          white-space: nowrap;
        }
        .child2{
          padding-left: 11px;
          margin-top: 11px;
          font-size: 13px;
          color: #999999;
          overflow: hidden;
          text-overflow:ellipsis;
          white-space: nowrap;
        }
        .ad{
          width: 100%;
          height: 60%;
          border-radius: 10px 10px 0 0;
          >img{
            object-fit: cover;
            width: 100%;
            height:100%;
            border-radius: 10px 10px 0 0;
          }
        }
        &:nth-of-type(5n){
          margin-right: 0;
        }
      }
    }
    .footerImg{
      margin-top: 40px;
    }
  }
</style>
