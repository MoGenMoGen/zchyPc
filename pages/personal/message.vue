<template>
<!--  消息列表-->
    <div class="main">
      <div style="padding:0 30px">
        <tobbar :title="title" :showAll="false"></tobbar>
      </div>
      <p style="border-bottom: 1px solid rgba(0,0,0,0.1);"></p>

      <div class="line4">
        <div v-for="item in list" :key="item.id" @click="toDetail(item)">
          <span v-if="!item.status"></span>
          <p>{{item.nm}}</p>
          <p>{{item.sendTm}}</p>

        </div>

      </div>

      <div class="line5">
        <el-pagination
          @size-change="handleSizeChange"
          @current-change="handleCurrentChange"
          :current-page="pageNo"
          :page-sizes="[10, 20, 30, 40]"
          :page-size="pageSize"
          prev-text="上一页"
          next-text="下一页"
          layout="total, sizes, prev, pager, next, jumper"
          :total="total">
        </el-pagination>
      </div>

    </div>
</template>

<script>
    import tobbar from "../../components/person/tobbar";
    import searchIc from '../../assets/img/personal/search.png'
    import deleteIc from '../../assets/img/personal/delete.png'
    import successIc from '../../assets/img/success.png'
    export default {
        name: "allShip",
        mounted(){
          this.getList()
        },
        components:{
            tobbar,
        },
        methods:{

            async getList(){
                let qry=this.query.new();
                this.query.toP(qry,this.pageNo,this.pageSize)
                let obj=  await this.api.msgList(this.query.toEncode(qry))
                this.list=obj.data.list
                this.total=obj.page.total
            },
            handleSizeChange(val) {
                this.pageSize=val
                this.pageNo=1
                this.getList()
                console.log(`每页 ${val} 条`);
            },
            handleCurrentChange(val) {
                console.log(`当前页: ${val}`);
                this.pageNo=val
                this.getList()
            },
            //跳转详情
            toDetail(item){
                this.$router.push({
                    path: '/personal/msgDtl',
                    query:{
                        id:item.id
                    }
                })
            }
        },
        data(){
            return{
                pageNo:1,
                pageSize:10,
                total:0,//
                title:'我的消息',
                searchIc,
                deleteIc,
                successIc,
                value: '',
                list:[]
            }
        },
        layout:'person',
    }
</script>

<style scoped lang="less">
  @import url("../../assets/css/init.less");
  .main{
    width: 100%;


    .line4{

      div{
        height: 100px;
        box-sizing: border-box;
        border-bottom: 1px solid #DFDFDF;
        padding: 18px 31px;
        color: #333333;
        font-size: 15px;
        position: relative;
        >span{
          display: block;
          position: absolute;
          left: 18px;
          top:30px;
          width: 5px;
          height: 5px;
          background: @redColor;
          border-radius: 50%;
        }
        &:hover{
          cursor: pointer;

        }
        p:nth-of-type(1){
          height: 50%;
          display: flex;
          align-items: center;
        }
        p:nth-of-type(2){
          display: flex;
          align-items: center;
          height: 50%;
          color: #999999;
        }


      }


    }
    .line5{
      padding: 30px 40px;
      width: 100%;
      display: flex;
      justify-content: flex-end;
    }
  }




</style>
