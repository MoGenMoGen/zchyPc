<template>
  <div class="main">
    <div style="padding:0 30px">
      <tobbar :title="title" :showAll="false"></tobbar>
    </div>
    <p style="border-bottom: 1px solid rgba(0,0,0,0.1);"></p>
    <div class="body" v-if="nextShow==false">
      <div class="head">
        <span>新增整改单</span>
        <div class="line">
        </div>
      </div>
      <div class="list">
        <div class="" style="display: flex;">
          <div class="" style="width: 48%;">
            <div class="listContent">
              <div class="listLeft">
                <span>*</span>
                整改名称:
              </div>
              <div class="listRight">
                <input type="" name="" id="" value="" placeholder="请输入整改名称" />
              </div>
            </div>
            <div class="listContent">
              <div class="listLeft">
                <span>*</span>
                整改单位:
              </div>
              <div class="listRight">
                <el-select v-model="value" clearable filterable placeholder="请选择整改单位">
                  <el-option v-for="item in options" :key="item.value" :label="item.label" :value="item.value">
                  </el-option>
                </el-select>
              </div>
            </div>
            <div class="listContent">
              <div class="listLeft">
                <span>*</span>
                检查区域:
              </div>
              <div class="listRight">
                <input type="" name="" id="" value="" placeholder="请输入检查区域" />
              </div>
            </div>
            <div class="listContent">
              <div class="listLeft">
                <span>*</span>
                责任整改人:
              </div>
              <div class="listRight">
                <el-select v-model="value2" clearable filterable placeholder="请选择整改人">
                  <el-option v-for="item in optionsTwo" :key="item.value" :label="item.label" :value="item.value">
                  </el-option>
                </el-select>
              </div>
            </div>
            <div class="listContent">
              <div class="listLeft">
                <span>*</span>
                整改期限:
              </div>
              <div class="listRight">
                <el-date-picker v-model="pickTime" type="date" placeholder="选择日期">
                </el-date-picker>
              </div>
            </div>
          </div>
          <div class=""  style="width: 48%;">
            <div class="listContent" style="align-items: flex-start;">
              <div class="listLeft" style="line-height: 40px;">
                <span>*</span>
                隐患说明:
              </div>
              <div class="listRight">
                <textarea rows="" cols="" placeholder="请输入隐患说明"></textarea>
              </div>
            </div>
            <div class="listContent" style="align-items: flex-start;">
              <div class="listLeft"  style="line-height: 40px;">
                <span>*</span>
                整改要求:
              </div>
              <div class="listRight">
                <textarea rows="" cols="" placeholder="请输入整改要求"></textarea>
              </div>
            </div>
          </div>
        </div>
        <div class="listContent">
          <div class="listLeft">
            <span>*</span>
            隐患图片:
          </div>
          <div class="listRight">
            <el-upload action="https://jsonplaceholder.typicode.com/posts/" list-type="picture-card"
              :on-preview="handlePictureCardPreview" :on-remove="handleRemove">
              <i class="el-icon-plus"></i>
            </el-upload>
            <el-dialog :visible.sync="dialogVisible">
              <img width="100%" :src="dialogImageUrl" alt="">
            </el-dialog>
          </div>
        </div>
        <div class="btn">
          <button type="button" class="next" @click="nextTo">下一步</button>
          <button type="button" class="cancel">取消</button>
        </div>
      </div>
    </div>
    <div class="body" v-if="nextShow==true">
      <div class="head">
        <span>安全隐患整改通知单</span>
        <div class="line">
        </div>
      </div>
    </div>
  </div>

</template>

<script>
  import tobbar from "../../components/person/tobbar";
  import {
    mapState
  } from "vuex";
  export default {
    name: "allShip",
    components: {
      tobbar,
    },
    data() {
      return {
        title: '整改单',
        value: "",
        value2: "",
        pickTime: "",
        dialogImageUrl: '',
        nextShow: false,
        dialogVisible: false,
        options: [{
          value: '选项1',
          label: '黄金糕'
        }, {
          value: '选项2',
          label: '双皮奶'
        }, {
          value: '选项3',
          label: '蚵仔煎'
        }, {
          value: '选项4',
          label: '龙须面'
        }, {
          value: '选项5',
          label: '北京烤鸭'
        }],
        optionsTwo: [{
          value: '选项1',
          label: '傻杯'
        }, {
          value: '选项2',
          label: '250'
        }, {
          value: '选项3',
          label: '蚵仔煎'
        }, {
          value: '选项4',
          label: '龙须面'
        }, {
          value: '选项5',
          label: '北京烤鸭'
        }]
      }
    },
    layout: 'person',
    async mounted() {},
    computed: {
      ...mapState([
        'currentRole'
      ])
    },
    watch: {
    },
    methods: {
      handleRemove(file, fileList) {
        console.log(file, fileList);
      },
      handlePictureCardPreview(file) {
        this.dialogImageUrl = file.url;
        this.dialogVisible = true;
      },
      nextTo() {
        this.nextShow = true
      }
    },
  }
</script>

<style scoped lang="less">
  .main {
    width: 100%;
    .body {
      .head {
        padding: 30px;
        display: flex;
        align-items: center;
        span {
          font-size: 16px;
          color: #333333;
          font-weight: 400;
          position: relative;
        }
        .line {
          width: 578px;
          height: 1px;
          background: #000000;
          opacity: 0.1;
          margin-left: 10px;
        }
      }
      .list {
        padding-left: 54px;
        .listContent {
          margin-top: 20px;
          display: flex;
          align-items: center;
          .listLeft {
            font-size: 14px;
            font-weight: 400;
            width: 88px;
            span {
              color: #E4393C;
            }
          }
          .listRight {
            .el-select {
              width: 277px;
              font-size: 12px;
            }
            .el-select::placeholder {
              font-size: 12px;
              font-family: Microsoft YaHei;
              font-weight: 400;
              color: #999999;
            }
            .el-input {
              width: 277px;
              // padding: 10px;
              font-size: 12px;
            }
            /deep/.el-input--suffix .el-input__inner::placeholder {
              font-size: 12px;
              font-family: Microsoft YaHei;
              font-weight: 400;
              color: #999999;
            }
            input {
              width: 277px;
              height: 40px;
              background: #FFFFFF;
              border: 1px solid #DDDDDD;
              padding: 10px;
              font-size: 12px;
            }
            input::placeholder {
              font-size: 12px;
              font-family: Microsoft YaHei;
              font-weight: 400;
              color: #999999;
            }
            textarea {
              width: 277px;
              height: 92px;
              background: #FFFFFF;
              border: 1px solid #DDDDDD;
              padding: 10px;
              font-size: 12px;
              resize: none;
            }
            textarea::placeholder {
              font-size: 12px;
              font-family: Microsoft YaHei;
              font-weight: 400;
              color: #999999;
            }
          }
        }
        .btn {
          margin-left: 90px;
          margin-top: 40px;
          .next {
            width: 86px;
            height: 35px;
            background: #2778BE;
            border: 0;
            border-radius: 4px;
            font-size: 16px;
            color: #ffffff;
          }
          .cancel {
            width: 86px;
            height: 35px;
            background: #FFFFFF;
            border: 1px solid #DDDDDD;
            border-radius: 4px;
            font-size: 16px;
            color: #999999;
            margin-left: 30px;
          }
        }
      }
    }
  }
</style>
