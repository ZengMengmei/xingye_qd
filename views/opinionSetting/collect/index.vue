<template>

  <div style="padding:30px;">
    <el-alert :closable="false" title="采集配置" />
  </div>
</template>

<script>


export default {
  name: 'Collect',
  data () {

  },
  created () {

  },
  methods: {
    getNewsList () {
      crudNews.getNews().then(res => {
        console.log(res)
        this.newslist = res.content
        this.currentTotal = res.content.length
        console.log(this.newslist)
        console.log(this.currentTotal)
      })
    },

  }
}

</script>

<style scoped>
</style>
