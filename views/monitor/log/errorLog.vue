<template>
  <div class="app-container">
    <div class="align-center">
      <div>
        来源网站：
      </div>
      <div style="margin: 15px 0;"></div>
      <div>
        <el-checkbox :indeterminate="isIndeterminate" v-model="checkAll" @change="handleCheckAllChange">全选</el-checkbox>
        <div style="margin: 15px 0;"></div>
        <el-checkbox-group v-model="checkedCities" @change="handleCheckedCitiesChange">
          <el-checkbox v-for="city in cities" :label="city" :key="city" style="display:block;">{{city}}</el-checkbox>
        </el-checkbox-group>
      </div>
      <div style="margin: 15px 0;"></div>
      <div>
        <el-button type="primary">提交</el-button>
      </div>
    </div>

  </div>
</template>

<script>
const cityOptions = ['中国证券时报网-今日视点', '上海证券时报-今日头条', '中国证券业协会-通知公告', '中国证券业协会-行业动态',
  '中国结算-公司总部', '中国证监会-证监要闻', '中证网-今日头条', '巨潮资讯', '上海证券交易所上市公司-公告摘要', '中国结算-业界动态',
  '商务部-日常新闻发布', '商务部-发言人谈话', '全国中小企业转让系统-新闻动态', '中国人民银行-新闻发布', '国家统计局-最新发布',
  '深圳证券交易所-本所公告', '深圳证券交易所-上市公司公告'];

export default {
  name: 'ErrorLog',
  data () {
    return {
      checkAll: false,
      checkedCities: ['中国证券时报网-今日视点', '上海证券时报-今日头条', '中国证券业协会-通知公告', '中国证券业协会-行业动态',
        '中国结算-公司总部', '中国证监会-证监要闻', '中证网-今日头条', '巨潮资讯', '上海证券交易所上市公司-公告摘要', '中国结算-业界动态',
        '商务部-日常新闻发布', '商务部-发言人谈话', '全国中小企业转让系统-新闻动态', '中国人民银行-新闻发布'],
      cities: cityOptions,
      isIndeterminate: true
    }
  },
  methods: {
    handleCheckAllChange (val) {
      this.checkedCities = val ? cityOptions : [];
      this.isIndeterminate = false;
    },
    handleCheckedCitiesChange (value) {
      let checkedCount = value.length;
      this.checkAll = checkedCount === this.cities.length;
      this.isIndeterminate = checkedCount > 0 && checkedCount < this.cities.length;
    }

  }
}
</script>

<style scoped>
.align-center {
  margin: 0 auto; /* 居中 这个是必须的，，其它的属性非必须 */
  /* text-align: center; 文字等内容居中 */
  font-size: 20px;
  margin-top: 10px;
  margin-bottom: 15px;
  width: 320px;
  height: 100px;
}
.align-left {
  margin: 0 auto; /* 居中 这个是必须的，，其它的属性非必须 */
  font-size: 20px;
  margin-left: 550px;
}
</style>
