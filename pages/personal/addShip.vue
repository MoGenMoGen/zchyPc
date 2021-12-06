<template>
<!-- 新增船舶设计-->
<div >
  <div class="popupBox"  v-if="addShipShow">
    <div class="mainAddShip">
      <div class="popupHead">
        <span>船舶档案</span>
        <img :src="close" @click="addShipShow=false" class="point">
      </div>
      <div class="tableAddShip">
        <el-table
         :data="tableData"
         style="width: 100%">
           <el-table-column
             prop="cd"
             align="center"
             width="250"
             label="船舶编号">
           </el-table-column>
           <el-table-column
             prop="nm"
             width="240"
             label="船舶名称">
           </el-table-column>
           <!-- <el-table-column
             width="180"
             prop="productName"
             label="客户名称">
           </el-table-column> -->
           <el-table-column
             width="120"
             prop="statusNm"
             align="center"
             label="船舶状态">
           </el-table-column>
           <el-table-column
             fixed="right"
             align="center"
             width="180"
             prop="operations"
             label="操作">
             <template slot-scope="scope">
                <button @click="toAddShip(scope.row.id)" class="shipCopyBtn">添加</button>
             </template>
           </el-table-column>
        </el-table>
      </div>
    </div>
  </div>
  <add-equip :show="addEqShow" @close="fClose" :solutionId='id' @confirm="confirm"></add-equip>
  <div class="head">
    <p><span class="lineC"></span>新增船舶设计</p>
    <span @click="back" class="point">< 返回 船舶展示</span>
  </div>
  <div class="addDesignMain">
    <div style="width: 100%;margin-bottom: 40px;padding-right: 110px;">
      <span><i class="redStars">*</i>船舶名称 :</span>
      <input type="text" v-model="form.nm" placeholder="请输入船舶名称" style="flex: none;">
      <p class="addShipBtn" v-if="ways"><button class="point" @click="addShipShow=true">添加船舶</button><span><span style="color: #E4393C;">* </span> 可从船舶档案获取船舶信息,快速新增船舶设计</span></p>
    </div>
    <div style="width: 100%;">
      <span><i class="redStars">*</i>船舶主图 :</span>
      <div class="imgBox">
        <div class="uploadImg point" :style="{height:imgBoxHeight+'px',width:imgBoxHeight+'px'}" id="imgHBox">
          <div>
          	<p  :style="{lineHeight:(imgBoxHeight-20)+'px'}">+</p>
            <input ref="upload" type="file" name="file"   multiple="multiple" @change="upImg($event)"/>
          </div>
        </div>
        <div v-for="(item,index) in imgList" :key="index" :style="{height:imgBoxHeight+'px',width:imgBoxHeight+'px'}">
          <img :src="del" class="delImg point" @click="deleteImg(index)"/>
          <img :src="item.imgUrl" @click="toLink(item)"/>
        </div>
      </div>
    </div>
    <div class="floatL">
      <span><i class="redStars">*</i>船舶分类:</span>
      <!--<el-select v-model="form.cid2" placeholder="请选择船舶分类">-->
        <!--<el-option-->
          <!--v-for="item in cid2List"-->
          <!--:key="item.id"-->
          <!--:label="item.nm"-->
          <!--:value="item.id">-->
        <!--</el-option>-->
      <!--</el-select>-->
      <el-cascader
        v-model="cids"
        :options="cid2List"
        placeholder="请选择船舶分类"
        :props="{ expandTrigger: 'hover','label':'nm','value':'id' }"
        ></el-cascader>
    </div>
    <!--<div class="floatR">-->
      <!--<span><i class="redStars">*</i>船舶类型:</span>-->
      <!--<el-select v-model="form.cid3" placeholder="请选择船舶类型">-->
        <!--<el-option-->
          <!--v-for="item in cid3List"-->
          <!--:key="item.id"-->
          <!--:label="item.nm"-->
          <!--:value="item.id">-->
        <!--</el-option>-->
      <!--</el-select>-->
    <!--</div>-->
    <div class="floatL"><span><i class="redStars">*</i>最高价格 :</span><input v-model="form.origMaxPrice" type="number" min="0" placeholder="请输入船舶区间价格(万元)"></div>
    <div class="floatR"><span><i class="redStars">*</i>最低价格 :</span><input v-model="form.origMinPrice" type="number" min="0" placeholder="请输入船舶区间价格(万元)"></div>
    <div class="floatL"><span><i class="redStars">*</i>船舶户籍:</span><input v-model="form.registry" type="text" placeholder="船籍"></div>
    <div class="floatR">
      <span>船舶长度:</span>
      <el-select v-model="form.lengthCd" placeholder="船舶长度(m)">
        <el-option
          v-for="item in lengthList"
          :key="item.cd"
          :label="item.nm"
          :value="item.cd">
        </el-option>
      </el-select>
    </div>
    <!-- <input v-model="form.lengthCd" type="number" placeholder="船舶长度(m)"></div> -->
    <div class="floatL">
      <span><i class="redStars">*</i>船舶材质:</span>
      <el-select v-model="form.hullMaterialCd" placeholder="请选择材质">
        <el-option
          v-for="item in materList"
          :key="item.cd"
          :label="item.nm"
          :value="item.cd">
        </el-option>
      </el-select>
    </div>
    <div class="floatR">
      <span><i class="redStars">*</i>船舶航区:</span>
      <el-select v-model="form.sailingAreaCd" placeholder="请选择航区">
        <el-option
          v-for="item in areaList"
          :key="item.cd"
          :label="item.nm"
          :value="item.cd">
        </el-option>
      </el-select>
    </div>
    <div class="floatL"><span>完工日期:</span>
      <el-date-picker
        v-model="form.completeDate"
        type="date"
        placeholder="完工日期">
      </el-date-picker>
    </div>
    <div class="floatR"><span>船舶航速:</span><input v-model="form.speed" min="0" type="number" placeholder="船舶航速"></div>
    <div class="floatL"><span>船舶型宽:</span><input v-model="form.width" min="0" type="number" placeholder="船舶型宽(m)"></div>
    <div class="floatR"><span>船舶型深:</span><input v-model="form.deep" min="0" type="number" placeholder="船舶型深(m)"></div>
    <div class="floatL"><span>船舶总长:</span><input v-model="form.totalLen" min="0" type="number" placeholder="船舶总长(m)"></div>
    <div class="floatR"><span>公约总长:</span><input v-model="form.pactLen" min="0" type="number" placeholder="公约总长(m)"></div>
    <div class="floatL"><span>主机:</span><input v-model="form.hostBrand" type="text" placeholder="主机品牌"></div>
    <div class="floatR"><span>主机功率:</span><input v-model="form.hostPower" min="0" type="number" placeholder="主机功率(kW)"></div>
    <div class="floatL"><span>齿轮箱:</span><input v-model="form.gearBrand" type="text" placeholder="齿轮箱品牌"></div>
    <div class="floatR"><span>齿轮箱型号:</span><input v-model="form.gearModel" type="text" placeholder="齿轮箱型号"></div>
    <div class="floatL"><span>发动机组:</span><input v-model="form.generatorSetBrand" type="text" placeholder="发动机组品牌"></div>
    <div class="floatR"><span>发动机组功率:</span><input v-model="form.generatorSetPower" min="0" type="number" placeholder="发动机组功率(kW)"></div>
    <div class="floatL"><span>制冷机组:</span><input v-model="form.refrigeratorBrand" type="text" placeholder="制冷机组品牌"></div>
    <div class="floatR"><span>制冷机组功率:</span><input v-model="form.refrigeratorPower" min="0" type="number" placeholder="制冷机组功率(kW)"></div>
    <div class="floatL"><span>总吨:</span><input v-model="form.totalTon" min="0" type="number" placeholder="总吨位"></div>
    <div class="floatR"><span>净吨:</span><input v-model="form.netTon" min="0" type="number" placeholder="净吨位"></div>
    <div class="floatL"><span>满载排水:</span><input v-model="form.loadDisplacement" min="0" type="number" placeholder="满载排水"></div>
    <div class="floatR"><span>满载吃水:</span><input v-model="form.loadDraught" min="0" type="number" placeholder="满载吃水"></div>
    <div class="floatL"><span>船舶定员:</span><input v-model="form.complement" type="text" placeholder="船舶定员"></div>
    <div class="floatR"><span>详情描述 :</span><textarea v-model="form.description" name="" id="" cols="30" rows="10" placeholder="请输入"></textarea></div>
    <div><span>主要参数 :</span><button @click="addParamet" class="point">新增参数</button></div>
    <div v-for="(item,index) in parametList.args" :key='index' class="parametBox">
      <div class="floatL">
        <span>第{{index+1}}个参数名</span><input type="text" v-model="item.colNm">
      </div>
      <div class="floatR">
        <span>第{{index+1}}个类型值</span><input type="text" v-model="item.value">
      </div>
    </div>

    <div style="width: 100%;">
      <span>船舶详情 :</span>
      <!--<editor style="z-index: 99;width: 87%" :config=config ref="ueditor"></editor>-->
      <vue-ueditor-wrap v-model="form.intro" style="z-index: 99;width: 87%"  :config="myConfig"></vue-ueditor-wrap>
    </div>
    <div>
      <span>船舶设备 :</span>
      <button class="point" @click="addEqShow=true">添加设备</button>
    </div>
    <div class="equipBox" style="width: 100%;padding-right: 0;">
      <ship-equip v-if="equipShow" @toDel="toDel" @copyDevice='copyDevice'  ref="equipChange" :ways='ways' :id='id' :listA="nowEqiupList" :delGDeviceList="delGDeviceList" :width="width*0.81-80" :ifAddShip='true'></ship-equip><!-- v-if="type=='ship' || shipShow" v-show="currentType=='equip'" :id="currentId" -->
    </div>
    <div class="addShipFooter">
      <button class="cancel">取消</button>
      <button class="confirm" @click="toSubmit">确认</button>
    </div>

  </div>
