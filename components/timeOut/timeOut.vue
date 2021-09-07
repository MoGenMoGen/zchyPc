<template>
  <span>
    {{remainTime | secondToDate}}
  </span>
</template>

<script>
    export default {
        props: {
            remainTime: {    // 倒计时间总秒数
                type: Number,
                default:0
            },

        },

        model:{
            //prop是父组件v-model绑定的值，event你可以理解为子组件对v-model绑定值赋值的绑定事件
            prop:'remainTime',
            event:'change'
        },
        mounted () {
            this.countDowm()
        },
        methods: {

            countDowm () {
                let this_=this
                var f= setInterval(()=>{
                     if(this_.remainTime>0){
                         //抛出事件
                         let tmp=this_.remainTime-1
                         this_.$emit('change',tmp)
                     }else{
                         //清除定时任务
                         clearInterval(f);
                         this_.$emit('finish')
                     }
                 },1000)
            },

        },
        filters: {
            secondToDate(time){
                if (null != time && "" != time) {
                    if (time > 60 && time < 60 * 60) {
                        time = parseInt(time / 60.0) + "分" + parseInt((parseFloat(time / 60.0) -
                            parseInt(time / 60.0)) * 60) + "秒";
                    }
                    else if (time >= 60 * 60 && time < 60 * 60 * 24) {
                        time = parseInt(time / 3600.0) + "时" + parseInt((parseFloat(time / 3600.0) -
                            parseInt(time / 3600.0)) * 60) + "分" +
                            parseInt((parseFloat((parseFloat(time / 3600.0) - parseInt(time / 3600.0)) * 60) -
                                parseInt((parseFloat(time / 3600.0) - parseInt(time / 3600.0)) * 60)) * 60) + "秒";
                    } else if (time >= 60 * 60 * 24) {
                        time = parseInt(time / 3600.0/24) + "天" +parseInt((parseFloat(time / 3600.0/24)-
                            parseInt(time / 3600.0/24))*24) + "时" + parseInt((parseFloat(time / 3600.0) -
                            parseInt(time / 3600.0)) * 60) + "钟" +
                            parseInt((parseFloat((parseFloat(time / 3600.0) - parseInt(time / 3600.0)) * 60) -
                                parseInt((parseFloat(time / 3600.0) - parseInt(time / 3600.0)) * 60)) * 60) + "秒";
                    }
                    else {
                        time = parseInt(time) + "秒";
                    }
                }
                return time;
            },

        }
    }
</script>
<style scoped lang="less">
  span{
    color:#E64347 ;
  }
</style>
