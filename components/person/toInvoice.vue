<template>
  <div class="afterSale">
    <div class="container">
      <div class="header">
          申请发票
        <img :src="closeImg" @click="close"/>
      </div>
      <el-scrollbar class="main">
        <el-form :model="ruleForm" :rules="rules" ref="ruleForm" label-width="120px" class="demo-ruleForm">
          <el-form-item label="发票类型： " prop="invoiceTypeCd">
            <el-radio-group v-model="ruleForm.invoiceTypeCd">
              <el-radio :label="item.cd" v-for="(item,index) in typeList" :key="index">{{item.nm}}</el-radio>
            </el-radio-group>
          </el-form-item>
          <el-form-item label="抬头类型： " prop="type">
            <el-radio-group v-model="ruleForm.type">
              <el-radio label="2">个人</el-radio>
              <el-radio label="1">企业</el-radio>
            </el-radio-group>
          </el-form-item>
          <el-form-item label="发票抬头：" prop="nm">
            <el-autocomplete
              clearable
              style="width: 100%"
              v-model="invoiceId"
              :fetch-suggestions="querySearchAsync"
              placeholder="请输入发票抬头"
              @select="handleSelect"
            ></el-autocomplete>
            <!--<el-select v-model="ruleForm.invoiceId"-->
                       <!--filterable-->
                       <!--remote-->
                       <!--clearable-->
                       <!--allow-create-->
                       <!--@change="changeCustom"-->
                       <!--:remote-method="remoteMethod"-->
                       <!--:loading="loading"-->
                       <!--placeholder="请选择发票抬头">-->
              <!--<el-option-->
                <!--v-for="item in custumList"-->
                <!--:key="item.id"-->
                <!--:label="item.nm"-->
                <!--:value="item.id">-->
                <!--{{item.nm }}({{item.cd}})-->
              <!--</el-option>-->
            <!--</el-select>-->
          </el-form-item>
          <el-form-item label="纳税识别号：" prop="cd" v-show="ruleForm.type==1">
            <el-input v-model="ruleForm.cd" placeholder="纳税识别号" clearable></el-input>
          </el-form-item>
          <el-form-item label="开户行账号：" prop="bankAccount">
            <el-input v-model="ruleForm.bankAccount" placeholder="请输入开户行账号" clearable></el-input>
          </el-form-item>
          <el-form-item label="地址电话：" prop="addrPhone">
            <el-input v-model="ruleForm.addrPhone" placeholder="请输入地址电话" clearable></el-input>
          </el-form-item>
          <el-form-item label="开票金额：" prop="invoiceAmt">
            <el-input v-model="ruleForm.invoiceAmt" type="number" placeholder="请输入开票金额" clearable></el-input>
          </el-form-item>
          <el-form-item label="商品明细： " prop="resource">
            {{invoiceProNm}}
          </el-form-item>

          <el-form-item label="发票寄送地址：" prop="invoiceAddr">
            <el-select v-model="ruleForm.invoiceAddr"
                       filterable
                       remote
                       allow-create
                       @change="changeAddr"
                       :remote-method="remoteMethod2"
                       :loading="loading"
                       placeholder="请选择发票寄送地址">
              <el-option
                v-for="item in addrList"
                :key="item.id"
                :label="item.addrDtl"
                :value="item.id">
                {{item.addrDtl}}({{item.receNm }}{{item.receMob}})
              </el-option>
            </el-select>
            <!--<el-input v-model="ruleForm.invoiceAddr" placeholder="请输入发票寄送地址" clearable></el-input>-->
          </el-form-item>
          <el-form-item label="联系人：" prop="linkman">
            <el-input v-model="ruleForm.linkman" placeholder="请输入联系人" clearable></el-input>
          </el-form-item>
          <el-form-item label="电话：" prop="phone">
            <el-input v-model="ruleForm.phone" placeholder="请输入联系人电话" clearable></el-input>
          </el-form-item>
          <el-form-item label="邮箱：" prop="email">
            <el-input v-model="ruleForm.email" placeholder="请输入邮箱" clearable></el-input>
          </el-form-item>

          <el-form-item label="备注：" prop="rmks">
            <el-input type="textarea" v-model="ruleForm.rmks" placeholder="请输入备注"></el-input>
          </el-form-item>
          <el-form-item>
            <el-button @click="submitForm('ruleForm')">取消</el-button>
            <el-button type="primary" @click="submitForm('ruleForm')">确认</el-button>
          </el-form-item>
        </el-form>

      </el-scrollbar>

    </div>
  </div>