</div>
</template>

<script>
    import {mapState} from "vuex";
    import VueUeditorWrap from "vue-ueditor-wrap";
    import excel from '@/assets/img/personal/excel.png'
    import ppt from '@/assets/img/personal/ppt.png'
    import word from '@/assets/img/personal/word.png'
    import pdf from '@/assets/img/personal/pdf.jpg'
    import del from '@/assets/img/personal/del.png'
    import close from "@/assets/img/close.png"
    import editor from '@/components/editor.vue'
    // import editor from '@/components/ueditor.vue'
    import shipEquip from '@/components/onlineTrading/shipEquipAll.vue'
    import addEquip from '@/components/onlineTrading/addEquip.vue'
    export default {
      layout:'person',
      name: "shipowner",
      components: {
        editor,
        VueUeditorWrap,
        shipEquip,
        addEquip
      },
      data(){
          return{
            excel,
            ppt,
            word,
            pdf,
            del,
            close,
            nowEqiupList:[],  //添加的设备列表
            delGDeviceList:[],//被删除的设备列表
            addShipShow:false,  //船舶档案弹窗
            addEqShow:false,  //新增设备弹窗
            equipShow:false,  //船体设备组件
            // cid2:'',
            // cid3:'',
            cids:null,
            cid2List:[], //船舶2级分类列表
            cid3List:[], //船舶3级分类列表
            lengthList:[],  //船舶长度列表
            materList:[], //船舶材质列表
            areaList:[],  //船舶航区列表
            parametList:{args:[{colNm:"",value:""}]},//主要参数
            form:{
              "cid": "",  //分类id
              "cid1": "", //一级分类id
              "cid2": "", //二级分类id
              "cid3": "", //三级分类id
              "cids": "", //多级分类id
              "designId": "", //设计院id
              "cd": "", //商品编码
              "nm": "", //商品名称
              "description": "",  //描述
              "imgUrl": "", //图片

              "origMinPrice": "", //最低价格
              "origMaxPrice": "", //最高价格
              "registry": "", //船舶户籍
              "typesCd": "",  //船舶类型
              "sailingAreaCd": "",  //船舶航区
              "hullMaterialCd": "", //船体材质
              "lengthCd": "", //船舶长度
              "totalLen": "", //船舶总长
              "pactLen":"", //公约船长
              "width": "",  //船舶型宽
              "deep": "", //船舶型深
              "loadDraught": "",  //满载吃水
              "loadDisplacement": "", //满载排水
              "speed": "",  //船舶航速
              "complement": "", //船舶定员
              "hostBrandId": "",  //主机品牌id
              "hostBrand": "",  //主机品牌
              "hostPower": "",  //主机功率
              "gearBrandId": "",  //齿轮箱品牌id
              "gearBrand": "",  //齿轮箱品牌
              "gearModel": "",  //齿轮箱型号
              "totalTon": "", //总吨位
              "netTon": "", //净吨位
              "generatorSetBrandId": "",  //发电机组品牌id
              "generatorSetBrand": "",  //发电机组品牌
              "generatorSetPower": "",  //发电机组功率
              "refrigeratorBrandId": "",  //制冷机组品牌id
              "refrigeratorBrand": "",  //制冷机组品牌
              "refrigeratorPower": "",  //制冷机组功率
              "completeDate": "", //完工日期
              "mainParam": "",  //船舶其他参数
              "intro": "",  //详请
              // "seq": "排序",
              // "istop": "",  //是否置顶(0:未置顶, 1:已置顶)
              "rmks": "", //备注
              "shipSoluDeviceRos": [  //设备
                  // {
                  //   "solutionId": "", //船舶设计方案id
                  //   "goodsId": "",  //商品id
                  //   "skuId": "",  //sku id
                  //   "deviceType": "", //设备型号
                  //   "deviceSupply": "", //供应商
                  //   "deviceSupplyId": "", //供应商id
                  //   "cid": "",  //分类id
                  //   "cid1": "", //一级分类
                  //   "cid2": "", //二级分类
                  //   "cid3": "", //三级分类
                  //   "cids": "", //多级分类id
                  //   "cd": "", //编码
                  //   "nm": "", //名称
                  //   "num": "",  //数量
                  //   "imgUrl": "", //图片
                  //   "origMinPrice": "", //最低价格
                  //   "origMaxPrice": "", //最高价格
                  //   // "status": "", //状态 [r](0：上架，1：下架)
                  //   "rmks": "", //备注
                  //   "intro": "" //详细描述
                  // }
                ],
                "delDeviceIds": []  //删除设备id
            },
            myConfig: {
              // 编辑器不自动被内容撑高
              autoHeightEnabled: false,
              // 初始容器高度
              initialFrameHeight: 440,
              // 初始容器宽度
              initialFrameWidth: "100%",
              // 上传文件接口（这个地址是我为了方便各位体验文件上传功能搭建的临时接口，请勿在生产环境使用！！！）
              serverUrl: "https://www.ship88.cn/general/ueditor/exec",
              UEDITOR_HOME_URL: '/sinovat/UEditor/'
              // UEditor 资源文件的存放路径，如果你使用的是 vue-cli 生成的项目，通常不需要设置该选项，vue-ueditor-wrap 会自动处理常见的情况，如果需要特殊配置，参考下方的常见问题2
              // UEDITOR_HOME_URL:
              //   process.env.NODE_ENV === "production"
              //     ? "/static/ueditor/"
              //     : "/static/ueditor/"

            },
            config: {
              //可以在此处定义工具栏的内容
              // toolbars: [
              //  ['fullscreen', 'undo', 'redo','|','bold', 'italic', 'underline',
              //  '|','superscript','subscript','|', 'insertorderedlist', 'insertunorderedlist',
              //  '|','fontfamily','fontsize','justifyleft','justifyright','justifycenter','justifyjustify']
              // ],
              autoHeightEnabled: false,
              autoFloatEnabled: true,
              initialContent:'请输入内容',   //初始化编辑器的内容,也可以通过textarea/script给值，看官网例子
              autoClearinitialContent:true, //是否自动清除编辑器初始内容，注意：如果focus属性设置为true,这个也为真，那么编辑器一上来就会触发导致初始化的内容看不到了
              initialFrameWidth: null,
              initialFrameHeight: 450,
              serverUrl: "https://www.ship88.cn/general/ueditor/exec",
              UEDITOR_HOME_URL: '/sinovat/UEditor/'
            },

            imgList:[],
            imgBoxHeight:'100',
            currentRole:{},
            ways:'',//判断是新增还是修改
            id:'',
            flag:0,
          }
      },
      computed:{
        ...mapState([
          'bWidth',
          'width'
        ])
      },
      watch:{
        width(){
          this.getHeight()
        },
        'form.cid2':{
          handler: function() {
            // console.log('cid2变了')
            console.log('cid2 : '+this.form.cid2)
            this.getOptions('3')

          },

        },
        'form.cid3':{
          handler: function() {
            // console.log('cid3变了')
            console.log('cid3 : '+this.form.cid3)
             // if(this.form.cid2 && this.form.cid3){
               // this.form.cids=this.form.cid2+','+this.form.cid3
               // console.log(this.form.cids)
             // }
          },

        },
      },
      mounted() {
        this.currentRole=JSON.parse(this.until.seGet('currentRole'))
        if(this.until.getQueryString('ways')){    //新增
          this.ways= this.until.getQueryString('ways')
          console.log('ways :'+this.ways)
        }
        if(this.until.getQueryString('id')){  //修改，获取详情
          this.id= this.until.getQueryString('id')
          this.getInfo()
        }
        this.form.designId=this.currentRole.id
        this.getOptions()
        this.equipShow=true
        this.getList()
      },
      methods: {
        // //获取编辑器文本内容
        // getContent(){
        //   let content = this.$refs.ueditor.getUEContent();
        //   this.form.intro=content
        //   // console.log(content);
        // },
        async getInfo(){
          let data = await this.api.designFileDetail(this.id)
          // console.log('获取详情')
          // console.log(data)
          this.form=data
          this.cids = this.form.cids.split(',')
          this.cids.forEach((item,index)=>{
            this.$set(this.cids,index,parseInt(item))
          })
          // console.log(this.cids)
          let newList= data.imgUrl.split(',')
          newList.forEach(item=>{
            this.imgList.push({url:item,imgUrl:item})
          })
          // console.log( this.form.mainParam)
          let mainParam=JSON.parse(this.form.mainParam)
          // console.log(mainParam.args)
          // for(let [k,v] of Object.entries(mainParam.args)){
          //   if(v.length>0){
          //     this.parametList.args.push({
          //       colNm:k,
          //       value:v
          //     })
          //   }
          // }
          this.parametList.args=[]
          mainParam.args.forEach(item=>{
            this.parametList.args.push({
              colNm:item.colNm,
              value:item.value
            })
          })

        },
        async toAddShip(id){  //从船舶档案中添加船舶
          this.flag=0
          this.imgList=[]
          this.nowEqiupList=[]
          this.ways=''
          this.addShipShow=false
          let data = await this.api.shipInfoBasic(id)
          // console.log('获取添加船舶的详情')
          // console.log(data)
          if(data.cids.split(',').length==3){
            data.cid2=parseInt(data.cids.split(',')[1])
            data.cid3=parseInt(this.form.cids.split(',')[2])
          }else if(data.cids.split(',').length==2){
            data.cid2=parseInt(data.cids.split(',')[0])
            data.cid3=parseInt(data.cids.split(',')[1])
          }
          // data.cid2=''
          // data.cid3=''
          this.form=data
          this.cids = this.form.cids.split(',')
          this.cids.forEach((item,index)=>{
            this.$set(this.cids,index,parseInt(item))
          })
          // if(this.form.cids.split(',').length==3){
          //   this.form.cid2=parseInt(this.form.cids.split(',')[1])
          //   this.form.cid3=parseInt(this.form.cids.split(',')[2])
          // }else if(this.form.cids.split(',').length==2){
          //   this.form.cid2=parseInt(this.form.cids.split(',')[0])
          //   this.form.cid3=parseInt(this.form.cids.split(',')[1])
          // }

          // console.log('cid3 : '+this.form.cid3)
          let newList= data.imgUrl.split(',')
          newList.forEach(item=>{
            this.imgList.push({url:item,imgUrl:item})
          })
          // this.form.origMaxPrice=data.totalPrice
          this.form.mainParam=this.form.mainParams
          let mainParam=JSON.parse(this.form.mainParam)
          console.log(mainParam.args)
          this.parametList.args=[]
          mainParam.args.forEach(item=>{
            this.parametList.args.push({
              colNm:item.colNm,
              value:item.value
            })
          })
          let qry = this.query.new()
          // this.query.toW(qry,'pid','4942035725390848','EQ')
          let copyList=await this.api.fileDeviceList(this.query.toEncode(qry),{docsId:id})
          copyList.forEach(item=>{
            item.deviceSupply=item.supplierNm
            item.deviceType=item.model
            if(item.cids.split(',').length==3){
              item.cid2=item.cids.split(',')[1]
              item.cid3=item.cids.split(',')[2]
            }else if(item.cids.split(',').length==2){
              item.cid2=item.cids.split(',')[0]
              item.cid3=item.cids.split(',')[1]
            }
          })
          console.log(copyList)
          this.nowEqiupList=copyList
          this.ways='copy'
          this.id=id
        },
        copyDevice(copyList){ //添加的船舶的设备列表
          // console.log('添加的船舶的设备列表')
          // console.log(JSON.parse(copyList))
          // let data=JSON.parse(copyList)
          // data.forEach(item=>{
          //   if(item.cids.split(',').length==3){
          //     item.cid2=item.cids.split(',')[1]
          //     item.cid3=item.cids.split(',')[2]
          //   }else if(item.cids.split(',').length==2){
          //     item.cid2=item.cids.split(',')[0]
          //     item.cid3=item.cids.split(',')[1]
          //   }
          // })
          // this.nowEqiupList=data
        },
        toDel(item){
          // console.log(item)
          let data=JSON.parse(item)
          console.log(data)
          if(data.type=='jiekou'){
            this.delGDeviceList.push(data)
          }else if(data.type=='xinzen'){
            let k
            this.nowEqiupList.forEach((v,index)=>{
              if(v.id==data.id){
                k=index
              }
            })
            console.log(k)
            this.nowEqiupList.splice(k,1)
            this.$refs.equipChange.delList();
            console.log(this.nowEqiupList)
          }

        },
        async getOptions(num){
          if(!num){
            // let qry = this.query.new()
            // this.query.toW(qry,'pid','4942035725390848','EQ')
            // this.query.toO(qry,'seq','asc')
            this.cid2List=await this.api.getClassify2()  //船舶2级分类
            console.log('船舶2级分类')
            console.log(this.cid2List)
            this.lengthList= await this.api.dataDictionary('SHIP_LENGTH')  //船舶长度
            this.materList = await this.api.dataDictionary('HULL_MATERIAL')  //船舶材质
            this.areaList = await this.api.dataDictionary('SAILING_AREA')  //船舶航区
          }
          else if(num==3){
            console.log('flag : '+this.flag)
            this.cid3List=[]
            if(this.flag>0){
              this.form.cid3=''
              console.log('谁快一旦')
            }
            this.flag++

            let qry1 = this.query.new()
            this.query.toW(qry1,'pid',this.form.cid2,'EQ')
            // this.query.toO(qry,'seq','asc')
            this.cid3List=await this.api.getClassify(this.query.toEncode(qry1))
            // if(this.flag==1){
            //   this.form.cid3=parseInt(this.form.cids.split(',')[1])
            // }
          }
        },
        toSubmit(){
          this.form.cid=this.form.cid3
          console.log(this.cids)
          this.form.cids=this.cids.join(',')
          this.form.designId=this.currentRole.id
          this.form.imgUrl=''
          this.imgList.forEach((item,index)=>{
            this.form.imgUrl+=item.url
            if(index<this.imgList.length-1){
              this.form.imgUrl+=','
            }
          })
          let timeC=this.until.formatDate(this.form.completeDate)
          this.form.completeDate=timeC.year+'-'+timeC.month+'-'+timeC.day
          this.form.mainParam=this.parametList
          this.form.mainParam.args.forEach((item,index)=>{
            if(item.colNm==''&&item.value==''){
              this.form.mainParam.args.splice(index, 1);
            }
          })
          this.form.mainParam=JSON.stringify(this.form.mainParam)
          this.form.shipSoluDeviceRos=this.nowEqiupList
          console.log(this.form)
          if(!this.form.nm){
            this.$message({message: '请填写船舶名称',type: 'warning'});
            return
          }
          if(!this.form.imgUrl){
            this.$message({message: '请上传船舶照片',type: 'warning'});
            return
          }
          if(!this.form.cids){
            this.$message({message: '请选择船舶分类',type: 'warning'});
            return
          }
          if(!this.form.origMaxPrice){
            this.$message({message: '请填写最高价格',type: 'warning'});
            return
          }
          if(!this.form.origMinPrice){
            this.$message({message: '请填写最低价格',type: 'warning'});
            return
          }
          if(!this.form.registry){
            this.$message({message: '请填写船舶户籍',type: 'warning'});
            return
          }
          if(!this.form.hullMaterialCd){
            this.$message({message: '请填写船舶材质',type: 'warning'});
            return
          }
          if(!this.form.sailingAreaCd){
            this.$message({message: '请填写船舶航区',type: 'warning'});
            return
          }
          if(this.ways=='add' || this.ways=='copy'){
            this.api.addDesignFile(this.form).then(res=>{
              if(res.code==0){
                this.$message({
                  message: '提交成功，请等待审核',
                  type: 'success',
                  duration:'1000'
                });
                setTimeout(()=>{
                  window.history.back();
                },1500)
              }
            })
          }else{
            console.log('修改')
            let delDeviceList=[]
            this.delGDeviceList.forEach(item=>{
              delDeviceList.push(item.id)
            })
            this.form.delDeviceIds=delDeviceList
            // this.form.solutionId=this.id
            this.api.updDesignFile(this.form).then(res=>{
              if(res.code==0){
                this.$message({
                  message: '修改成功，请等待审核',
                  type: 'success',
                  duration:'1000'
                });
                setTimeout(()=>{
                  window.history.back();
                },1500)
              }
            })
          }

        },
        addParamet(){//新增参数
          this.parametList.args.push({colNm:"",value:""})
        },
        fClose(){
          console.log('父组件关闭')
          this.addEqShow=false
        },
        confirm(info){
          console.log('确认新增设备')
          console.log(JSON.parse(info))
          console.log(this.nowEqiupList)
          console.log(this.nowEqiupList.length)
          let flag=0
          this.nowEqiupList.forEach(item=>{

            if(JSON.parse(info).skuId==item.skuId){
              item.num+=JSON.parse(info).num
              flag++
            }
          })
          if(flag==0){
            this.nowEqiupList.push(JSON.parse(info))
          }
          this.$refs.equipChange.addGoodsC(JSON.parse(info));
          console.log(this.nowEqiupList)
          this.addEqShow=false
        },
        async getList(){
          let qry = this.query.new()
          this.query.toP(qry,'1','999')
          let param={
            keyWord:'',
            orgEnterId:this.currentRole.id
          }
          this.query.toO(qry,'cd','asc')
          console.log('获取数据')
          let data=await this.api.allShip(this.query.toEncode(qry),param)
          this.tableData=data.data.list
          console.log(this.tableData)
        },
        //获取高度，达到1：1
        getHeight(){
          this.imgBoxHeight=(this.width-60)*0.813*0.432*0.3;
          console.log(this.bWidth)
          console.log(this.width)
          console.log('高度'+this.imgBoxHeight)
        },
        //删除图片
        deleteImg(index){
           this.imgList.splice(index,1)
         },
         //上传图片
        async upImg(e){
                console.log(e)
          if(e.target.files.length===0){
            return
          }
             // for(let i=0;i<e.length;i++){
                let img = await this.api.uploadImg(e)
                let type=img.split('.')[img.split('.').length - 1]
                console.log(type)
                if(type=='pdf'){
                	this.imgList.push({url:img,imgUrl:this.pdf})
                }else if(type=='doc'||type=='docx'){
                	this.imgList.push({url:img,imgUrl:this.word})
                }else if(type=='ppt'||type=='pptx'){
                	this.imgList.push({url:img,imgUrl:this.ppt})
                }else if(type=='xls'||type=='xlsx'){
                	this.imgList.push({url:img,imgUrl:this.excel})
                }else{
                	this.imgList.push({url:img,imgUrl:img})
                }
                console.log('照片列表')
                console.log(this.imgList)
             // }
        },
        back(){//返回
          this.$router.go(-1)
        },

      },
    }
