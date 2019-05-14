<template>
  <div>
    <page-filter
    :query.sync="filterInfo.query"
    :filterList="filterInfo.list"
    :listTypeInfo="listTypeInfo"
    @handleClickBtn="handlePageFilterClickBtn"
    @handleEvent="handlePageFilterEvent">
    </page-filter>
  </div>
</template>
<script>
import PageFilter from '@/components/PageFilter'
export default {
  name: "PageFilterDemo",
  components: {
    PageFilter
  },
  data () {
    return {
      // 过滤相关配置
      filterInfo: {
        query: {
          account: '',
          groupInput:{
            field:'no',
            value:''
          },
          createUser: 2,
          createTime:'',
        },
        list: [
          {type: 'input', label: '账户', value: 'account', event: 'accountValChange'},
          {type: 'group-input', label: '供应商编码', value: 'groupInput', fields:[
             {label: '供应商编码', value: 'no'},
             {label: '供方分类', value: 'category'},
             {label: '供方名称', value: 'name'},
             {label: '履约评级', value: 'level'},
          ]},
          {type: 'select', label: '创建人', value: 'createUser', list: 'accountTypeList'},
          {type: 'date', label: '创建时间', value: 'createTime'},
          {type: 'button', label: '搜索', btType: 'primary', icon: 'el-icon-search', event: 'search', show: true},
          {type: 'button', label: '添加', btType: 'primary', icon: 'el-icon-plus', event: 'create', show: true}
        ]
      },
      // 数据项列表
      listTypeInfo: {
        accountTypeList: [
          {label: '管理员', value: 0},
          {label: '普通用户', value: 1},
          {label: '外部用户', value: 2}
        ]
      },
    }
  },
  methods: {
    handlePageFilterEvent (event, data) {
      console.log(event);
      switch (event) {
        // 对表格获取到的数据做处理
        case 'accountValChange':
          console.log('账号关闭离开后执行');
          break
      }
    },
    handlePageFilterClickBtn (event, data) {
      console.log(event);
      switch (event) {
        case 'search':
          console.log(this.filterInfo.query);
          console.log('点击搜索按钮执行...');
          break
        case 'create':
          console.log('点击添加按钮执行...');
          break
      }
    }
  }
}
</script>
<style scoped>

</style>
