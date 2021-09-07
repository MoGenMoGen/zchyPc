<template>
  <div class="overall">
    <!-- 步骤条盒子 -->
    <div class="steps-box">
      <!-- 步骤条 -->
      <div class="Article-steps" :class="stepList.length-1 <= activeIndex ? 'step-over':''">
        <!-- 每步部分 -->
        <div class="step" v-for="(i,index) in stepList" :key="index" :class="i.stepIndex<=activeIndex ? 'step-active':''">
          <p class="step-num">
            <img :src="i.imgA" v-if="i.stepIndex<=activeIndex">
            <img :src="i.img" v-else>
          </p>
          <p class="title">
            <span>{{i.title}}</span>
            <span>{{i.date}}</span>
            <span>{{i.time}}</span>
          </p>
          <img class="arrow" :src="a11" v-if="i.stepIndex<=activeIndex && i.stepIndex!=stepList.length-1">
          <img class="arrow" :src="a1" v-if="i.stepIndex>activeIndex && i.stepIndex!=stepList.length-1">
        </div>
      </div>
      <!-- 步骤条对应内容 -->
      <div class="Article-content">
        <div>{{stepList[activeIndex].title}}</div>
        <span class="btn" v-if="activeIndex != stepList.length-1" @click="nextStep()">下一步</span>
        <span class="btn" v-if="activeIndex != 0" @click="lastStep()">上一步</span>
      </div>
    </div>
  </div>
</template>

<script>
  import p1 from './img/step1.png'
  import p2 from './img/step2.png'
  import p3 from './img/step3.png'
  import p4 from './img/step4.png'
  import p11 from './img/step11.png'
  import p22 from './img/step22.png'
  import p33 from './img/step33.png'
  import p44 from './img/step44.png'
  import a1 from './img/arrow1.png'
  import a11 from './img/arrow11.png'

  export default {
    props: ['info'],
    data() {
      return {
        a1,
        a11,
        //当前位置
        activeIndex: 0,
        //步骤条步数
        stepList: [
          {
            stepIndex: 0,
            title: "提交订单",
            img: p1,
            imgA: p11,
            date: '2020-06-18',
            time: '12:00:50',
          },
          {
            stepIndex: 1,
            title: "付款成功",
            img: p2,
            imgA: p22,
            date: '2020-06-18',
            time: '12:00:50',
          },
          {
            stepIndex: 2,
            title: "待收货",
            img: p3,
            imgA: p33,
            date: '2020-06-18',
            time: '12:00:50',
          },

          {
            stepIndex: 3,
            title: "交易完成",
            img: p4,
            imgA: p44,
            date: '2020-06-18',
            time: '12:00:50',
          }
        ]
      };
    },
    mounted(){
      console.log("sssss")
      console.log(this.info)
    },
    methods: {
      //点击下一步
      nextStep() {
        this.activeIndex += 1;
      },
      //点击上一步
      lastStep() {
        this.activeIndex -= 1;
      }
    }
  };
</script>

<style lang="less" scoped>
  .steps-box {
    /*
文本不能被选择
*/
    user-select: none;
    /*pc端宽度可以写成固定值，移动端宽度100%*/
    width: 100%;
    height: 400px;
    position: relative;

    .Article-steps {
      display: flex;
      /*justify-content: space-between;*/

      .step {
        flex: 1;
        position: relative;
        /*图标外部框框和标题宽度一样*/

        .step-num {
          width: 80px;

          img {
            display: block;
            margin: 0 auto;
            width: 24px;
            height: 24px;
          }
        }

        /*步骤条标题*/

        .title {
          width: 80px;
          font-size: 14px;

          color: #999999;
          margin-top: 5px;
          text-align: center;

          span {
            white-space: nowrap;
          }
        }

        .arrow {
          position: absolute;
          top: 8px;
          left: 60px;
          width: 80%;

        }
      }
    }

    //已经激活样式
    .step-active {
      .step-num {

      }

      .title {
        color: #333333 !important;
      }
    }

    //全部完成样式
    .step-over {

      /*三角形*/

      .step-num {
        /*background: #67c23a !important;*/
      }

      /*步骤条标题*/

      .title {
        /*color: #67c23a !important;*/
      }
    }

    //对应内容和按钮
    .Article-content {
      display: none;
      padding: 20px;

      .btn {
        width: 150px;
        display: block;
        margin: 0 auto;
        margin-bottom: 10px;
        background: #2d7df5;
        color: white;
        padding: 10px;
        border-radius: 5px;
        cursor: pointer;
      }

    }
  }
</style>
