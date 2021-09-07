<template>
    <!--全部船舶-->
    <div class="main">
      <div style="padding:0 30px">
        <tobbar :title="title" :show-all="false"></tobbar>

      </div>
      <p style="border-bottom: 1px solid rgba(0,0,0,0.1);"></p>
      <div class="line2" style="border-bottom: 1px solid rgba(0,0,0,0.1);">
        <div class="slt">
          <p>产品分类：</p>
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
            <input placeholder="搜索船舶名称" >
          </p>

          <p><img :src="searchIc"></p>
        </div>
      </div>
      <div class="equip">
        <ship-equip></ship-equip>
      </div>

    </div>
</template>

<script>
    import tobbar from "../../components/person/tobbar";
    import searchIc from '../../assets/img/personal/search.png'
    import shipEquip from '@/components/onlineTrading/shipEquip'
    export default {
        name: "allShip",
        components:{
            tobbar,
          shipEquip
        },
        methods:{
          toDetail(){
            this.$router.push('./archives')
          },

        },
        data(){
            return{
                searchIc,
                dialogVisible:false,
                toastVisible:false,
                title:'我的产品',

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
    .equip{
      padding: 0 30px;
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
            background:#E4393C;
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

  .pop{
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    background: rgba(0,0,0,0.1);
    .pop-cont{
      position: absolute;
      top: 50%;
      left: 50%;
      transform: translate(-50%,-50%);
      width:381px;
      height:249px;
      background:rgba(255,255,255,1);
      border-radius:8px;
      p:nth-of-type(1){
        width: 100%;
        height: 78px;
        display: flex;
        align-items: center;
        justify-content: center;
        position: relative;
        font-size:18px;
        font-family:Microsoft YaHei;
        font-weight:400;
        color:rgba(51,51,51,1);
        border-bottom: 1px solid #DFDFDF;
        img{
          position: absolute;
          width:20px;
          height:20px;
          top:20px;
          right: 20px;
          &:hover{
            cursor: pointer;
          }
        }
      }
      p:nth-of-type(2){
        font-size:20px;
        font-family:Microsoft YaHei;
        font-weight:400;
        color:rgba(51,51,51,1);
        line-height:18px;
        width: 100%;
        height: 78px;
        display: flex;
        align-items: center;
        justify-content: center;
      }
      p:nth-of-type(3){
        display: flex;
        align-items: center;
        justify-content: center;
        span{
          width:128px;
          height:42px;
          border:1px solid #CECECE;
          border-radius:5px;
          display: flex;
          align-items: center;
          justify-content: center;
          font-size:18px;
        }
        span:hover{
          cursor: pointer;
        }
        span:nth-of-type(1){
          margin-right: 22px;
        }
        span:nth-of-type(2){
          margin-left: 22px;
          color: white;
          background: #E4393C;
        }
      }
    }
  }



  .toast{
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    background: rgba(0,0,0,0.1);
    .toast-cont{
      position: absolute;
      top: 50%;
      left: 50%;
      transform: translate(-50%,-50%);
      width:271px;
      height:139px;
      background:rgba(255,255,255,1);
      border-radius:8px;
      display: flex;
      align-items: center;
      justify-content: center;
      p{
        font-size:20px;
        font-family:Microsoft YaHei;
        font-weight:400;
        display: flex;
        align-items: center;

        img{
          width:32px;
          height:34px;
          margin-right: 11px;
        }
      }
    }
  }



</style>
