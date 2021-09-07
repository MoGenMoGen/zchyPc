<template>
    <div class="service">
        <div class="title">售后保障</div>
        <ul>
          <li v-for="(item,index) in list" :key="index">
            <img :src="item.img"/>
            <div>
              <b>{{item.nm}}</b>
              <p v-html="item.cont"></p>
            </div>
          </li>
        </ul>
        <div class="cont">
          <p class="red">权利声明：</p>
          <p>中创上的所有商品信息、客户评价、商品咨询、网友讨论等内容，是中创重要的经营资源，未经许可，禁止非法转载使用。 </p>
           <p> <b>注：</b>本站商品信息均来自于合作方，其真实性、准确性和合法性由信息拥有者（合作方）负责。本站不提供任何保证，并不承担任何法律责任。</p>
        </div>
      <div class="cont">
        <p class="red">价格说明：</p>
        <p>中创上的所有商品信息、客户评价、商品咨询、网友讨论等内容，是中创重要的经营资源，未经许可，禁止非法转载使用。 <br/>
          <b>中创价：</b>本站商品信息均来自于合作方，其真实性、准确性和合法性由信息拥有者（合作方）负责。本站不提供任何保证，并不承担任何法律责任。
        </p>
      </div>


    </div>
</template>

<script>
  import {mapState} from "vuex";
  import service1 from '../img/service1.png'
  import service2 from '../img/service2.png'
  import service3 from '../img/service3.png'
    export default {
        name: "productList",
      data(){
          return{
            list:[{
              nm:'卖家服务',
              cont:'',
              img:service1
            },{
              nm:'中创承诺',
              cont:'中创平台卖家展示销售并由中创发货的商品，由平台卖家提供相应的售后服务。请您放心购买！<br/>注：因厂家会在没有任何提前通知的情况下更改产品包装、产地或者一些附件，本司不能确保客户收到的货物与商城图片、产地、附件说明完全一致。只能确保为原厂正货！并且保证与当时市场上同样主流新品一致。若本商城没有及时更新，请大家谅解！',
              img:service2
            },{
              nm:'正品行货',
              cont:'中创商城向您保证所售商品均为正品行货，中创自营商品开具机打发票或电子发票。',
              img:service3
            }],
          }
      },
      computed:{
        ...mapState([
          'bWidth',
          'width'
        ]),

      },
      methods:{

      }
    }
</script>

<style lang="less" scoped>
  @import url("../../assets/css/init.less");

    .service{
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
      ul{
        width: 100%;
        padding: 0 27px;
        box-sizing: border-box;
        margin-bottom: 50px;
        li{
          width: 100%;
          display: flex;
          display: -webkit-flex;
          align-items: flex-start;
          padding-bottom: 30px;
          img{
            width: 38px;
            height: auto;
            margin-right: 14px;
            flex-shrink: 0;
          }
          div{
            line-height: 22px;
            font-size: 12px;
            width:100%;
            flex: 1;
            b{
              font-size: 14px;
              display: block;
              color: @redColor;
              line-height: 38px;
            }
          }

        }
      }
      .cont{
        padding: 0 27px;
        margin-bottom: 50px;
        font-size: 12px;
        line-height: 22px;
      }
    }


</style>
