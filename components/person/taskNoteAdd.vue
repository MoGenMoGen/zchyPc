<template>
  <div class="addPop">
    <div class="pop-cont">
      <div style="height: 77px;">
        <i class="el-icon-circle-close" @click="close"></i>
        <div class="p-line1">
          维保服务单
        </div>
      </div>
      <div style="flex: 1; overflow-y: scroll;">
        <div class="p2">
            <p><span>联系人姓名：</span>
              <el-input placeholder="请输入联系人姓名" v-model="form.linkman" clearable></el-input>
            </p>
            <p><span>联系电话：</span>
              <el-input placeholder="请输入联系电话" v-model="form.mob" clearable></el-input>
            </p>
        </div>
        <div class="p2">
          <p><span>公司名称：</span>
            <el-input placeholder="请输入公司名称" v-model="form.company" clearable></el-input>
          </p>
          <p><span>服务地点：</span>
            <el-input placeholder="请输入服务地点" v-model="form.addr" clearable></el-input>
          </p>
        </div>
        <div class="p1">
          <p><span>维保船舶：</span>
            <el-select v-model="form.docsId"
                       filterable
                       remote
                       clearable
                       allow-create
                       @change="changeShip"
                       :remote-method="remoteMethod"
                       :loading="loading"
                       placeholder="请选择维保船舶">
              <el-option
                v-for="item in shipList"
                :key="item.id"
                :label="item.nm"
                :value="item.id">
                {{item.nm}}
              </el-option>
            </el-select>
          </p>

        </div>
        <!--<div class="p1">-->
          <!--<p>-->
            <!--<span>一级类别：</span>-->
            <!--<el-radio-group v-model="catId">-->
              <!--<el-radio :label="item.id" v-for="(item,index) in catList" :key="index">{{item.nm}}</el-radio>-->
            <!--</el-radio-group>-->
          <!--</p>-->
        <!--</div>-->
        <div class="p1">
          <p><span>产品名称：</span>
            <el-select v-model="deviceIds" multiple placeholder="请选择">
              <el-option
                v-for="item in proList"
                :key="item.id"
                :label="item.nm"
                :value="item.id">
              </el-option>
            </el-select>
          </p>

        </div>
        <div class="p1">
          <p><span>安装日期：</span>
            <el-date-picker
              v-model="form.installDt"
              type="date"
              value-format="yyyy-MM-dd"
              @change="installDtChange"
              :picker-options="pickerOptions"
              placeholder="选择安装日期">
            </el-date-picker>
          </p>
        </div>
        <div class="p1">
          <p>
            <span>服务内容：</span>
            <el-radio-group v-model="form.serviceItemCd">
              <el-radio :label="item.cd" v-for="(item,index) in serviceList" :key="index">{{item.nm}}</el-radio>
            </el-radio-group>
          </p>
        </div>
        <div class="p1">
          <p>
            <span>服务内容说明：</span>
            <el-input
              type="textarea"
              :autosize="{ minRows: 2, maxRows: 4}"
              placeholder="请输入内容"
              v-model="form.serviceCont"/>
          </p>
        </div>
        <div class="upload">
            <span>图片上传：</span>
            <div class="uploadImg">
              <div>
                <p>+</p>
                <input ref="upload" type="file" name="file"   multiple="multiple" @change="upImg($event)"/>
              </div>
            </div>
            <div v-for="(item,index) in imgList" class="imgList" :key="index">
              <img :src="del" class="delImg point" @click="deleteImg(index)"/>
              <img :src="item" @click="toLink(item)"/>
            </div>
        </div>
        <div class="p1">
          <p>
            <span>预约时间：</span>
            <el-date-picker
              style="margin-right: 45px"
              value-format="yyyy-MM-dd"
              v-model="form.appointmentTm"
              @change="appointmentTmChange"
              type="date"
              :picker-options="pickerOptions2"
              placeholder="选择预约时间">
            </el-date-picker>
            <el-radio-group v-model="form.dateSlice">
              <el-radio label="上午"></el-radio>
              <el-radio label="下午"></el-radio>
            </el-radio-group>
          </p>
        </div>
        <div class="btn">
           <p @click="close">取消</p>
           <p @click="add">提交</p>
        </div>
      </div>

    </div>
  </div>
