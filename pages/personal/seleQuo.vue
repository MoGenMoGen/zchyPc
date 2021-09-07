<template>
<!-- 选型报价-->
<div class="mainSele">
  <div class="popupBox"  v-if="engShow||editorShow||deleteShow||projectShow">
    <div class="engineer" v-if="engShow">
      <div class="head">
        <p>添加项目工程</p>
        <img :src="close" @click="engShow=false">
      </div>
      <div class="bodyer">
        <div>
          <span>项目工程名称:</span>
          <el-select v-model="projCatId">
            <el-option
              v-for="item in projectList"
              :key="item.id"
              :label="item.nm"
              :value="item.id">
            </el-option>
          </el-select>
          <!--<button class="point">新增</button>-->
        </div>
        <div>
          <span>项目名称:</span>
          <el-select v-model="currentInfo.skuId"
                     filterable
                     remote
                     allow-create
                     @change="changeProduct"
                     :remote-method="remoteMethod"
                     :loading="loading"
                     placeholder="请选择项目名称">
            <el-option
              v-for="item in productList"
              :key="item.skuId"
              :label="item.nm"
              :value="item.skuId">
              {{item.nm }}({{item.attrDesc}})
            </el-option>
          </el-select>
          <!--<button class="point">新增</button>-->
        </div>
        <div><span>规格型号:</span><input type="text" v-model="currentInfo.models" :disabled="disabled"></div>
        <div><span>数量:</span><input type="number" min="1" v-model="currentInfo.num"></div>
        <div><span>单价(元):</span><input type="number" min="1" v-model="currentInfo.unitPrice"></div>
        <div><span>单位:</span><input type="text"  v-model="currentInfo.unitNm"></div>
        <div><span></span>
        <div><button class="cancelBtn point" @click="engShow=false">取消</button>
        <button class="confirmBtn point" @click="engConfirm">确认</button></div></div>
      </div>
    </div>
    <div class="editor" v-if="editorShow">
      <div class="head">
        <p>{{projectNow}}</p>
        <img :src="close" @click="editorShow=false">
      </div>
      <div class="bodyer">

        <div>
          <span>项目名称:</span>
          <el-select v-model="currentInfo.skuId"
                     filterable
                     remote
                     allow-create
                     @change="changeProduct"
                     :remote-method="remoteMethod"
                     :loading="loading"
                     placeholder="请选择项目名称">
            <el-option
              v-for="item in productList"
              :key="item.skuId"
              :label="item.nm"
              :value="item.skuId">
              {{item.nm }}({{item.attrDesc}})
            </el-option>
          </el-select>
        </div>
        <div><span>规格型号:</span><input type="text" v-model="currentInfo.models" :disabled="disabled"></div>
        <div><span>数量:</span><input type="number" min="1" v-model="currentInfo.num"></div>
        <div><span>单价(元):</span><input type="number" min="1" v-model="currentInfo.unitPrice"></div>
        <div><span>单位:</span><input type="number" min="1" v-model="currentInfo.unitNm"></div>
        <div><span></span>
        <div><button class="cancelBtn point" @click="editorShow=false">取消</button>
        <button class="confirmBtn point" @click="editorConfirm">确认</button></div></div>
      </div>
    </div>
    <div class="delete" v-if="deleteShow" style="height: auto;">
      <div class="head">
        <p>删除</p>
        <img :src="close" @click="deleteShow=false">
      </div>
      <div class="bodyDel">
        <p>确认删除<span>“{{projectNow}}”</span>项目吗？</p>
        <div><button class="cancelBtn point" @click="deleteShow=false">取消</button>
        <button class="confirmBtn point" @click="deleteConfirm">确认</button></div>
      </div>
    </div>
    <div class="project" v-if="projectShow">
      <div class="head">
        <p>添加项目工程</p>
        <img :src="close" @click="projectShow=false">
      </div>
      <div class="bodyPro">
        <div class="firstBox"><span>项目工程名称:</span>{{projectNow}}</div>
        <div class="inputBox">
          <div>
            <span>项目名称:</span>
            <el-select v-model="currentInfo.skuId"
                       filterable
                       remote
                       allow-create
                       @change="changeProduct"
                       :remote-method="remoteMethod"
                       :loading="loading"
                       placeholder="请选择项目名称">
              <el-option
                v-for="item in productList"
                :key="item.skuId"
                :label="item.nm"
                :value="item.skuId">
                {{item.nm }}({{item.attrDesc}})
              </el-option>
            </el-select>
          </div>
          <div><span>规格型号:</span><input type="text" v-model="currentInfo.models" :disabled="disabled"></div>
          <div><span>数量:</span><input type="number" min="1" v-model="currentInfo.num"></div>
          <div><span>单价(元):</span><input type="number" min="1" v-model="currentInfo.unitPrice"></div>
        </div>
        <div class="firstBox"><span></span>
        <div><button class="cancelBtn point" @click="projectShow=false">取消</button>
        <button class="confirmBtn point" @click="projectConfirm">确认</button></div></div>
      </div>
    </div>
  </div>
  <div class="head">
    <p><span class="lineC"></span>选型报价</p>
    <span @click="back" class="point">< 返回</span>
  </div>
  <div class="mainBasic">
    <div class="title">
      <img :src="headLeft" alt=""><p>船舶基本参数</p><img :src="headRight" alt="">
    </div>
    <div class="msg">
      <div class="floatL"><span>报价单标题:</span><input type="text" v-model="form.nm" placeholder="报价单标题" :disabled="type=='detail'"></div>
      <div class="floatR"><span>船舶户籍:</span><input type="text" v-model="form.registry" placeholder="船籍" :disabled="type=='detail'"></div>
      <div class="floatL">
        <span>船舶类型:</span>
        <el-select v-model="form.typesCd" placeholder="请选择船舶类型" :disabled="type=='detail'">
          <el-option
            v-for="item in typeList"
            :key="item.cd"
            :label="item.nm"
            :value="item.cd">
          </el-option>
        </el-select>
      </div>
      <div class="floatR"><span>船舶型宽:</span><input type="text" v-model="form.width" placeholder="船舶型宽(m)" :disabled="type=='detail'"></div>
      <div class="floatL">
        <span>船舶航区:</span>
        <el-select v-model="form.sailingAreaCd" placeholder="请选择航区" :disabled="type=='detail'">
          <el-option
            v-for="item in areaList"
            :key="item.cd"
            :label="item.nm"
            :value="item.cd">
          </el-option>
        </el-select>
      </div>
      <div class="floatR"><span>船舶型深:</span><input type="text" v-model="form.deep" placeholder="船舶型深(m)" :disabled="type=='detail'"></div>
      <div class="floatL">
        <span>船舶材质:</span>
        <el-select v-model="form.hullMaterialCd" placeholder="请选择材质" :disabled="type=='detail'">
          <el-option
            v-for="item in materialList"
            :key="item.cd"
            :label="item.nm"
            :value="item.cd">
          </el-option>
        </el-select>
      </div>
      <div class="floatR"><span>船舶总吨位:</span><input type="text" v-model="form.tonnage" placeholder="船舶总吨位" :disabled="type=='detail'"></div>
      <div class="floatL">
        <span>船舶总长:</span>
        <input v-model="form.totalLen" placeholder="船舶总长(m)" type="number" :disabled="type=='detail'"/>
      </div>
      <div class="floatR"><span>船舶航速:</span><input type="text" v-model="form.speed" placeholder="船舶航速(kn)" :disabled="type=='detail'"></div>
      <div class="floatL"><span>公约船长:</span><input type="text" v-model="form.pactLen" placeholder="公约船长(m)" :disabled="type=='detail'"></div>
      <div class="floatR"><span>船舶定员:</span><input type="text" v-model="form.complement" placeholder="船舶定员(P)" :disabled="type=='detail'"></div>
    </div>
    <!--<div class="money">-->
      <!--<p class="detail"><span class="detail-title">船舶预算报价：</span><span class="price">¥ 132,000.00</span></p>-->
      <!--<p class="rmks">注:  该船初步方案参考造价约为人民币 {{money}} 万元；此价格不含船级社检验费用，制冷设备及其安装费用。</p>-->
    <!--</div>-->
  </div>

  <div class="mainDetail">
    <div class="title">
      <img :src="headLeft" alt=""><p>项目参数详情</p><img :src="headRight" alt="">
    </div>
    <div class="btnBox">
       <p class="btnAdd point" @click="addAll('engineer')"  v-if="type!=='detail'">添加项目工程</p>
    </div>
    <div class="msg">
      <div class="proBox" v-for="(item,index) in list" :key="index">
        <div class="proTitle">
          <p class="checkPro">
            <img :src="item.check ? checked : unchecked" @click="checkEng(item)"  v-if="type!=='detail'"><span>{{item.nm}}</span>
          </p>
          <p class="proPrice">￥{{item.amt}}</p>
        </div>
        <div class="tablePro">
          <el-table
           :data="item.projItem"
           style="width: 100%">
             <el-table-column
               type="index"
               align="center"
               width="70"
               label="序号">
             </el-table-column>
             <el-table-column
               prop="nm"
               width="180"
               label="项目名称">
             </el-table-column>
             <el-table-column
               width="200"
               prop="models"
               label="规格型号">
             </el-table-column>
             <el-table-column
               width="90"
               prop="unit"
               label="单位">
             </el-table-column>
             <el-table-column
               prop="num"
               width="100"
               align="center"
               label="数量">
             </el-table-column>
             <el-table-column
               prop="unitPrice"
               width="120"
               label="单价(元)">
             </el-table-column>
             <el-table-column
               prop="amt"
               width = "100"
               label="总价(万元)">
               <template slot-scope="scope">
                 {{scope.row.amt/10000}}
               </template>
             </el-table-column>
             <el-table-column
               fixed="right"
               align="center"
               width="120"
               prop="operations"
               label="操作"  v-if="type!=='detail'">
             <template slot-scope="scope">
               <div class="btnList">
                 <el-button type="text" size="small" style="color: #2778BE;" @click="addAll('editor',item,scope.$index)">编辑</el-button>
                 <span class="lineC"></span>
                 <el-button type="text" size="small" style="color: #999;" @click="addAll('delete',item,scope.$index)">删除</el-button>
               </div>
             </template>

             </el-table-column>
          </el-table>
        </div>
        <button class="addPro point" @click="addAll('project',item)"  v-if="type!=='detail'">添加项目</button>
      </div>
    </div>

    <div class="mainTotal">
      <div class="title">
        <img :src="headLeft" alt=""><p>建造材料和设备成本小计</p><img :src="headRight" alt="">
      </div>
      <div class="msg" style="height: 216px;margin-top: 50px;">
        <div class="floatL"><span>人工费用:</span><input type="number" @input="changeMoney" v-model="form.labourFee" placeholder="请输入" :disabled="type=='detail'">（元）</div>
        <div class="floatR"><span>企业管理费用5%:</span><input type="number" @input="changeMoney" v-model="form.manageFee" placeholder="请输入" :disabled="type=='detail'">（元）</div>
        <div class="floatL"><span>税票7%:</span><input type="number" @input="changeMoney" v-model="form.taxes" placeholder="请输入" :disabled="type=='detail'">（元）</div>
        <div class="floatR"><span>模具制作费用:</span><input type="number" @input="changeMoney" v-model="form.mouldFee" placeholder="请输入" :disabled="type=='detail'">（元）</div>
        <div class="floatL"><span>试航费用:</span><input type="number" @input="changeMoney" v-model="form.trialTripFee" placeholder="请输入" :disabled="type=='detail'">（元）</div>
        <div class="floatR"><span>船舶检验费:</span><input type="number" @input="changeMoney" v-model="form.surveyFee" placeholder="请输入" :disabled="type=='detail'">（元）</div>
      </div>
      <div class="money">
        <p class="detail"><span class="detail-title">船舶预算报价：</span><span class="price">¥ {{form.budget}}</span></p>
        <p class="rmks">注:  该船初步方案参考造价约为人民币 {{form.budget/10000}} 万元；此价格不含船级社检验费用，制冷设备及其安装费用。</p>
      </div>
    </div>
    <div class="mainPrice">
      <div class="title">
        <img :src="headLeft" alt=""><p>产品整船概算造价（元）</p><img :src="headRight" alt="">
      </div>
      <div class="estimatePrice">
        <p>设计、管理和监理费（造价3%)：</p>
        <div>
          <input type="text" placeholder="请输入" @input="changeMoney" v-model="form.designOtherFee" :disabled="type=='detail'">
          <button class="point" @click="toConfrim" v-if="type!=='detail'">确认提交</button>
        </div>
      </div>
    </div>
  </div>

