<template>
<!--  消息详情-->
    <div class="main">
      <div style="padding:0 30px">
        <tobbar :title="title" :showAll="false"></tobbar>
      </div>
      <p style="border-bottom: 1px solid rgba(0,0,0,0.1);"></p>

      <div class="line4">
        <calendar class="cal" ref="sign"></calendar>
        <div class="line4-btn">
          <p class="btn"  v-if="hasSign">今日已签到</p>
          <p class="btn noSign" @click="toSign" v-else>签到</p>
          <p class="signDay">本月已签到<span style="color: #E4393C"> {{signDay}}</span>天</p>
        </div>

      </div>

    </div>
</template>

<script>
    import tobbar from "../../components/person/tobbar";
    import searchIc from '../../assets/img/personal/search.png'
    import deleteIc from '../../assets/img/personal/delete.png'
    import successIc from '../../assets/img/success.png'
    import calendar from "../../components/person/calendar";
    export default {
        name: "allShip",
        components:{
            tobbar,
            calendar,
        },
        mounted(){
            // this.getList();
            this.isSign()
            this.getSignDay()
        },
        methods:{
            //签到分页列表
            async getList(){
                let qry=this.query.new();
                this.query.toP(qry,1,31)
                this.query.toW(qry,"crtTm","[2020-07-01,2020-07-31]","BT")
                this.query.toO(qry,'crtTm',"asc")
                let res= await this.api.signList(this.query.toEncode(qry))
            },
            //签到接口
            toSign(){
                this.api.signIn().then(res=>{
                    this.$refs.sign.updData()
                })
            },
        //    是否签到
            async  isSign(){
                let date=new Date()

                let qry=this.query.new();
                this.query.toP(qry,1,1)
                this.query.toW(qry,"crtTm",this.dateFormat(date),"LK")
                this.query.toO(qry,'crtTm',"asc")
                let res= await this.api.signList(this.query.toEncode(qry))

                if(res.list.length>0){
                    this.hasSign=true

                }else{
                    this.hasSign=false
                }
            },
            //格式化日期
            dateFormat(date) {
                if(date instanceof  Date){
                    var year=date.getFullYear();
                    /* 在日期格式中，月份是从0开始的，因此要加0
                     * 使用三元表达式在小于10的前面加0，以达到格式统一  如 09:11:05
                     * */
                    var month= date.getMonth()+1<10 ? "0"+(date.getMonth()+1) : date.getMonth()+1;
                    var day=date.getDate()<10 ? "0"+date.getDate() : date.getDate();
                    var hours=date.getHours()<10 ? "0"+date.getHours() : date.getHours();
                    var minutes=date.getMinutes()<10 ? "0"+date.getMinutes() : date.getMinutes();
                    var seconds=date.getSeconds()<10 ? "0"+date.getSeconds() : date.getSeconds();
                    // 拼接
                    return year+"-"+month+"-"+day;
                }else{
                    return date
                }
            },

            dateFormat2(date) {
                if(date instanceof  Date){
                    var year=date.getFullYear();
                    /* 在日期格式中，月份是从0开始的，因此要加0
                     * 使用三元表达式在小于10的前面加0，以达到格式统一  如 09:11:05
                     * */
                    var month= date.getMonth()+1<10 ? "0"+(date.getMonth()+1) : date.getMonth()+1;
                    var day=date.getDate()<10 ? "0"+date.getDate() : date.getDate();
                    var hours=date.getHours()<10 ? "0"+date.getHours() : date.getHours();
                    var minutes=date.getMinutes()<10 ? "0"+date.getMinutes() : date.getMinutes();
                    var seconds=date.getSeconds()<10 ? "0"+date.getSeconds() : date.getSeconds();
                    // 拼接
                    return year+"-"+month;
                }else{
                    return date
                }
            },

        //    获取当月签到天数
            async getSignDay(){
                let date=new Date()
                let qry=this.query.new();
                this.query.toP(qry,1,31)
                this.query.toW(qry,"crtTm",this.dateFormat2(date),"LK")
                this.query.toO(qry,'crtTm',"asc")
                let res= await this.api.signList(this.query.toEncode(qry))
                this.signDay=res.list.length
            }


        },
        data(){
            return{
                title:'每日签到',
                searchIc,
                deleteIc,
                successIc,
                value: '',
                list:[],
                hasSign:false,//是否签到
                signDay:0,//当月签到天数


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
      position: relative;
      margin-top: 46px;
      .cal{
        margin: 0 auto;
      }
      .line4-btn{
        position: absolute;
        left: 50%;
        transform: translateX(-50%);
        top:70%;

        /*签到按钮*/
        .btn{
          width:153px;
          height:49px;
          background:rgba(153,153,153,1);
          border-radius:5px;
          display: flex;
          align-items: center;
          justify-content: center;
          color: white;
          font-size: 18px;
          margin: 25px auto;
          &:hover{
            cursor: pointer;
          }
          &:active{
            opacity: 0.5;
          }
        }
        .noSign{
          background: @themeColor;
          color: #ffffff;
        }


        /*签到天数*/
        .signDay{
          text-align: center;
          font-size:18px;
          font-family:PingFang SC;
          /*font-weight:500;*/
          color:rgba(102,102,102,1);
        }
      }


    }
  }




</style>