</template>

<script>
    import closeImg from '@/assets/img/close.png'
    export default {
        name: "role",
      data(){
          return{
            closeImg,
            loading:false,
            typeList:[],
            custumList:[],
            restaurants: [],
            addrChoose:'',
            ruleForm:{
              orgEnterId: "",
              orderId: "",
              type: "1",
              invoiceTypeCd: "",
              nm: "",
              cd: "",
              addrPhone: "",
              bankAccount: "",
              invoiceAmt: "",
              linkman: "",
              phone: "",
              invoiceAddr: "",
              email: "",
              rmks: "",
              invoiceId:'',
            },
            invoiceId:'',
            addrList:[],
            rules: {
              id: [
                { required: true, message: '请填写或输入发票抬头', trigger: 'blur' }
              ],
              type: [
                { required: true, message: '请选择抬头类型', trigger: 'change' }
              ],
              invoiceTypeCd: [
                { required: true, message: '请选择发票类型', trigger: 'change' }
              ],

            }
          }
      },
      watch:{
        id(){
          this.ruleForm.orderId = this.id
        },
        "ruleForm.type":{
          handler:function(e){
            if(e==2){
              this.rules = {
                invoiceId: [
                  { required: true, message: '请填写或输入发票抬头', trigger: 'blur' }
                ],
                type: [
                  { required: true, message: '请选择抬头类型', trigger: 'change' }
                ],
                invoiceTypeCd: [
                  { required: true, message: '请选择发票类型', trigger: 'change' }
                ],
              }
            }else {
              this.rules = {
                invoiceId: [
                  { required: true, message: '请填写或输入发票抬头', trigger: 'blur' }
                ],
                type: [
                  { required: true, message: '请选择抬头类型', trigger: 'change' }
                ],
                invoiceTypeCd: [
                  { required: true, message: '请选择发票类型', trigger: 'change' }
                ],
                cd: [
                  { required: true, message: '请输入纳税识别号', trigger: 'blur' }
                ],
              }
            }
          },
          immediate:true
        }
      },
      props: ['id','invoiceProNm'],
      mounted(){
          this.ruleForm.orgEnterId = JSON.parse(this.until.seGet('currentRole')).id
          this.ruleForm.orderId = this.id
          this.getType()
          this.getCompany('')
          this.getAddrList('')
      },
      methods: {
        //更换客户公司
        changeCustom(id){

          let val = this.custumList.find(item=>item.id==id)
          if(val){
            this.ruleForm.nm = val.nm
            this.ruleForm.cd = val.cd
            this.ruleForm.addrPhone = val.addrPhone
            this.ruleForm.bankAccount = val.bankAccount
          }
        },
        handleSelect(item) {
          console.log(item);
          this.ruleForm.invoiceId = item.id
          this.ruleForm.nm = item.nm
          this.ruleForm.cd = item.cd
          this.ruleForm.addrPhone = item.addrPhone
          this.ruleForm.bankAccount = item.bankAccount
        },
        //更换发票地址
        changeAddr(id){

          let val = this.addrList.find(item=>item.id==id)
          if(val){
            this.ruleForm.linkman = val.receNm
            this.ruleForm.phone = val.receMob
          }

        },
        querySearchAsync(queryString, cb) {
          this.ruleForm.invoiceId = ''
          // console.log(queryString)
          // console.log(cb)
          let qry = this.query.new()
          this.query.toW(qry,'nm',queryString,'LK')
          this.api.invoiceCompany(this.query.toEncode(qry)).then(res=>{
            this.restaurants = res
            console.log(this.restaurants)
          })
          var restaurants = this.restaurants;
          // var results = queryString ? restaurants.filter(this.createStateFilter(queryString)) : restaurants;
          var results = []
          restaurants.forEach(item=>{
            item.value = item.nm+' '+item.cd
            results.push(item)
          })

          cb(results);
          // clearTimeout(this.timeout);
          // this.timeout = setTimeout(() => {
          //   cb(results);
          // }, 3000 * Math.random());
        },
        createStateFilter(queryString) {
          return (state) => {
            return (state.value.toLowerCase().indexOf(queryString.toLowerCase()) === 0);
          };
        },

        remoteMethod(query) {

            this.loading = true;
            this.getCompany(query)

        },
        remoteMethod2(query) {

            this.loading = true;
            this.getAddrList(query)

        },
          async getCompany(v){
            let qry = this.query.new()
            this.query.toW(qry,'nm',v,'LK')
            this.custumList = await this.api.invoiceCompany(this.query.toEncode(qry))
            this.loading = false
          },
          //发票类型
         async getType(){
             this.typeList = await this.api.dataDictionary('INVOICE_TYPE')
         },
          close(){
            this.$emit('close')
          },
        //地址列表
        async getAddrList(val){
          let qry = this.query.new()
          this.query.toW(qry,'receNm',val,'LK')
          this.query.toP(qry,1,100)
          this.addrList = await this.api.addrList(this.query.toEncode(qry))
          this.loading = false
        },
        // chooseAddr(){
        //   this.ruleForm.invoiceAddr = this.addrChoose
        // },
          submitForm(formName) {
            this.ruleForm.invoiceId = this.ruleForm.invoiceId ? this.ruleForm.invoiceId : this.invoiceId
            console.log(this.ruleForm.invoiceId)
            // return
            this.$refs[formName].validate((valid) => {
              if (valid) {
                this.store.commit('changeLoading',true)
                this.api.invoiceApply(this.ruleForm).then(()=>{
                  this.$message({
                    message: '申请成功',
                    type: 'success',
                    duration:'1500'
                  });
                  this.$emit('confirm')
                })

              } else {
                return false;
              }
            });
          },

      }
    }
