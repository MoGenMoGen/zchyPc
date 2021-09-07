<template>
<div id="home">
  <img :src="item"  v-for="(item,index) in imgList" :key="index"/>
</div>
</template>

<script>
    export default {
        name: "cooperationDetail",
      data(){
          return{
            imgList:[
              'https://sinovat.oss-cn-shanghai.aliyuncs.com/e99435180ada490286ed3bf2789c1bc9-%E5%8D%8F%E5%90%8C%E5%88%9B%E6%96%B0-%E8%AF%A6%E6%83%85_01.png',
              'https://sinovat.oss-cn-shanghai.aliyuncs.com/8371c7aae20c44d5954f50b05ddcca9c-%E5%8D%8F%E5%90%8C%E5%88%9B%E6%96%B0-%E8%AF%A6%E6%83%85_02.png',
              'https://sinovat.oss-cn-shanghai.aliyuncs.com/cf51811efefe4321926f8204298d5dea-%E5%8D%8F%E5%90%8C%E5%88%9B%E6%96%B0-%E8%AF%A6%E6%83%85_03.png',
              'https://sinovat.oss-cn-shanghai.aliyuncs.com/2fc6728877e248d792f43b6f569e2fc9-%E5%8D%8F%E5%90%8C%E5%88%9B%E6%96%B0-%E8%AF%A6%E6%83%85_04.png',
              'https://sinovat.oss-cn-shanghai.aliyuncs.com/fe719f96f33142fe93c9af395733ce98-%E5%8D%8F%E5%90%8C%E5%88%9B%E6%96%B0-%E8%AF%A6%E6%83%85_05.png',
              'https://sinovat.oss-cn-shanghai.aliyuncs.com/b733792ef4de426cb99b61cc02599890-%E5%8D%8F%E5%90%8C%E5%88%9B%E6%96%B0-%E8%AF%A6%E6%83%85_06.png',
              'https://sinovat.oss-cn-shanghai.aliyuncs.com/93441cdea3e445dc955872bcad739c57-%E5%8D%8F%E5%90%8C%E5%88%9B%E6%96%B0-%E8%AF%A6%E6%83%85_07.png',
              'https://sinovat.oss-cn-shanghai.aliyuncs.com/64893b35d9ec493998bb6489f21b286b-%E5%8D%8F%E5%90%8C%E5%88%9B%E6%96%B0-%E8%AF%A6%E6%83%85_08.png',
              'https://sinovat.oss-cn-shanghai.aliyuncs.com/018ebc205d7740bc8d15075eef7a8467-%E5%8D%8F%E5%90%8C%E5%88%9B%E6%96%B0-%E8%AF%A6%E6%83%85_09.png',
              'https://sinovat.oss-cn-shanghai.aliyuncs.com/72b54c9720c14e3bbb41dd8986b02148-%E5%8D%8F%E5%90%8C%E5%88%9B%E6%96%B0-%E8%AF%A6%E6%83%85_10.png',
              'https://sinovat.oss-cn-shanghai.aliyuncs.com/f1b258c7e5ca4b218e99fcb90804a431-%E5%8D%8F%E5%90%8C%E5%88%9B%E6%96%B0-%E8%AF%A6%E6%83%85_11.png',
              'https://sinovat.oss-cn-shanghai.aliyuncs.com/70fe2c2ba7b3466eb10892d49280f1f2-%E5%8D%8F%E5%90%8C%E5%88%9B%E6%96%B0-%E8%AF%A6%E6%83%85_12.png',
            ]
          }
      }
    }
</script>

<style lang="less" scoped>
  @import url("../../assets/css/init.less");
  img{
    display: block;
    width: 100%;
  }

</style>
