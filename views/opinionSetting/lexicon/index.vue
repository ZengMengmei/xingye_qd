<template>

  <div class="app-container">
    <!--部门数据-->
    <el-row style="margin-left:20px">
      <!--工具栏-->
      <div class="head-container">

        词语名称：<el-input v-model="wordname" style="width: 250px;margin-right: 50px"></el-input>

        <!-- 搜索 -->
        词语极性：
        <el-select v-model="polarityselected" placeholder="请选择" style="margin-right: 50px">
          <el-option v-for="item in polarityOptions" :key="item.value" :label="item.label" :value="item.value" />
        </el-select>

        词语权重：
        <el-select v-model="weightselected" placeholder="请选择" style="margin-right: 50px">
          <el-option v-for="item in weightOptions" :key="item.value" :label="item.label" :value="item.value" />
        </el-select>

        <el-button type="primary" style="margin-right: 10px">查询</el-button>
        <el-button type="warning">重置</el-button>

      </div>
    </el-row>

  </div>
</template>

<script>


export default {
  name: 'Lexicon',
  data () {
    return {
      wordname: '',
      polarityOptions: [
        { value: '选项1', label: '褒义' },
        { value: '选项2', label: '贬义' }
      ],
      polarityselected: '',
      weightOptions: [
        { value: '选项1', label: '高' },
        { value: '选项2', label: '中' },
        { value: '选项3', label: '低' }
      ],
      weightselected: '',
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
</style>