</template>

<script>
  import del from '@/assets/img/personal/del.png'
    export default {
        name: "",
        props:['shipId','shipInfo'],
        data(){
            return{
              flag:0,
              info:{},
              del,
              shipList:[],
              proList:[],
              serviceList:[],
              loading:false,
              installDt:'',//安装时间
              appointmentTm:'',//预约时间
              deviceIds:[],
              form:{
                addr: "",
                installDt:'',
                company:'',
                appointmentTm: "",
                dateSlice: "上午",
                deviceIds: "",
                deviceNms: "",
                docsId: "",
                orgEnterId: "",
                evelStatus: 0,
                receiptsImg: "",
                linkman: "",
                mob: "",
                serviceCont: "",
                serviceItemCd: "",
                statusCd: "TASK_NOTE_STATUS.10"
              },
              imgList:[],
              value1:'',
              pickerOptions:{
                disabledDate(time) {
                  return time.getTime() > Date.now();
                },
              },
              pickerOptions2:{
                disabledDate(time) {
                  return time.getTime() < Date.now();
                },
              }
            }
        },
        watch:{
          'form.docsId'(){
             this.getProList()
          },
          shipId:{
            handler:function () {
               if(this.shipId){
                 this.flag = 1
                  this.getShipList()
               }
            },
            immediate:true
          }
        },
        mounted(){
          this.getInfo()
          this.getShipList('') //船舶
          // this.getCatId()  //分类
          this.getService() //服务
        },
        methods:{
          //安装时间变化
          installDtChange(){
            console.log(this.installDt)
          },
          //预约时间
          appointmentTmChange(){
            console.log(this.installDt)
          },
          //获取个人信息
          async getInfo(){
            this.info = await this.api.personInfo() //个人信息
            this.form.addr = this.info.addrNm+this.info.address
            this.form.company = this.info.company
            this.form.linkman = this.info.realNm
            this.form.mob = this.info.mob
          },
          //服务
          async getService(){
            this.serviceList = await this.api.dataDictionary('TASK_NOTE_SERVICE_ITEM')
          },
          //获取分类
          async getCatId(){
            this.catList = await this.api.shopClassify()
            this.catList = this.catList.filter(item=>item.id!='4942035725390848')
          },
          close(){
            this.$emit('close')
          },
          remoteMethod(query) {

            this.loading = true;
            this.getShipList(query)

          },
          //维保船舶更换
          changeShip(){
             this.getProList()
          },
          //获取产品列表
          async getProList(){
            let param = {
              docsId:this.form.docsId,
              // cid:this.catId
            }
            let qry = this.query.new()
            let data  = await this.api.fileDeviceList(this.query.toEncode(qry),param)
            this.proList = data.data.list
            this.deviceIds = []
            if(this.flag ==1 ){
              this.flag = 2
              console.log(this.shipInfo.id)
                this.deviceIds.push(this.shipInfo.id)
            }
          },
          //获取船舶列表
          async getShipList(key){
            let qry = this.query.new()
            this.query.toW(qry,'statusCd','DOCS_STATUS.03','EQ')
            this.query.toP(qry,1,50)
            let param = {
              orgEnterId:JSON.parse(this.until.seGet('currentRole')).id,
              keyWord:key
            }
            let data = await this.api.allShip(this.query.toEncode(qry),param)
            this.loading = false
            this.shipList = data.data.list
            if(this.flag==1){

              this.form.docsId = parseInt(this.shipId)

            }
            console.log(this.form.docsId)
            console.log(this.shipList)
          },
          //上传图片
          async upImg(e){
            console.log(e)
            if(e.target.files.length===0){
              return
            }
            let type=e.target.files[0].type
            if(type.indexOf('image')==-1){
              this.$message({
                message: '请上传图片',
                type: 'warning'
              });
            }else{
              console.log('上传图片')
              let img = await this.api.uploadImg(e)
              this.imgList.push(img)
            }
          },
          toLink(url){
            window.open(url)
          },
          //新增
          add(){
            this.form.orgEnterId = JSON.parse(this.until.seGet('currentRole')).id
            this.form.receiptsImg = this.imgList.join(',')
            if(!this.form.serviceItemCd){
              this.$message.error('请选择服务内容！');
              return
            }
            this.store.commit('changeLoading',true)
            let deviceNms = []
            this.form.deviceIds = this.deviceIds.join(',')
            this.deviceIds.forEach(item=>{
                let info = this.proList.find(v=>item==v.id)
                deviceNms.push(info.nm)
            })
            this.form.deviceNms = deviceNms.join(',')
            this.api.taskNoteAdd(this.form).then(()=>{
              this.$message({
                message: '提交成功',
                type: 'success'
              });
              this.$emit('confirm')
            })
          },
        }
    }
