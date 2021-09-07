<template>
  <div class="left1">
    <!--跟进记录-->
    <div class="record">
      <div class="title">
        <p><img src="@/assets/img/personal/首页/跟进记录.png" alt=""/>跟进记录<span class="more" @click="toPage('./follow')">查看更多<i class="el-icon-arrow-right"></i></span></p>
      </div>
      <div class="content2">
        <el-table
          :data="recordList"
          highlight-current-row
          style="width: 100%">
          <el-table-column
            prop="no"
            align="center"
            width="60"
            label="序号">
          </el-table-column>
          <el-table-column
            prop="nm"
            width="115"
            align="left"
            label="跟进客户">
          </el-table-column>
          <el-table-column
            width="70"
            prop="tel"
            align="center"
            label="联系人">
          </el-table-column>
          <el-table-column
            width="190"
            prop="dt"
            align="center"
            label="跟进内容">
          </el-table-column>
          <el-table-column
            width="90"
            prop="way"
            align="center"
            label="跟进方式">
          </el-table-column>
          <el-table-column
            fixed="right"
            prop="time"
            width="120"
            align="center"
            label="下次跟进时间">
          </el-table-column>
        </el-table>
      </div>
    </div>
    <!--客户档案-->
    <div class="document">
      <div class="title">
        <p><img src="@/assets/img/personal/首页/客户档案.png" alt=""/>客户档案<span class="more" @click="toPage('./profile')">查看更多<i class="el-icon-arrow-right"></i></span></p>
      </div>
      <div class="content2">
        <el-table
          :data="documentList2"
          style="width: 100%">
          <el-table-column
            prop="no"
            align="center"
            width="60"
            label="序号">
          </el-table-column>
          <!-- <el-table-column
            prop="nm"
            width="160"
            align="left"
            label="客户名称">
          </el-table-column> -->
          <el-table-column
            width="70"
            prop="pn"
            align="center"
            label="联系人">
          </el-table-column>
          <el-table-column
            width="160"
            prop="tel"
            align="left"
            label="联系电话">
          </el-table-column>
          <el-table-column
            fixed="right"
            prop="add"
            width="205"
            align="left"
            label="客户地址">
          </el-table-column>
        </el-table>
      </div>
    </div>
  </div>
</template>

<script>
  import {mapState} from "vuex";

  export default {
    name: "shipowner",
    components: {
     },
    data(){

      return {
        documentList2: [{
          no: '1',
          nm: '宁波聚联科技',
          pn: '张先生',
          tel: '123456789012',
          add: '浙江宁波镇海329创业社区',
        }, {
          no: '2',
          nm: '宁波聚联科技',
          pn: '张先生',
          tel: '123456789012',
          add: '浙江宁波镇海329创业社区',
        }, {
          no: '3',
          nm: '宁波聚联科技',
          pn: '张先生',
          tel: '123456789012',
          add: '浙江宁波镇海329创业社区',
        }],
        recordList: [{
          no: '1',
          nm: '宁波聚联科技',
          tel: '张先生',
          dt: '电话沟通问候客户……',
          way: '电话拜访',
          time: '2020-05-01 ',
        }, {
          no: '1',
          nm: '宁波聚联科技',
          tel: '张先生',
          dt: '电话沟通问候客户……',
          way: '电话拜访',
          time: '2020-05-01 ',
        }, {
          no: '1',
          nm: '宁波聚联科技',
          tel: '张先生',
          dt: '电话沟通问候客户……',
          way: '电话拜访',
          time: '2020-05-01 ',
        }, {
          no: '1',
          nm: '宁波聚联科技',
          tel: '张先生',
          dt: '电话沟通问候客户……',
          way: '电话拜访',
          time: '2020-05-01 ',
        }, {
          no: '1',
          nm: '宁波聚联科技',
          tel: '张先生',
          dt: '电话沟通问候客户……',
          way: '电话拜访',
          time: '2020-05-01 ',
        }, {
          no: '1',
          nm: '宁波聚联科技',
          tel: '张先生',
          dt: '电话沟通问候客户……',
          way: '电话拜访',
          time: '2020-05-01 ',
        }],
      }

    },
    computed:{
      ...mapState([
        'bWidth',
        'width',
        "currentRole",
        'loading',
        'tel',
        'msgNum'
      ])
    },
    watch:{

    },
    mounted(){

      // this.getData()
    },
    methods:{
      async getData(){

      },
      toPage(url){
        if(url){
          this.$router.push(url)
        }
      },

    },
  }
</script>

<style lang="less">


  .content2{
    //选中时更换字体 背景颜色   需设置高亮才能生效
    .el-table__body tr.current-row>td{
      background-color: #FFFFFF !important;
      color: #2778BE;
    }

    .el-table__header thead{
      color: #666666;
      font-size:14px;
    }
    .el-table__header th{
      background-color: #F7F7F7;
      height: 52px;
    }
    .el-table__body tr{
      color: #333333;
      font-size:15px;
      height: 77px;
    }
  }
  .message{
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
    textarea::-webkit-input-placeholder {
      /* Chrome/Opera/Safari */
      color: #999;
    }

    textarea::-moz-placeholder {
      /* Firefox 19+ */
      color: #999;
    }

    textarea:-ms-input-placeholder {
      /* IE 10+ */
      color: #999;
    }

    textarea:-moz-placeholder {
      /* Firefox 18- */
      color: #999;
    }
  }
    #resize{
      resize: none !important;
      height: 92px;
    }
    #btnBA{
      display: inline-flex;
      width: 100%;
      .el-form-item__content{
        display: inline-flex;
        justify-content: center;
        width: 100%;
      }
      #buttonA{
        width: 46%;
        height: 42px;
        border: 1px solid #2778BE;
        color: #FFFFFF;
        font-size: 16px;
        background-color: #2778BE;
        cursor: pointer;
        margin-left: 30px;
      }
      #buttonB{
        width: 46%;
        height: 42px;
        border: 1px solid #DDDDDD;
        color: #666666;
        font-size: 16px;
        background-color: #FFFFFF;
        cursor: pointer;
      }
    }

</style>
<style lang="less" scoped>
  @import url("../../assets/css/init.less");
  .left1{
    width: 67.3%;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    padding-top: 31px;

    /*经销商 客户档案*/
    .document{
      width: 100%;
      background-color: #FFFFFF;
      min-height: 328px;
    }
    .title{
      width: 100%;
      height: 56px;
      border-bottom: 1px solid #F3F3F3;
      .more{
        float: right;
        font-size: 12px;
        color: #666666;
        margin-right: 30px;
        cursor: pointer;
      }
      >p{
        font-size: 16px;
        color: #333333;
        line-height: 56px;
        margin-left: 20px;
      }
      img{
        padding-right: 10px;
        height: 19px;
        vertical-align: sub;
      }
    }
    /*跟进记录*/
    .record{
      width: 100%;
      background-color: #FFFFFF;
      min-height: 551px;
      margin-bottom: 20px;
    }

  }

</style>
