<template>

  <div class="app-container">
    分类标准词表

  </div>
</template>

<script>


export default {
  name: 'Classify',
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