</script>

<style scoped lang="less">
  @import "../../assets/css/init.less";
.addPop{
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: rgba(0,0,0,0.1);
  z-index: 100;
  .pop-cont{
    position: absolute;
    display: flex;
    flex-direction: column;
    top: 50%;
    left: 50%;
    transform: translate(-50%,-50%);
    width:691px;
    height: 75%;
    background:rgba(255,255,255,1);
    border-radius:8px;
    .el-icon-circle-close{
      position: absolute;
      top:23px;
      right: 23px;
      font-size: 20px;
      color: #999999;
    }
    .p-line1{
      height: 77px;
      width: 100%;
      display: flex;
      align-items: center;
      justify-content: center;

      border-bottom: 1px solid #DFDFDF;
      font-family:Microsoft YaHei;
      font-weight:400;
      color:rgba(51,51,51,1);
      font-size: 18px;

    }
    .p2{
      width: 100%;
      display: flex;
      display: -webkit-flex;
      padding: 0 15px;
      >p>span{
        display: block;
        width: 100px;
        flex-shrink: 0;
      }
      p{
        flex: 1;
        display: flex;
        display: -webkit-flex;
        align-items: center;
        padding: 20px 10px 0;
      }
    }
    .p1{
      padding: 0 15px;
      >p>span{
        display: block;
        width: 100px;
        flex-shrink: 0;
      }
      >p{
        display: flex;
        display: -webkit-flex;
        align-items: center;
        padding: 20px 10px 0;
        /deep/.el-select{
          flex: 1;
        }
      }
    }
    .upload{
      overflow: hidden;
      padding: 20px 25px 10px;
      >span{
        display: block;
        width: 100px;
        float: left;
      }
      >div{
        width:154px;
        height: 136px;
        // width: 85px;
        margin-right: 10px;
        margin-bottom: 10px;
        float: left;
        position: relative;
        img{
          width: 100%;
          height: 100%;
        }
        .delImg{
          position: absolute;
          top: -7.5px;
          right: -7.5px;
          z-index: 2;
          width: 15px;
          height: 15px;
        }
      }
      .uploadImg{
        display: flex;
        display: -webkit-flex;
        align-items: center;
        // width: 85px;
        >p{
          color: #333333;
          font-size: 15px;
          float: left;
          line-height: 85px;
          margin-left: 3%;
        }
        >div{
          display: flex;
          display: -webkit-flex;
          color: #C6C6C6;
          // width: 85px;
          width: 100%;
          // height: 85px;
          height: 100%;
          p{
            // width: 85px;
            width: 100%;
            text-align: center;
            height: 100%;
            line-height: 136px;
            border: 1px dashed #C6C6C6;
            font-size: 50px;
          }
          input{
            width: 100%;
            height: 100%;
            opacity: 0;
            position: absolute;
            left: 0;
            top: 0;
          }
        }
      }
    }
    .btn{
      padding: 20px 25px 70px 125px;
      display: flex;
      display: -webkit-flex;
      p{
        height: 52px;
        line-height: 52px;
        text-align: center;
        cursor: pointer;
      }
      p:first-of-type{
        width: 179px;
        border: 1px solid #DDDDDD;
        margin-right: 20px;
      }
      p:last-of-type{
        width: 308px;
        background: @themeColor;
        border: 1px solid @themeColor;
        color: #ffffff;
      }
    }
  }
}
</style>
