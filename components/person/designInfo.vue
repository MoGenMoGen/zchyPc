<template>
<!--  设计院-设计档案-基本信息-->
  <div>
    <p class="title">设计院简介</p>
    <div class="info">
      <p style="font-size:16px;font-weight:bold;color: #333333">中创海洋设计总院</p>
      <p style="color: #666666;margin-top: 20px" ><span>优势设计:</span>游艇、公务船、客船、渔船</p>
      <p style="margin-top: 29px;width: 633px;">
        <span>ISO9001:</span>2000质量体系认证，实验手段齐全，配有200吨拉力试验台，有员工120名，技术力量雄厚， 高级工程师、工程师、高级技师、技师达30余人。、
      </p>
      <p style="margin-top: 29px"><span>公司地址:</span>浙江省宁波市北仑区天台山路206号 </p>
      <p style="margin-top: 4px"><span>联 系 人:</span>谢静怡 </p>
      <p style="margin-top: 4px"><span>联系电话:</span>13961417298</p>
      <p style="margin-top: 4px;margin-bottom: 20px"><span>联系邮箱:</span>cnczfrp@163.com</p>

      <div style="display: flex">
        <div class="list" >
          <p>
            <img class="icon" :src="yingyezhizhao">
            <span>企业营业执照</span>
          </p>
          <img class="pre" :src="test1">
        </div>

        <div class="list">
          <p>
            <img class="icon" :src="shenfenzheng">
            <span>设计资质证书</span>
          </p>
          <img class="pre" :src="test1">
        </div>

        <div class="list">
          <p>
            <img class="icon" :src="shenfenzheng">
            <span>法人身份证</span>
          </p>
          <img class="pre2" :src="test1">
          <img class="pre2" :src="test1">
        </div>

      </div>
    </div>
    <p class="title">项目基本信息</p>
      <div class="basicInfo">
        <p>项目名称：{{info.nm}}</p>
        <p>项目金额：{{info.nm}}</p>
        <p>项目起止日期：{{info.nm}}</p>
        <p>项目负责人：{{info.nm}}</p>
        <p>项目说明：{{info.rmks}}</p>
        <img :src="info.img" alt="">
      </div>
  </div>

</template>

<script>
   import  imgTest from '../../assets/img/car.png'
   import shenfenzheng from '../../assets/img/personal/shenfenzheng.png'
   import yingyezhizhao from '../../assets/img/personal/yingyezhizhao.png'
   import zizhizhengshu from '../../assets/img/personal/zizhizhengshu.png'
   import test1 from '../../assets/img/personal/test1.png'
    export default {
        name: "fangansheji",
        props:{
          title:{
              type:String,
              default:'设计'
          }
        },
        data(){
            return{
                imgTest,
                shenfenzheng,
                yingyezhizhao,
                zizhizhengshu,
                test1,
                list:[
                    {
                        id:1,
                        nm:'企业营业执照',
                        icon:yingyezhizhao,

                    },
                    {
                        id:3,
                        nm:'设计资质证书',
                        icon:zizhizhengshu,
                        img:imgTest,
                    },
                    {
                        id:2,
                        nm:'法人身份证',
                        icon:shenfenzheng,
                        img:imgTest,
                    },
                ],
                info:{nm:'中国南方船厂',
                rmks:'对方水电费是非得失师傅的说法水电费第三方发斯蒂芬斯蒂芬斯蒂芬发送到发送到发送到发大水方式发生大幅度发第三方士大夫递四方速递',
                img:test1}
            }
        },
        mounted() {
           console.log('设计院简介')
        },

    }
</script>

<style scoped lang="less">

  .title{
    width: 100%;
    height: 40px;
    background: #EEEEEE;
    color: #666666;
    padding-left: 28px;
    display: flex;
    align-items: center;
  }
  .basicInfo{
    padding: 21px 24px 40px 19px;
    >p{
      font-size:14px;
      color:rgba(102,102,102,1);
      margin-bottom: 30px;
    }
    >img{
      width: 66.9%;
      height: 420px;
      display: block;
      margin: 31px auto;
    }
  }
  .info{
    margin-top: 30px;
    color: #999999;
    .list{
      display: flex;
      align-items: flex-start;
      margin-bottom: 24px;
      color: #666666;
      flex:1;
      p{
        display: flex;
        align-items: center;
        .icon{
          margin-right: 11px;
          width:19px;
          height:21px;
        }
      }
      .pre{
        width:113px;
        height:72px;
        margin-left: 11px;
      }
      .pre2{
        margin-left: 11px;
        width:79px;
        height:51px;
      }
    }
  }
</style>
