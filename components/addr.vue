<template>
  <div id="addr">
    <el-select v-model="addr.provinceId" placeholder="省" @change="changeProvince">
      <el-option
        v-for="item in provinceList"
        :key="item.id"
        :label="item.nm"
        :value="item.id">
      </el-option>
    </el-select>
    <el-select v-model="addr.cityId" placeholder="市" @change="changeCity">
      <el-option
        v-for="item in cityList"
        :key="item.id"
        :label="item.nm"
        :value="item.id">
      </el-option>
    </el-select>
    <el-select v-model="addr.regionId" placeholder="区" @change="changeRegion">
      <el-option
        v-for="item in regionList"
        :key="item.id"
        :label="item.nm"
        :value="item.id">
      </el-option>
    </el-select>
  </div>
</template>
<script>
  import {mapState} from "vuex";
  export default {
    props:[],
    data(){
      return{
        addr:{
          province:'',
          provinceId:'',
          city:'',
          cityId:'',
          region:'',
          regionId:'',
        },
        addrInfo:[],
        provinceList:[],
        cityList:[],
        regionList:[]
      }
    },
    components: {
    },
    watch:{
      addrInfo(){

      },
      'addr.provinceId'(id){
        this.getCity(id)
      },
      'addr.cityId'(id){
        this.getRegion(id)
      }
    },
    computed: {
      ...mapState([
        'bWidth',
        'width'
      ])
    },
    mounted(){
        // this.getProvice()
    },
    methods:{
      changeProvince(id){
        let info = this.provinceList.find(item=> item.id == id)
        this.addr.province = info.nm
        this.addr.provinceId = info.id
      },
      changeCity(id){
        let info = this.cityList.find(item=> item.id == id)
        this.addr.city = info.nm
        this.addr.cityId = info.id
      },
      changeRegion(id){
        let info = this.regionList.find(item=> item.id == id)
        this.addr.region = info.nm
        this.addr.regionId = info.id
        this.$emit('changeAddr',JSON.stringify(this.addr))
      },

      //省
      async getProvice(data){
        console.log('省')
        let qry = this.query.new()
        this.query.toW(qry,'pid','100000','EQ')
        this.provinceList = await this.api.getAddr(this.query.toEncode(qry))
         if(data){
           this.addrInfo=data.split('-')
         }
         if(this.addrInfo[0]){
           this.addr.province = this.addrInfo[0]
           let info = this.provinceList.find(item=>item.nm==this.addr.province)
           this.addr.provinceId = info.id
           this.addrInfo[0]=''
         }else {
           this.addr.province = this.provinceList[0].nm
           this.addr.provinceId = this.provinceList[0].id
         }
      },
      async getCity(id){
        let qry = this.query.new()
        this.query.toW(qry,'pid',id,'EQ')
        this.cityList = await this.api.getAddr(this.query.toEncode(qry))
        if(this.addrInfo[1]){
          this.addr.city = this.addrInfo[1]
          let info = this.cityList.find(item=>item.nm==this.addr.city)
          this.addr.cityId = info.id
          this.addrInfo[1]=''
        }else {
          this.addr.city = this.cityList[0].nm
          this.addr.cityId= this.cityList[0].id
        }

      },
      async getRegion(id){
        let qry = this.query.new()
        this.query.toW(qry,'pid',id,'EQ')
        this.regionList = await this.api.getAddr(this.query.toEncode(qry))
        if(this.addrInfo[2]){
          this.addr.region = this.addrInfo[2]
          let info = this.regionList.find(item=>item.nm==this.addr.region)
          this.addr.regionId = info.id
          this.addrInfo[2]=''
        }else {
          this.addr.region = this.regionList[0].nm
          this.addr.regionId = this.regionList[0].id
        }

        this.$emit('changeAddr',JSON.stringify(this.addr))
      }
    }
  }
</script>
<style lang="less">
  @import url("../assets/css/init.less");
  #addr{
    width: 100%;
    display: flex;
    display: -webkit-flex;
    align-items: center;
    .el-select{
      flex: 1;
      &:nth-of-type(2){
        margin: 0 8px;
      }
    }
  }
</style>