</script>
<style lang="less">
  .el-upload{
    text-align: left;
  }
  /*富文本*/
  .editor{
    z-index: 1500;
    padding-top: 1px;
    box-sizing: border-box;
    height: 1600px;
    display: flex;
    line-height: normal !important;
    >div{
      width:100%;
    }

  }
  #edui_fixedlayer{
    z-index: 2000 !important;
  }
  .shipCopyBtn{
    cursor: pointer;
    padding: 9px 30px;
    background-color: #FFFFFF;
    border:1px solid rgba(39, 120, 190, 1);
    border-radius:3px;
    font-size:15px;
    color:rgba(39,120,190,1);
  }
  .mainAddShip{
    .tableAddShip{
      .el-table__header thead{
        color: #666666;
        font-size:16px;
      }
      .el-table__header th{
        background-color: #F7F7F7;
      }
    }
  }
  .addDesignMain{
    .floatL,.floatR{
      .el-cascader,.el-select,.el-date-editor{
        height: 52px;
        flex:1;
        .el-input{
          line-height: 52px;
        }
        .el-input__inner{
          height: 52px;
          border-radius: 0;
        }
      }
    }

  }
</style>
<style lang="less" scoped>
  @import url("../../assets/css/init.less");
  .point{
    cursor:pointer;
  }
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
    z-index: 1001;
    .mainAddShip{
      width: 60.4%;
      padding: 31px 0 20px;
      background-color: #FFFFFF;
      display: flex;
      flex-direction: column;
      border-radius:8px;
      min-width: 604px;
      max-height: 80%;
      .popupHead{
        display: flex;
        margin-bottom: 30px;
        >span{
          font-size: 18px;
          color: #333333;
          display: block;
          margin: 0 auto;
          padding-left: 42px;
        }
        >img{
          width: 20px;
          height: 20px;
          margin: -11px 22px 0 0;
        }
      }
      .tableAddShip{
        .btnList{
          >button{
            cursor: pointer;
            padding: 9px 30px;
            background-color: #FFFFFF;
            border:1px solid rgba(39, 120, 190, 1);
            border-radius:3px;
            font-size:15px;
            color:rgba(39,120,190,1);
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
      font-family:Microsoft YaHei;
      font-weight:400;
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
      font-family:Microsoft YaHei;
      font-weight:400;
      color: #666666;
    }
  }
  .addDesignMain{
    padding: 40px 30px;
    >div{
      width: 50%;
      margin-bottom: 30px;
      display: flex;
      position: relative;
      padding-right: 40px;
      .addShipBtn{
        display: flex;
        flex-direction: column;
        align-items: center;
        position: absolute;
        right: 0;
        >button{
          width: 50%;
          height: 42px;
          background:rgba(39,120,190,1);
          font-size:16px;
          color:rgba(255,255,255,1);
          margin-bottom: 22px;
          border: none;
        }
        >span{
          font-size:14px;
          color:#999999;
        }
      }
      >span{
        width: 100px;
        font-size:16px;
        color:rgba(102,102,102,1);
        line-height: 52px;
        margin-right: 10px;
        .redStars{
          margin-left: -12px;
          margin-right: 5px;
          color: rgb(228, 57, 60);
        }
      }
      >input,>textarea{
        width: 37.2%;
        border:1px solid rgba(221, 221, 221, 1);
        font-size: 14px;
        flex: 1;
      }
      >input{
        height:52px;
        padding-left: 24px;
      }
      >textarea{
        height:112px;
        padding: 20px 24px 0;
        resize: none;
      }
      >button{
        width: 100px;
        height:52px;
        background:rgba(39,120,190,1);
        border: none;
        font-size:16px;
        color:rgba(255,255,255,1);
      }
      .imgBox{
        width: 43.2%;
        >div{
          width:30%;
          // width: 85px;
          margin-right: 1.5%;
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
              // line-height: 100%;
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
    }
    .parametBox{
      width: 100%;
      padding-right: 0px;
      >div{
        width: 50%;
        display: flex;
        padding-right: 40px;
        >span{
          width: 100px;
          font-size:16px;
          color:rgba(102,102,102,1);
          line-height: 52px;
          margin-right: 10px;
        }
        >input{
          width: 37.2%;
          border:1px solid rgba(221, 221, 221, 1);
          font-size: 14px;
          flex: 1;
          height:52px;
          padding-left: 24px;
        }
      }
    }
    .floatL{
      float: left;
    }
    .floatR{
      float: right;
    }
    .addShipFooter{
      width: 50%;
      margin-left: 50%;
      display: flex;
      justify-content: space-between;
      >button{
        height: 52px;
        font-size:16px;
        cursor: pointer;
      }
      .cancel{
        width: 37.2%;
        background:#FFFFFF;
        border:1px solid rgba(221, 221, 221, 1);
        color:rgba(102,102,102,1);
      }
      .confirm{
        width: 58.3%;
        background:rgba(39,120,190,1);
        border: none;
        color: #FFFFFF;
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
    textarea::-webkit-input-placeholder {
      /* Chrome/Opera/Safari */
          color: #999;
    }
    textarea::-moz-placeholder {
        /* Firefox 19+ */
        color: #999;
    }
    textarea:-ms-input-placeholder {
        /* IE 10+ */
        color: #999;
    }
    textarea:-moz-placeholder {
        /* Firefox 18- */
        color: #999;
    }
  }

</style>