</div>
</template>

<script>
    import {mapState} from "vuex";
    import close from "@/assets/img/close.png"
    import headRight from "@/assets/img/personal/右.png"
    import headLeft from "@/assets/img/personal/左.png"
    import unchecked from "@/assets/img/unchecked.png"
    import checked from "@/assets/img/checked.png"
    export default {
      layout:'person',
        name: "shipowner",
      components: {

      },
      data(){
          return{
            close,
            headRight,
            headLeft,
            unchecked,
            checked,
            type:'',

            loading:false,
            disabled:false,
            projCatId:'',
            currentProIndex:'',
            currentInfo:{
              goodsId:'',
              skuId:'',
            },//当前弹窗信息
            engShow:false,        //添加项目工程
            editorShow:false,     //编辑
            deleteShow:false,     //删除
            projectShow:false,    //添加项目
            typeList: [],//船舶类型
           areaList: [],//航区
            materialList: [],//船舶材质
            lengthList:[], //船舶总长
            productList:[],//产品商务列表
           money:132,
           tableData: [],
            projectNow:'模具使用费用',
            projectList:[],
            list:[],
            currentRole:{},
            form:{
              orgEnterId: "",
              nm: "",//报价单名称（选型名称）
              cid: "",//船舶分类id
              "cids": "",
              registry: "",//船舶户籍
              typesCd: "",//船舶类型
              sailingAreaCd: "",//船舶航区
              hullMaterialCd: "",//船舶材质
              totalLen: "",//船舶长度
              pactLen: "",//公约总长
              width: "",//船舶宽度
              deep: "",//船舶深度
              tonnage: "",//吨位
              speed: "",//速度
              complement: "",  //船舶定员
              labourFee: 0,//人工费用
              manageFee: 0,//企业管理费用
              taxes: 0,//税
              mouldFee: 0,//模具制作费用
              trialTripFee: 0,//试航费用
              surveyFee: 0,//船舶检验费
              designOtherFee: 0, //设计、管理和监理费
              "otherFee": 0,
              budget: 0,
              "rmks": "",
              "crtOrgId": "",
              "crtById": "",
              "tntId": "",
              "projectItem": [
                {
                  "id": "",
                  "billId": "",
                  "projCatId": "5007128051799040",
                  "nm": "船体项目工程",
                  "seq": "1",
                  "amt": 200,
                  "projItem": [
                    {
                      "id": "",
                      "billId": "",
                      "catStoreId": "5007128051799040",
                      "goodsId": 5020033265996800,
                      "nm": "最牛逼发电机",
                      "models": "功率：1万瓦，颜色：黑色",
                      "unit": "",
                      "num": 1,
                      "unitPrice": 200,
                      "amt": 200,
                      "rmks": "",
                      "crtOrgId": "",
                      "crtById": ""
                    }
                  ]
                }
              ]
            }
          }
      },
      computed:{
        ...mapState([
          'bWidth',
          'width'
        ])
      },
      mounted() {
        this.type = this.until.getQueryString('type')
        this.currentRole=JSON.parse(this.until.seGet('currentRole'))
        this.form.orgEnterId = this.currentRole.id
       if(this.type){
          this.getInfo()
        }
        this.getData()
      },
      watch:{
        $route(){

        },
        list:{
          deep:true,
          handler:function () {
            this.changeMoney()
          }
        }
      },
      methods: {
        //获取详情
        async getInfo(){
          this.form = await this.api.xuanxingDetail(this.until.getQueryString('id'))
          this.form.shipProjCatList.forEach((item,index)=>{
            item.check = true
            this.$set(this.form.shipProjCatList,index,item)
          })
          this.list = this.form.shipProjCatList

        },
        async getData(){
          this.typeList=await this.api.dataDictionary('SHIP_TYPES')
          this.materialList=await this.api.dataDictionary('HULL_MATERIAL')
          this.areaList=await this.api.dataDictionary('SAILING_AREA')
          this.lengthList=await this.api.dataDictionary('SHIP_LENGTH')
          this.projectList = await this.api.xuanxingProjectList()
          this.getProduct('')
        },
        //提交选型报价
        toConfrim(){
          if(!this.form.nm){
            this.$message({
              message: '请输入造型报价标题',
              type: 'warning'
            });
            return
          }
          if(!this.form.registry){
            this.$message({
              message: '请输入船舶户籍',
              type: 'warning'
            });
            return
          }
          if(!this.form.typesCd){
            this.$message({
              message: '请选择船舶类型',
              type: 'warning'
            });
            return
          }
          if(!this.form.sailingAreaCd){
            this.$message({
              message: '请选择船舶航区',
              type: 'warning'
            });
            return
          }
          if(!this.form.hullMaterialCd){
            this.$message({
              message: '请选择船舶材质',
              type: 'warning'
            });
            return
          }
          if(!this.form.totalLen){
            this.$message({
              message: '请输入船舶总长',
              type: 'warning'
            });
            return
          }
          if(!this.form.pactLen){
            this.$message({
              message: '请输入公约船长',
              type: 'warning'
            });
            return
          }

          this.form.projectItem = this.list.filter(item=>item.check)
          this.form.projectItem.forEach((item,index)=>{
              item.seq = index+1
              item.num = 1
          })
          this.store.commit('changeLoading',true)
          this.api.xuanxingAdd(this.form).then(()=>{
            this.$message({
              message: '提交成功',
              type: 'success'
            });
            if(this.type=='edit'){
              this.$router.back()
            }else {
              this.$router.push('./record')
            }
          })
        },
        //预算改变
        changeMoney(){
          let amt = this.list.filter(item=>item.check).reduce((p,n)=>{
            return p+n.amt
          },0)
          this.form.labourFee = this.form.labourFee ? parseFloat(this.form.labourFee) : 0
          this.form.manageFee = this.form.manageFee ? parseFloat(this.form.manageFee) : 0
          this.form.taxes = this.form.taxes ? parseFloat(this.form.taxes) : 0
          this.form.mouldFee = this.form.mouldFee ? parseFloat(this.form.mouldFee) : 0
          this.form.trialTripFee = this.form.trialTripFee ? parseFloat(this.form.trialTripFee) : 0
          this.form.surveyFee = this.form.surveyFee ? parseFloat(this.form.surveyFee) : 0
          this.form.designOtherFee = this.form.designOtherFee ? parseFloat(this.form.designOtherFee) : 0
          this.form.budget =
            parseFloat(this.form.designOtherFee)+
            parseFloat(this.form.labourFee)+
            parseFloat(this.form.manageFee)+
            parseFloat(this.form.taxes)+
            parseFloat(this.form.mouldFee)+
            parseFloat(this.form.trialTripFee)+
            parseFloat(this.form.surveyFee)+
            parseFloat(amt)
        },
        //获取设备列表
        async getProduct(val){
           let qry = this.query.new()
           this.query.toW(qry,'nm',val,'LK')
           this.productList = await this.api.xuanxingProductList(this.query.toEncode(qry))
           this.loading = false
        },
        //添加项目工程
        engConfirm(){
          if(!this.projCatId){
            this.$message({
              message: '请选择项目工程名称',
              type: 'warning'
            });
            return
          }
          if(!this.currentInfo.nm){
            this.$message({
              message: '请选择或输入项目名称',
              type: 'warning'
            });
            return
          }
          if(!this.currentInfo.models){
            this.$message({
              message: '请输入规格',
              type: 'warning'
            });
            return
          }
          if(!this.currentInfo.unitPrice){
            this.$message({
              message: '请输入单价',
              type: 'warning'
            });
            return
          }
          if(!this.currentInfo.num){
            this.$message({
              message: '请输入数量',
              type: 'warning'
            });
            return
          }
           let index = this.list.findIndex(item=>item.projCatId==this.projCatId)
          let item = {
            skuId:this.currentInfo.skuId,
            catStoreId: this.projCatId,
            goodsId: this.currentInfo.goodsId,
            nm: this.currentInfo.nm,
            models: this.currentInfo.models,
            unit: this.currentInfo.unitNm,
            num: this.currentInfo.num,
            unitPrice: this.currentInfo.unitPrice,
            amt: this.currentInfo.num*this.currentInfo.unitPrice,
          }
          if(index>-1){ //已有项目工程
              this.list[index].projItem.push(item)
              this.list[index].amt = this.list[index].projItem.reduce((p,n)=>{
                return p+n.amt
              },0)
            }else {
                let info = this.projectList.find(item=>item.id==this.projCatId) //工程项目信息
                let info2={
                  projCatId: info.id,
                  nm: info.nm,
                  amt: this.currentInfo.unitPrice*this.currentInfo.num,
                  check:true,
                  projItem:[item]
                }
                // info2.projItem.push(info)
                this.list.push(info2)
            }
            this.engShow = false
        },
        //编辑项目（设备）确认
        editorConfirm(){
          if(!this.currentInfo.nm){
            this.$message({
              message: '请选择或输入项目名称',
              type: 'warning'
            });
            return
          }
          if(!this.currentInfo.models){
            this.$message({
              message: '请输入规格',
              type: 'warning'
            });
            return
          }
          if(!this.currentInfo.unitPrice){
            this.$message({
              message: '请输入单价',
              type: 'warning'
            });
            return
          }
          if(!this.currentInfo.num){
            this.$message({
              message: '请输入数量',
              type: 'warning'
            });
            return
          }
          let index  = this.list.findIndex(item=>item.projCatId==this.projCatId)
          let info = JSON.parse(JSON.stringify(this.currentInfo))
          info.amt =  info.unitPrice*info.num
          this.list[index].projItem[this.currentProIndex] = info
          this.list[index].amt = this.list[index].projItem.reduce((p,n)=>{
            return p+n.amt
          },0)
          this.$set(this.list[index].projItem,this.currentProIndex,info)
          // this.$set(this.list,index,this.list[index])
          console.log(this.list)
          this.changeMoney()
          this.editorShow=false
        },
        //删除项目（设备确认）
        deleteConfirm(){
          let index  = this.list.findIndex(item=>item.projCatId==this.projCatId)
          this.list[index].projItem.splice(this.currentProIndex,1)
          this.list[index].amt = this.list[index].projItem.reduce((p,n)=>{
            return p+n.amt
          },0)
          this.$set(this.list,index,this.list[index])
          this.deleteShow = false
        },
        //添加子项目名称（设备）确认
        projectConfirm(){
          let index  = this.list.findIndex(item=>item.projCatId==this.projCatId)
          this.list[index].projItem.push({
            skuId:this.currentInfo.skuId,
            catStoreId: this.projCatId,
            goodsId: this.currentInfo.goodsId,
            nm: this.currentInfo.nm,
            models: this.currentInfo.models,
            unit: this.currentInfo.unitNm,
            num: this.currentInfo.num,
            unitPrice: this.currentInfo.unitPrice,
            amt: this.currentInfo.num*this.currentInfo.unitPrice,
          })
          this.list[index].amt = this.list[index].projItem.reduce((p,n)=>{
            return p+n.amt
          },0)
          this.projectShow = false
        },
        //更换设备
        changeProduct(id){
          console.log(id)
          let val = this.productList.find(item=>item.skuId==id)
          if(val){
            this.currentInfo.nm = val.nm
            this.currentInfo.goodsId = val.id
            this.currentInfo.models = val.attrDesc
            this.currentInfo.unitPrice = val.origPrice
            this.currentInfo.unitNm = val.unitNm
            this.disabled = true
          }else {
            this.currentInfo.nm = id
            this.currentInfo.goodsId = ''
            this.currentInfo.models = ''
            this.currentInfo.unitPrice = ''
            this.currentInfo.unitNm = ''
            this.disabled = false
          }
          this.currentInfo.num = 1

        },
        //远程搜索
        remoteMethod(query) {
          if (query !== '') {
            this.loading = true;
            this.getProduct(query)
          }
        },

        addAll(type,info,index){   //弹窗
          console.log(type)
          if(type=='engineer'){//添加项目工程
            this.engShow=true
          }else if(type=='editor'){//编辑项目（设备）
            this.currentProIndex = index
            this.projectNow = info.nm
            this.projCatId = info.projCatId
            this.currentInfo = JSON.parse(JSON.stringify(info.projItem[index]))
            this.editorShow=true
          }else if(type=='delete'){//删除
            this.currentProIndex = index
            this.projectNow = info.projItem[index].nm
            this.projCatId = info.projCatId
            this.deleteShow=true
          }else if(type=='project'){//添加项目工程
            this.projectNow = info.nm
            this.projCatId = info.projCatId
            this.projectShow=true
          }
        },
        checkEng(item){   //选择项目工程
          item.check=!item.check
        },
        back(){//返回
			    this.$router.back()
        },
      },
    }
