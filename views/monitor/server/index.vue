<template>

  <div class="app-container">

    <!-- 表单渲染 -->
    <el-dialog title="修改优先级间隔时间" :visible.sync="priorityFormVisible">
      <el-form :model="priorityform">
        <el-form-item label="高级:" prop="high" label-width="80px">
          <el-input v-model="priorityform.high" autocomplete="off" style="width: 450px" type="number"></el-input>
          分钟（整数，最小为1）
        </el-form-item>
        <el-form-item label="中级:" prop="middle" label-width="80px">
          <el-input v-model="priorityform.middle" autocomplete="off" style="width: 450px" type="number"></el-input>
          分钟（整数）
        </el-form-item>
        <el-form-item label="低级:" prop="low" label-width="80px">
          <el-input v-model="priorityform.low" autocomplete="off" style="width: 450px" type="number"></el-input>
          分钟（整数）
        </el-form-item>

      </el-form>
      <div slot="footer" class="dialog-footer">
        <el-button @click="priorityFormVisible = false">取 消</el-button>
        <el-button type="primary" @click="priorityFormVisible = false">确 定</el-button>
      </div>
    </el-dialog>

    <!-- 表单渲染 -->
    <el-dialog title="修改采集网址" :visible.sync="dialogFormVisible">
      <el-form :model="editform">
        <el-form-item label="网站名:" prop="name" :label-width="formLabelWidth">
          <el-input v-model="editform.name" autocomplete="off" :disabled="true"></el-input>
        </el-form-item>
        <el-form-item label="网址:" prop="url" :label-width="formLabelWidth">
          <el-input v-model="editform.url" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item label="采集深度:" prop="depth" :label-width="formLabelWidth">
          <el-input v-model="editform.depth" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item label="媒介:" prop="medium" :label-width="formLabelWidth">
          <el-input v-model="editform.medium" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item label="优先级:" prop="priority" :label-width="formLabelWidth">
          <el-radio-group v-model="editform.priority">
            <el-radio :label="3">高</el-radio>
            <el-radio :label="2">中</el-radio>
            <el-radio :label="1">低</el-radio>
          </el-radio-group>
        </el-form-item>
      </el-form>
      <div slot="footer" class="dialog-footer">
        <el-button @click="dialogFormVisible = false">取 消</el-button>
        <el-button type="primary" @click="dialogFormVisible = false">确 定</el-button>
      </div>
    </el-dialog>

    <!--部门数据-->
    <el-row style="margin-right:10px;float:right;margin-bottom:10px">

      <el-button type="primary" @click.native.prevent="openPriorityForm">优先级时间配置</el-button>
      <el-button type="danger" icon="el-icon-delete" @click="deleteData" circle></el-button>

    </el-row>
    <!--表格渲染-->
    <el-table ref="multipleTable" :data="tableData.slice((currentPage-1)*pageSize,currentPage*pageSize)"
      tooltip-effect="dark" style="width: 100%" @selection-change="handleSelectionChange" size="small" border stripe>
      <el-table-column type="selection" width="55" align="center">
      </el-table-column>
      <el-table-column prop="name" label="网站名称" align="center" show-overflow-tooltip>
      </el-table-column>
      <el-table-column prop="url" label="网址" align="center" show-overflow-tooltip>
        <template slot-scope="scope">
          <a :href="scope.row.url" target="_blank" class="buttonText">{{ scope.row.url }}</a>
        </template>
      </el-table-column>
      <el-table-column prop="depth" label="采集深度" width="100" align="center">
      </el-table-column>
      <el-table-column prop="medium" label="媒介" width="150" align="center">
      </el-table-column>
      <el-table-column prop="priority" label="优先级" width="150" align="center">
        <template slot-scope="scope">
          <el-rate v-model="scope.row.priority" :max=3 score-template="" disabled>
          </el-rate>
        </template>
      </el-table-column>
      <el-table-column label="操作" width="120px" align="center" fixed="right">
        <template slot-scope="scope">
          <el-button size="mini" style="margin-right: 3px;" type="text" @click.native.prevent="openEditForm(scope.row)">
            编辑</el-button>
        </template>
      </el-table-column>
    </el-table>
    <div align="middle" style="margin-top: 10px">
      <!--分页组件-->
      <el-pagination @size-change="handleSizeChange" @current-change="handleCurrentChange" :current-page="currentPage"
        :page-sizes="[2, 5, 10, 20]" :page-size="pageSize" layout="total, sizes, prev, pager, next, jumper"
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
  data () {
    return {
      tableData: [{
        name: '上海证券时报-今日头条',
        url: 'http://company.cnstock.com/lists/tt',
        depth: '1',
        medium: '公告网站',
        priority: 3

      }, {
        name: '中国证券业协会-通知公告',
        url: 'https://www.sac.net.cn/tzgg/',
        depth: '1',
        medium: '公告网站',
        priority: 2
      }, {
        name: '中国证券业协会-行业动态',
        url: 'https://www.sac.net.cn/hyfw/hydt/',
        depth: '1',
        medium: '公告网站',
        priority: 1
      }, {
        name: '中国结算-公司总部',
        url: 'http://www.chinaclear.cn/zdjs/gszb/center_clist.shtml',
        depth: '1',
        medium: '公告网站',
        priority: 3
      }, {
        name: '中国证监会-证监要闻',
        url: 'http://www.csrc.gov.cn/pub/newsite/zjhxwfb/xwdd/',
        depth: '1',
        medium: '公告网站',
        priority: 3
      }, {
        name: '中证网-今日头条',
        url: 'http://www.cs.com.cn/',
        depth: '1',
        medium: '公告网站',
        priority: 3
      }, {
        name: '巨潮资讯',
        url: 'http://www.cninfo.com.cn/',
        depth: '1',
        medium: '公告网站',
        priority: 3
      }],
      multipleSelection: [],
      currentTotal: 7,
      currentPage: 1,
      pageSize: 5,
      dialogFormVisible: false,
      priorityFormVisible: false,
      editform: {
        name: '',
        url: '',
        depth: '',
        medium: '',
        priority: '',
        resource: '',
        desc: ''
      },
      formLabelWidth: '100px',
      priorityform: {
        high: 10,
        middle: 20,
        low: 30,
      }

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
    openEditForm (row) {
      this.dialogFormVisible = true
      console.log(row)
      this.editform = Object.assign({}, row);
      console.log(this.editform)
    },
    openPriorityForm () {
      this.priorityFormVisible = true
    },
    deleteData () {
      console.info(this.multipleSelection);
      console.info(this.$refs.multipleTable.tableData);
      console.info(this.$refs.multipleTable);
      if (this.multipleSelection.length != 0) {

      } else {
        this.$alert('请选择一条记录，执行删除操作', {
          confirmButtonText: '确定'
        });
      }
    },
    toggleSelection (rows) {
      if (rows) {
        rows.forEach(row => {
          this.$refs.multipleTable.toggleRowSelection(row);
        });
      } else {
        this.$refs.multipleTable.clearSelection();
      }
    },
    handleSelectionChange (val) {
      this.multipleSelection = val;
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
