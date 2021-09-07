<template>
    <div class="my-radio">
       <div class="item" v-for="item in list" >
           <div class="check-box">
               <img :src="checked" v-if="item.value==selected" @click="change(item)">
               <img :src="unchecked " v-else @click="change(item)">
               <span @click="change(item)">{{item.label}}</span>
           </div>

       </div>

    </div>
</template>

<script>
    import checked from './img/checked.png'
    import unchecked from './img/unchecked.png'
    export default {
        name: "myRadio",
        props:{
            selected:{
                type:String
            },
            list:{
                type:Array
            }

        },
        model:{
            //prop是父组件v-model绑定的值，event你可以理解为子组件对v-model绑定值赋值的绑定事件
            prop:'selected',
            event:'revert'
        },
        data(){
            return{
                checked,
                unchecked,
            }
        },
        methods:{
            change(item){
                if(item.value !== this.selected){
                    //this.selected = item.value;
                    //本来是不能对props过来的selected赋值的，现在可以赋值了，但是要加上下面那一句
                    this.$emit('revert',item.value)
                }
            }
        }

    }
</script>

<style scoped lang="less">
    //这里设置16px=1rem
    .my-radio{
        display: flex;
        align-items: center;

        .item{
            height: 54px;
            flex: 1;
            color: #333333;

            .check-box{
                display: flex;
                align-items: center;
                &:hover{
                  cursor: pointer;
                }
                img{
                    width: 24px;
                    height: 24px;
                    margin-right: 15px;

                }
                span{

                    white-space: nowrap;

                }
            }

        }
    }

</style>
