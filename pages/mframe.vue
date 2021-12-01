<template>
  <div>
    <iframe :src="iframeUrl" class="frame-box" frameborder="0"></iframe>
  </div>
</template>

<script>
  export default {
    layout: 'myFrame',
    name: 'mframe',
    data() {
      return {
        iframeUrl: '',
        typeCd: 1, //1 船舶设计进入，不需要登录 2船舶商城进入，不需要登录 3船舶档案进去，需要登录
        id: ''
      }
    },
    components: {},
    computed: {},
    watch: {

    },
    async mounted() {
      this.typeCd = this.until.getQueryString('typeCd')
      this.id = this.until.getQueryString('id')
      if (this.typeCd == 3) {
        if (!this.until.seGet('currentRole')) {
          this.$confirm('您未登录，请登录！', '提示', {
            confirmButtonText: '确定',
            cancelButtonText: '取消',
            type: 'warning'
          }).then(() => {
            location.href = './login/login'
          }).catch(() => {

          });
        } else {
          let orgEnterId = JSON.parse(this.until.seGet('currentRole')).id
          this.api.shipInfoBasic(this.id, {
            orgEnterId: orgEnterId
          }).then(res => {
            this.iframeUrl = res.vrUrl
          })
        }
      } else if(this.typeCd==1) {
        this.api.designFileDetail(this.id).then(res => {
          this.iframeUrl = res.vrUrl
        })
      } else if(this.typeCd==2) {
        this.api.productGetVr(this.id).then(res => {
          this.iframeUrl = res.msg
        })
      }
    },
    methods: {

    }
  }
</script>

<style lang="less" scoped>
  .frame-box {
    width: 100vw;
    height: 100vh;
  }
</style>