</script>
<style lang="less">
  .popupBox{
    .bodyer{
      .engineer{
        .el-select{
          width: 59.8%;
        }
      }
      .editor{
      }
      .project{
      }
      >div{
        .el-select{
          height: 52px;
          margin: 0 20px;
          .el-input{
            height: 100%;
            >input{
              height: 100%;
              border-radius:0;
            }
          }
        }
      }

    }
  }
  .mainBasic .msg{
    .el-input--suffix .el-input__inner{
      height: 52px;
      border-radius: 0;
    }
  }
  .tablePro{
    .el-table__header thead{
      color: #666666;
      font-size:16px;
      font-weight:400;
    }
    .el-table__header th{
      background-color: #F7F7F7;

    }
    .el-table__body tr{
      color: #333333;
      font-size:15px;
    }
    .el-table__fixed-right{
      height: 100%!important;
    }
  }
</style>
<style lang="less" scoped>
  @import url("../../assets/css/init.less");
  input:disabled{
    background: #F5F7FA;
  }
  /deep/.el-input.is-disabled .el-input__inner{
    color: #666666;
  }
  .mainSele{
    position: relative;
    .popupBox{
      position: fixed;
      width: 100%;
      height: 100%;
      left: 0;
      top: 0;
      display: flex;
      justify-content: center;
      align-items: center;
      background-color: rgba(0,0,0,0.23);
      z-index: 5;
      >div{
        background-color: #FFFFFF;
        border-radius:8px;
        display: flex;
        flex-direction: column;
        height: 80%;
        // z-index: 9;
        .head{
          position: relative;
          display: flex;
          justify-content: center;
          align-items: center;
          height: 77px;
          border-bottom: 1px solid #DFDFDF;
          >p{
            font-size:18px;
            color:rgba(51,51,51,1);
            line-height:18px;
          }
          >img{
            position: absolute;
            width:20px;
            height:20px;
            top: 20px;
            right: 20px;
            cursor: pointer;
          }
        }
        .bodyer{
          padding: 30px 30px 0;
          flex: 1;
          overflow-y: scroll;
          >div{
            display: flex;
            align-items: center;
            height:52px;
            margin-bottom: 30px;
            >span{
              // width: 100px;
            }
            >input{
              // width: 59.8%;
              height: 52px;
              padding-left: 19px;
              margin: 0 20px;
              border:1px solid rgba(221, 221, 221, 1);
              font-size:15px;
              color:rgba(51,51,51,1);
            }
            >button{
              background:rgba(53,53,53,0);
              border:1px solid rgba(39, 120, 190, 1);
              color:rgba(39,120,190,1);
              border-radius:3px;
              width: 15.3%;
              height:52px;
            }
            >div{
              // width: 59.8%;
              height: 52px;
              margin: 0 20px;
              display: flex;
              justify-content: space-between;
              >button{
                font-size:16px;
                height:52px;
              }
              .cancelBtn{
                width:36%;
                background:rgba(226,35,26,0);
                border:1px solid rgba(221, 221, 221, 1);
                color:rgba(102,102,102,1);
                // margin-right: 20px;
              }
              .confirmBtn{
                width:58.8%;
                background:rgba(39,120,190,1);
                color:rgba(255,255,255,1);
                border: none;
              }
            }
          }
        }
      }
      .engineer{
        width: 691px;
        .bodyer{
          >div{
            >span{
              width: 100px;
            }
            >input{
              width: 59.8%;
            }
            >div{
              width: 59.8%;
            }
          }
        }
      }
      .editor{
        width: 584px;
        .bodyer{
          >div{
            >span{
              width: 100px;
            }
            >input{
              width: 75.1%;
            }
            >div{
              width: 75.1%;
            }
          }
        }
      }
      .delete{
        width: 384px;
        .bodyDel{
          padding: 37px;
          >p{
            font-size:15px;
            color:rgba(102,102,102,1);
            text-align: center;
            margin-bottom: 60px;
            >span{
              color: #FF5A5A;
            }
          }
          >div{
            display: flex;
            justify-content: space-between;
            >button{
              width: 45.1%;
              height:42px;
            }
            .cancelBtn{
              background:rgba(226,35,26,0);
              border:1px solid rgba(221, 221, 221, 1);
              color:rgba(102,102,102,1);
              // margin-right: 20px;
            }
            .confirmBtn{
              background:rgba(39,120,190,1);
              color:rgba(255,255,255,1);
              border: none;
            }
          }
        }
      }
      .project{
        width: 604px;
        .bodyPro{
          padding: 30px 30px 0px 30px;
          flex: 1;
          overflow-y: scroll;
          .firstBox{
            padding: 0 30px 0 18px;
            margin-bottom: 30px;
            height: 52px;
            display: flex;
            align-items: center;
            >span{
              width: 100px;
              margin-right: 10px;
              font-size:15px;
              color:rgba(102,102,102,1);
            }
            >input{
              border:1px solid rgba(221, 221, 221, 1);
              width: 77.8%;
              height: 100%;
              padding-left: 19px;
            }
            >div{
              width: 77.8%;
              height: 52px;
              display: flex;
              justify-content: space-between;
              .cancelBtn{
                width:36%;
                background:rgba(226,35,26,0);
                border:1px solid rgba(221, 221, 221, 1);
                color:rgba(102,102,102,1);
                // margin-right: 20px;
              }
              .confirmBtn{
                width:58.8%;
                background:rgba(39,120,190,1);
                color:rgba(255,255,255,1);
                border: none;
              }
            }
          }
          .inputBox{
            padding: 30px 30px 10px 18px;
            margin-bottom: 30px;
            background-color: #F5F5F5;
            >div{
              height: 52px;
              margin-bottom: 20px;
              display: flex;
              align-items: center;
              >span{
              width: 100px;
              margin-right: 10px;
              font-size:15px;
              color:rgba(102,102,102,1);
              }
              >input{
                border:1px solid rgba(221, 221, 221, 1);
                width: 77.8%;
                height: 100%;
                padding-left: 19px;
              }
            }

          }
        }
      }
    }
    .head{
      display: flex;
      justify-content: space-between;
      align-items: center;
      height: 59px;
      width: 100%;
      padding: 0 24px 0 30px;
      border-bottom: 1px solid rgba(0,0,0,0.1);;
      >p{
        display: flex;
        align-items: center;
        font-size:20px;
        color:#333333;
        height: 18px;
        .lineC{
          display: block;
          height: 18px;
          width: 3px;
          background-color: #2778BE;
          margin-right: 8px;
        }
      }
      >span{
        display: block;
        height:12px;
        font-size:14px;
        color: #666666;
      }
    }
    >div{
      padding: 0 30px;
      .title{
        display: flex;
        align-items: center;
        justify-content: center;
        // margin-bottom: 60px;
        padding-top: 60px;
        >p{
          font-size:20px;
          color:rgba(51,51,51,1);
          margin: 0 30px;
        }
      }
      .msg{
        .floatL{
          float: left;
          padding-right: 30px;
        }
        .floatR{
          float: right;
          padding-left: 30px;
        }
      }
      .money{
        width: 100%;
        height:115px;
        background:rgba(243,243,243,1);
        margin-bottom: 10px;
        display: flex;
        flex-direction: column;
        align-items: center;
        .detail{
          display: flex;
          align-items: center;
          margin-top: 27px;
          margin-bottom: 30px;
          .detail-title{
            font-size:14px;
            color:rgba(102,102,102,1);
          }
          .price{
            font-size:24px;
            color:rgba(230,67,71,1);
          }
        }
        .rmks{
          font-size:14px;
          color:rgba(153,153,153,1);
        }
      }
    }
    .mainBasic{
      .msg{
        // padding: 0 30px;
        margin-top: 60px;
        // height: 492px;
        overflow: hidden;
        >div{
          width: 50%;
          display: flex;
          align-items: center;
          margin-bottom: 30px;
          >span{
            width: 95px;
            font-size:16px;
            color:rgba(102,102,102,1);
          }
          >input{
            width: 78%;
            height:52px;
            border:1px solid rgba(221, 221, 221, 1);

            font-size:14px;
            line-height: 52px;
            padding-left: 20px;
          }
          .el-select{
            width: 78%;
            height:52px;
            // border:1px solid rgba(221, 221, 221, 1);
          }
        }
      }
    }
    .mainDetail{
      width: 100%;
      .btnBox{
        display: flex;
        justify-content: flex-end;
        margin: 44px 0 30px;
        .btnAdd{
          background:rgba(39,120,190,1);
          border-radius:3px;
          width:201px;
          height:43px;
          line-height: 43px;
          text-align: center;
          font-size:15px;
          color:rgba(255,255,255,1);
          margin-right: 0;
        }
      }
      .msg{
        .proBox{
          border: 1px solid #DFDFDF;
          width: 100%;
          margin-bottom: 60px;
          .proTitle{
            height: 57px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 0 28px 0 20px;
            .checkPro{
              display: flex;
              align-items: center;
              >img{
                width:12px;
                height:12px;
                margin-right: 4px;
              }
              >span{
                font-size:16px;
                color:rgba(102,102,102,1);
              }
            }
            .proPrice{
              font-size:18px;
              color:rgba(230,67,71,1);
            }
          }
          .addPro{
            display: block;
            margin: 20px auto;
            width:20.4%;
            height:43px;
            background:rgba(39,120,190,0);
            border:1px solid rgba(39, 120, 190, 1);
            border-radius:3px;
            font-size:15px;
            color:rgba(39,120,190,1);
            line-height:43px;
          }
          .btnList{
            display: flex;
            justify-content: center;
            align-items: center;
            // flex-direction: column;
            >button{
              flex: 1;
            }
            .lineC{
              width: 1px;
              height:15px;
              background:rgba(0,0,0,1);
              opacity:0.22;
              // margin: 0 5px;
            }
          }
        }
        .proBox:last-child{
          margin-bottom: 9px;
        }
      }
    }
    .mainTotal{
      .msg{
        // padding: 0 30px;
        margin-top: 60px;
        height: 492px;
        >div{
          width: 50%;
          display: flex;
          align-items: center;
          margin-bottom: 30px;
          >span{
            font-size:16px;
            color:rgba(102,102,102,1);
          }
          >input{
            width: 56.5%;
            height:52px;
            border:1px solid rgba(221, 221, 221, 1);

            font-size:14px;
            line-height: 52px;
            padding-left: 20px;
          }
        }
        .floatL{
          >span{
            width: 80px;
            margin-left: 10px;
          }
        }
        .floatR{
          >span{
            width: 140px;
            margin-left: 7px;
          }
        }

      }
    }
    .mainPrice{
      .estimatePrice{
        display: flex;
        margin-top: 51px;
        justify-content: center;
        >p{
          font-size:16px;
          color:rgba(102,102,102,1);
          margin-left: 16px;
          margin-right: 16px;
          padding-top: 19px;
        }
        >div{
          display: flex;
          flex-direction: column;
          width: 28.2%;
          >input{
            width: 100%;
            height: 52px;
            border:1px solid rgba(221, 221, 221, 1);
            font-size:14px;
            color:rgba(153,153,153,1);
            line-height:52px;
            padding-left: 20px;
            margin-bottom: 60px;
          }
          >button{
            width: 100%;
            height: 52px;
            background:rgba(39,120,190,1);
            border-radius:3px;
            font-size:18px;
            color:rgba(255,255,255,1);
            line-height:52px;
            text-align: center;
            border: none;
            margin-bottom: 60px;
          }
        }
      }
    }
  }
  input::-webkit-input-placeholder {
    /* Chrome/Opera/Safari */
        color: #999;
  }
  input::-moz-placeholder {
      /* Firefox 19+ */
      color: #999;
  }
  input:-ms-input-placeholder {
      /* IE 10+ */
      color: #999;
  }
  input:-moz-placeholder {
      /* Firefox 18- */
      color: #999;
  }

  .point{
    cursor:pointer;
  }
</style>
