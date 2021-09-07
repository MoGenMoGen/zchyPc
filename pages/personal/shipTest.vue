<template>
    <!--船舶检测-->
    <div class="main">
      <div style="padding:0 30px">
        <tobbar :title="title" :show-all="false"></tobbar>

      </div>
      <p style="border-bottom: 1px solid rgba(0,0,0,0.1);"></p>
      <div class="line2">
        <div class="slt">
          <p>客户名称：</p>
          <el-select v-model="value" placeholder="请选择" clearable>
            <el-option

              v-for="item in options"
              :key="item.value"
              :label="item.label"
              :value="item.value">
            </el-option>
          </el-select>

        </div>

        <div class="slt">
          <p>完成时间：</p>
          <el-select v-model="value" placeholder="请选择" clearable>
            <el-option
              v-for="item in options"
              :key="item.value"
              :label="item.label"
              :value="item.value">
            </el-option>
          </el-select>

        </div>
        <div class="search">
          <p>
            <input placeholder="搜索船舶名称、船舶编号" >
          </p>

          <p><img :src="searchIc"></p>
        </div>
      </div>
      <div class="line3">
        <p>
          船舶编号
        </p>
        <p>
          船舶名称
        </p>
        <p>
          客户名称
        </p>
        <p>
          船舶状态
        </p>
        <p>
          完成时间
        </p>
        <p>
          操作
        </p>
      </div>
      <div class="line4">
        <div v-for="item in list" :key="item.id">
          <p>{{item.id}}</p>
          <p>{{item.nm}}</p>
          <p>{{item.price}}</p>
          <p>{{item.status}}</p>
          <p>{{item.time}}</p>
          <p>
            <span v-if="item.status=='已交船'" class="sail" @click="showPop(item)">新增</span>
            <span class="detail">查看详情</span>

          </p>

        </div>

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
        components:{
            tobbar,
        },
        methods:{

        },
        data(){
            return{
                searchIc,
                deleteIc,
                successIc,
                dialogVisible:false,
                toastVisible:false,
                title:'船舶检测',

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
                value: '',
                list:[
                    {
                        id:'12345689525522',
                        nm:'新型4000kw中型休闲渔船',
                        price:'1320000',
                        status:'设计中',
                        time:'2020-11-11',
                    },
                    {
                        id:'12345689325522',
                        nm:'新型4000kw中型休闲渔船',
                        price:'1320000',
                        status:'设计中',
                        time:'2020-11-11',
                    },
                    {
                        id:'1234489525522',
                        nm:'新型4000kw中型休闲渔船',
                        price:'1320000',
                        status:'设计中',
                        time:'2020-11-11',
                    },
                    {
                        id:'12345589525522',
                        nm:'新型4000kw中型休闲渔船',
                        price:'1320000',
                        status:'已交船',
                        time:'2020-11-11',
                    },
                ]

            }
        },
        layout:'person',

    }
</script>

<style scoped lang="less">
  .main{
    width: 100%;
    .line2{
      width: 100%;
      display: flex;
      align-items: center;
      height: 78px;
      padding-left: 29px;
      padding-right: 16px;
      div{
        flex:1;
        display: flex;
        align-items: center;
      }
      .slt{
        margin-right: 30px;
        p{
          margin-right: 10px;
        }
      }
      .search{
        display: flex;
        align-items: center;
        p:nth-of-type(1){
          border-top: 1px solid #DFDFDF;
          border-bottom: 1px solid #DFDFDF;
          border-left: 1px solid #DFDFDF;
          border-top-left-radius: 5px;
          border-bottom-left-radius: 5px;
          height: 38px;
          padding: 0 12px;
          align-items: center;
          display: flex;
          input{
            border: none;
            outline: none;
            color: #666666;
          }
          ::-webkit-input-placeholder { /* Chrome/Opera/Safari */
            color: #999999;
          }
          ::-moz-placeholder { /* Firefox 19+ */
            color: #999999;
          }
          :-ms-input-placeholder { /* IE 10+ */
            color: #999999;
          }
          :-moz-placeholder { /* Firefox 18- */
            color: #999999;
          }
        }
        p:nth-of-type(2){
          background:#2778BE;
          height: 38px;
          display: flex;
          align-items: center;
          justify-content: center;
          width: 55px;
          border-top-right-radius: 5px;
          border-bottom-right-radius: 5px;
          &:hover{
            cursor: pointer;
          }
          img{
            width: 18px;
          }
        }

      }
    }
    .line3{
      display: flex;
      align-items: center;
      height: 60px;
      padding: 0 31px;
      background:#F7F7F7;
      p:nth-of-type(1){
        flex:1;
      }
      p:nth-of-type(2){
        flex:1.5;
      }
      p:nth-of-type(3){
        flex:1;
      }
      p:nth-of-type(4){
        flex:1;
      }
      p:nth-of-type(5){
        flex:1;
      }
      p:nth-of-type(6){
        flex:1;
        text-align: center;
      }

    }
    .line4{

      div{
        display: flex;
        height: 76px;
        align-items: center;
        border-bottom: 1px solid #DFDFDF;
        padding: 0 31px;
        color: #333333;
        font-size: 15px;
        p:nth-of-type(1){
          flex:1;
        }
        p:nth-of-type(2){
          flex:1.5;
        }
        p:nth-of-type(3){
          flex:1;
          color: #E4393C;
        }
        p:nth-of-type(4){
          flex:1;
        }
        p:nth-of-type(5){
          flex:1;
        }
        p:nth-of-type(6){
          flex:1;
          display: flex;
          justify-content: center;
          flex-direction: column;
          span:hover{
            cursor: pointer;
          }
          .detail{
            margin: 0 auto;
            display: block;
            color: #2778BE;
          }
          .sail{
            margin: 5px auto;
            display: flex;
            color: #FFFFFF;
            background:#2778BE;
            width: 83px;
            height: 28px;
            align-items: center;
            justify-content: center;
            border-radius: 3px;
          }

        }
      }


    }
  }





</style>
