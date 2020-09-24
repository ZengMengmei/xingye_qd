<template>

  <div class="app-container">
    短信发送对象

  </div>
</template>

<script>


export default {
  name: 'Message',
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