</script>

<style lang="less" scoped>
  @import url("../../assets/css/init.less");
  .afterSale {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: rgba(0,0,0,.23);
    z-index: 10;
    display: flex;
    display: -webkit-flex;
  }
  .container {
    background: #fff;
    border-radius: 8px;
    margin: auto;
    max-height: 80%;
    padding-bottom: 20px;
    width: 571px;
    display: flex;
    display: -webkit-flex;
    flex-direction: column;
  }
  .header {
    height: 78px;
    font-size: 18px;
    color: #333333;
    width: 100%;
    line-height: 78px;
    text-align: center;
    border-bottom: 1px solid #DFDFDF;
    position: relative;
    img{
      position: absolute;
      width: 20px;
      right: 20px;
      top:20px;
      cursor: pointer;
    }
  }


  .main{
    flex: 1;
    overflow-y: auto;
    width: 100%;
    height: 100%;
    box-sizing: border-box;
    padding-left: 20px;
    .demo-ruleForm{
      padding-right: 40px;
      /deep/.el-select{
        width: 100%;
      }
      /deep/.el-button--primary{
         background: @themeColor;
         width: 251px;
        border: 0;
        border-radius: 0;
      }
      .addr{
        /deep/.el-radio{
          width: 100%;
          display: flex;
          display: -webkit-flex;
          align-items: center;
          line-height: 26px;
          padding-bottom: 20px;
          /deep/.el-radio__input{
            padding-top: 6px;
          }
        }
      }
    }
  }
</style>
