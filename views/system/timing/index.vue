<template>

  <div class="app-container">

    <!--部门数据-->
    <el-row style="margin-left:10px">
      <!--工具栏-->
      <div class="head-container">

        <!-- 搜索 -->
        选择网站来源：
        <el-select v-model="selected" placeholder="请选择">
          <el-option v-for="item in weboptions" :key="item.value" :label="item.label" :value="item.value" />
        </el-select>

        <!-- 排序
        <div style="float: right;">
          按：
          <el-select v-model="selected2" placeholder="请选择">
            <el-option v-for="item in orderoptions" :key="item.value" :label="item.label" :value="item.value" />
          </el-select>
          <el-radio v-model="radio" label="asc" style="margin-left:10px">升序</el-radio>
          <el-radio v-model="radio" label="desc" style="margin-left:-30px">降序</el-radio>
          <el-button class="filter-item" size="mini" type="primary" style="margin-left:-10px" @click="order">排序
          </el-button>
        </div> -->

      </div>
    </el-row>
    <!--表格渲染-->
    <el-table :data="newslist.slice((currentPage-1)*pageSize,currentPage*pageSize)"
      :default-sort="{prop: 'date', order: 'descending'}" size="small" style="width: 100%;" border stripe>
      <el-table-column show-overflow-tooltip prop="source" width="300px" align="center" label="来源网站" sortable />

      <el-table-column show-overflow-tooltip prop="title" align="center" label="新闻标题" sortable />
      <el-table-column show-overflow-tooltip prop="date" width="200px" align="center" label="时间" :formatter='formatter'
        sortable />
      <el-table-column label="操作" width="170px" align="center" fixed="right">
        <template slot-scope="scope">
          <el-button size="mini" style="margin-right: 3px;" type="text" @click="view(scope.row.url)">查看</el-button>
        </template>
      </el-table-column>

    </el-table>
    <div align="middle" style="margin-top: 10px">
      <!--分页组件-->
      <el-pagination @size-change="handleSizeChange" @current-change="handleCurrentChange" :current-page="currentPage"
        :page-sizes="[10, 20, 30, 40]" :page-size="pageSize" layout="total, sizes, prev, pager, next, jumper"
        :total="currentTotal">
      </el-pagination>
    </div>

  </div>
</template>

<script>

import Vue from 'vue'

import crudNews from '@/api/system/news'
import CRUD from '@crud/crud'
import pagination from '@crud/Pagination'
import moment from 'moment'

export default {
  name: 'News',
  components: { pagination, moment },
  cruds () {
    return CRUD({ title: '公告新闻', url: 'api/message/getList', crudMethod: { ...crudNews } })
  },
  data () {
    return {
      weboptions: [
        { value: 'allweb', label: '全部' },
        { value: '选项2', label: '人民日报' },
        { value: '选项3', label: '中央新闻' }
      ],
      selected: 'allweb',
      // orderoptions: [
      //   { value: 'none', label: '无' },
      //   { value: 'date', label: '时间' },
      //   { value: 'web', label: '来源网站' },
      //   { value: 'head', label: '新闻标题' }
      // ],
      // selected2: 'none',
      // radio: 'asc',
      newslist: [],
      currentTotal: 0,
      currentPage: 1,
      pageSize: 10,
    }
  },
  created () {
    this.getNewsList()
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
    formatter (row, column) {
      // 获取每行的时间
      const date = row[column.property]
      // 对时间格式进行处理
      var d = date.substring(0, 2)
      var year = date.substring(5, 9)
      var a = date.replace(date.substring(5, 9), d)
      var b = a.replace(date.substring(0, 2), year)
      return moment(new Date(b)).format('YYYY-MM-DD HH:mm:ss');
    },

    // order () {
    //   console.log(this.selected2)
    //   if (this.selected2 === 'date') {
    //     if (this.radio === 'asc') {
    //       this.info.sort((a, b) => new Date(a.time).getTime() - new Date(b.time).getTime())
    //     } else {
    //       this.info.sort((a, b) => new Date(b.time).getTime() - new Date(a.time).getTime())
    //     }
    //   } else if (this.selected2 === 'web') {
    //     if (this.radio === 'asc') {
    //       this.info.sort(function (x, y) {
    //         console.log(this.flag)
    //         return x['web'].localeCompare(y['web'])
    //       })
    //     } else {
    //       this.info.sort(function (x, y) {
    //         return y['web'].localeCompare(x['web'])
    //       })
    //     }
    //   } else if (this.selected2 === 'head') {
    //     if (this.radio === 'asc') {
    //       this.info.sort(function (x, y) {
    //         console.log(this.flag)
    //         return x['head'].localeCompare(y['head'])
    //       })
    //     } else {
    //       this.info.sort(function (x, y) {
    //         return y['head'].localeCompare(x['head'])
    //       })
    //     }
    //   }
    // },

    view (link) {
      console.log(link)
      // window.location.href = link
      // 打开一个新页面
      window.open(link, '_blank')
    },
    handleSizeChange (val) {
      this.pageSize = val
      console.log(`每页 ${val} 条`)
    },
    handleCurrentChange (val) {
      this.currentPage = val
      console.log(`当前页: ${val}`)
    }


  }
}

</script>

<style scoped>
</style>
