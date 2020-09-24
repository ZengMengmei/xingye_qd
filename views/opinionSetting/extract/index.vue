<template>

  <div class="app-container">
    <div class="align-center">
      <div style="margin: 15px 0;"></div>
      <div>
        <el-row style="margin: 15px 0;">
          长文本抽取阈值/KB：<el-input v-model="longText" style="width: 200px" type="number"></el-input>
        </el-row>
        <el-row style="margin: 15px 0;">
          短文本抽取阈值/KB：<el-input v-model="shortText" style="width: 200px" type="number"></el-input>
        </el-row>

      </div>
      <div style="margin: 15px 0;"></div>
      <div>
        <el-button type="primary">确认修改</el-button>
      </div>
    </div>

  </div>
</template>

<script>


export default {
  name: 'Extract',
  data () {
    return {
      longText: 5000,
      shortText: 500,

    }
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
.align-center {
  margin: 0 auto; /* 居中 这个是必须的，，其它的属性非必须 */
  text-align: center; /* 文字等内容居中 */
  font-size: 16px;
  margin-top: 10px;
  margin-bottom: 15px;
  width: 600px;
  height: 300px;
}
</style>
