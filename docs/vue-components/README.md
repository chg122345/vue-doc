### Vue封装组件说明文档
+ 目录说明
  + EditTable 可编辑动态表格
  + PageDialog 弹框
  + PageFilter 动态渲染输入框
  + PageForm 动态渲染表单
  + PageSteps 步骤条
  + PageTree 动态树型


### 动态可编辑表格

+ 组件依赖Element-ui

+ 界面效果
  ![动态表格](/images/table.png)

+ 使用说明
  + 引入组件
    ```js
    import EditTable from "@/components/EditTable";
    ```
  + 映射成本地标签
    ```js
    components: {
            EditTable
        },
    ```
  + 页面上使用标签
    ```html
    <edit-table :tableHead="tableHead"
                :tableData.sync="tableData"
                :toolBar="toolBar"
                ref="editTable"
                :spanMethod="spanMethod"
                :dataFilter="dataFilter"
                :showAdd="true"
                :checkbox="true"
                :cellStyle="tableRowStyle">
           <template slot-scope="scope">
               <el-button @click="handleClick(scope,'edit')" type="text" size="small">编辑</el-button>
               <el-button  @click="handleClick(scope,'del')" type="text" size="small">删除</el-button>
           </template>
       </edit-table>
    ```
+ 参数说明

| 参数 | 说明 | 类型 | 默认值 | 必选项 |
| :------: | :------: | :------: | :------: | :------: |
| tableHead | 表头数据 | Array | - | yes |
| tableData | 要渲染的数据 | Array | - | - |
| toolBar | 是否显示操作栏 | Boolean | true | - |
| toolBarWidth | 操作栏宽度 | Number or String | - | - |
| checkbox | 是否显示选择框 | Boolean | false | - |
| showAdd | 是否显示表格下方的新增按钮 | Boolean | false | - |
| enableEdit | 是否启用编辑 | Boolean | true | - |
| pageInfo | 分页信息 | Object | - | - |
| dataFilter | 表格筛选条件 | Object | - | - |
| cellStyle | 单元格样式 | Function | - | - |
| spanMethod | 单元格合并 | Function | - | - |

+ 方法说明

| 方法名 | 说明 | 参数 |
| :------: | :------: | :------: |
| addRow | 新增行 | - |

+ 事件说明

| 方法名 | 说明 | 参数 |
| :------: | :------: | :------: |
| handleCurrentChange | 翻页事件，参数（当前翻到的页数） | val（Number） |
| handleSelectionChange | 选择框事件，参数（当前选中的表格数据） | vals(Array) |
| cellClick | 单元格的点击事件，参数（当前点击的单元格行和列的信息） | row, column |

+ `EditTableDemo`
  ```html
  <template>
      <div>
          <edit-table :tableHead="tableHead"
                      :tableData.sync="tableData"
                      :toolBar="toolBar"
                      ref="editTable"
                      :spanMethod="spanMethod"
                      :dataFilter="dataFilter"
                      :showAdd="true"
                      :checkbox="true"
                      :cellStyle="tableRowStyle"
                      @handleSelectionChange="handleSelectionChange"
                      :pageInfo="pageInfo">
              <template slot-scope="scope">
                  <el-button @click="handleClick(scope,'edit')" type="text" size="small">编辑</el-button>
                  <el-button  @click="handleClick(scope,'del')" type="text" size="small">删除</el-button>
                  <el-button  @click="changeFilter" type="text" size="small">过滤</el-button>
              </template>
          </edit-table>
      </div>

  </template>

  <script>
      import EditTable from "@/components/EditTable";
      export default {
          name: "EditTableDemo",
          components: {
              EditTable
          },
          data() {
              return {
                  dataFilter: {
                      name: '王小虎6666',
                  },
                  toolBar: true,
                  pageInfo:{
                      total: 200, // 总记录数
                      offset: 5, // 每页显示多少条
                  },
                  tableHead:[
                      {
                          label:'个人信息',
                          property:'name',
                          width: 100,
                          readonly: true,
                          children: [
                              {
                                  label:'姓名',
                                  property:'name',
                                  width: 100,
                                  // readonly: true
                              },{
                                  label:'年龄',
                                  property:'age',
                                  width: 100,
                                  type: 'number',
                                  children: [
                                      {
                                          label:'姓名',
                                          property:'name',
                                          width: 100,
                                          // readonly: true
                                      },{
                                          label:'年龄',
                                          property:'age',
                                          width: 100,
                                          type: 'number',
                                      },
                                  ]
                              },
                          ]
                      }, {
                          label:'地址',
                          property:'address',
                          width: 250,
                          type: 'text',
                          columnKey: '00001', // 用于标识哪一列 用于单元格点击事件
                      },{
                          label:'日期',
                          property:'date',
                          width: 150,
                          type: 'date',
                          readonly: true,
                          columnKey: '00002', // 用于标识哪一列 用于单元格点击事件
                      },{
                          label:'是否开启',
                          property:'state',
                          width: 150,
                          type: 'checkbox',
                          // readonly: true,
                      },{
                          label:'选择框',
                          property:'select',
                          width: 150,
                          type: 'select',
                          // readonly: true,
                          select: [
                              {
                                  label: '上海',
                                  value: 'shanghai',
                              },
                              {
                                  label: '南昌',
                                  value: 'nanchang',
                              },
                              {
                                  label: '武汉',
                                  value: 'wuhan',
                              },] // key 保持与 property 一致
                      },
                  ],
                  tableData: [{
                      date: '2016-05-02',
                      name: '王小虎6666',
                      age: 19,
                      address: '上海市普陀区金沙江路 1518 弄',
                      select: 'shanghai',
                      state: false

                  }, {
                      date: '2016-05-04',
                      name: '王小虎45',
                      age: 19,
                      address: '上海市普陀区金沙江路 1517 弄',
                      select: 'nanchang',
                      state: true
                  }, {
                      date: '2016-05-01',
                      name: '王小虎333',
                      age: 19,
                      address: '上海市普陀区金沙江路 1519 弄',
                      select: 'nanchang',
                  }, {
                      date: '2016-05-03',
                      name: '王小虎222',
                      age: 19,
                      address: '上海市普陀区金沙江路 1519 弄',
                      select: 'nanchang',
                  }],
              }
          },
          methods: {
              changeFilter() {
                  this.dataFilter = {
                      address: '上海市普陀区金沙江路 1519 弄'
                  }
              },
              handleClick(scope,type) {
                  console.log(scope.data.scope,type)
                  if (type === 'del'){
                      this.tableData.splice(scope.data.scope.$index,1)
                  }
              },
              /**
               *
               * @param rowIndex  行索引
               * @param columnIndex  列索引
               * @returns {number[]}
               */
              spanMethod({rowIndex,columnIndex}) {
                  if (columnIndex === 2) {
                      if (rowIndex % 2 === 0) {
                          return [2,1]  // 合并2行1列
                      } else {
                          return [0,0] // 不合并
                      }
                  }
              },
              handleSelectionChange(vals) {
                  console.log(vals)
              },
              tableRowStyle({rowIndex,columnIndex }) {
                 /* if (rowIndex === 2){
                      return "background-color: green"
                  }*/
                  if (columnIndex === 1) {
                      return "color: red";
                  } else if (columnIndex === 3) {
                      return "color: blue";
                  }
                  return "";
              },
          }
      }
  </script>
  ```

### 弹窗

+ 组件依赖Element-ui

+ 使用说明
  + 引入组件
    ```js
    import PageDialog from "@/components/PageDialog";
    ```
  + 映射成本地标签
    ```js
    components: {
            PageDialog
        },
    ```
  + 页面上使用标签
    ```html
    <page-dialog :show.sync="dialogVisible" :title="title">
        <div slot="content">
            第一层dialog内容
            <div @click="showDialog()"> 弹出第二层dialog</div>
            <page-dialog :show.sync="dialogVisible2" width="50%" :appendToBody="true">
                <div slot="content">
                  第二层dialog内容
                </div>
            </page-dialog>
        </div>
    </page-dialog>
    ```

+ 参数说明

| 参数 | 说明 | 类型 | 默认值 | 必选项 |
| :------: | :------: | :------: | :------: | :------: |
| show | 双向绑定，是否显示弹窗 | Boolean | false | yes |
| title | 标题 | String | - | - |
| width | 宽度 | String | 100% | - |
| appendToBody | 是否加在body上（嵌套弹出窗应为true） | Boolean | false | - |

### 动态输入框

+ 组件依赖Element-ui

+ 界面效果
  ![动态输入框](/images/filter.png)

+ 使用说明
  + 引入组件
    ```js
    import PageFilter from '@/components/PageFilter'
    ```
  + 映射成本地标签
    ```js
    components: {
            PageFilter
        },
    ```
  + 页面上使用标签
    ```html
    <page-filter :query.sync="filterInfo.query"
                 :filterList="filterInfo.list"
                 :listTypeInfo="listTypeInfo"
                 @handleClickBtn="handlePageFilterClickBtn"
                 @handleEvent="handlePageFilterEvent">
    </page-filter>
    ```
+ 参数说明

| 参数 | 说明 | 类型 | 默认值 | 必选项 |
| :------: | :------: | :------: | :------: | :------: |
| labelStatus | 是否显示label | Boolean | false | - |
| className | 自定义类名 | String | - | - |
| query | 参数 | Object | - | - |
| filterList | 渲染列表 | Array | - | - |
| listTypeInfo | 属性类型 | Object | - | - |

+ 事件说明

| 方法名 | 说明 | 参数 |
| :------: | :------: | :------: |
| handleClickBtn | 按钮点击事件 |- |
| handleEvent | @focus绑定的事件 |- |

+ `PageFilterDemo`
  ```html
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
  ```

### 动态表单

+ 组件依赖Element-ui

+ 界面效果
  ![动态表单](/images/form.png)

+ 使用说明
  + 引入组件
    ```js
    import PageForm from "@/components/PageForm";
    ```
  + 映射成本地标签
    ```js
    components: {
            PageForm
        },
    ```
  + 页面上使用标签
    ```html
    <page-form :colCount="4"
               :domains="domains"
               @submit="submit"
               :showBtn="false"
               ref="child"
               space="25%">
    </page-form>
    ```
+ 参数说明

| 参数 | 说明 | 类型 | 默认值 | 必选项 |
| :------: | :------: | :------: | :------: | :------: |
| formId | 表单id（用来区分同一个页面渲染的多个表单） | String | 随机数 | - |
| colCount | 一行排多少列 | Number | 3 | - |
| domains | 渲染表单的数据 | Array | true | - |
| showBtn | 是否显示表单内部按钮 | Boolean | false | - |
| checkbox | 是否显示选择框 | Boolean | false | - |
| showAdd | 是否显示表格下方的新增按钮 | Boolean | true | - |
| space | 每列的间距（可以为百分数 20%，也可以为具体数 100px） | String | 20% | - |

+ 方法说明

| 方法名 | 说明 | 参数 |
| :------: | :------: | :------: |
| submitForm | 表单提交 |- |
| resetForm | 表单数据重置 |- |

+ 事件说明

| 事件名 | 说明 | 参数 |
| :------: | :------: | :------: |
| submit | 表单提交方法触发的提交事件，参数（表单的数据） | val(Object) |

+ `PageFormDemo`
  ```html
  <template>
      <div>
          <page-form :colCount="4" :domains="domains" @submit="submit" :showBtn="false" ref="child" space="25%"></page-form>

          <el-button @click="doChildMethod">点击触发子组件方法</el-button>
      </div>

  </template>

  <script>
      import PageForm from "@/components/PageForm";

      export default {
          name: "PageFormDemo",
          components: {
              PageForm
          },
          data() {
              return {
                  domains: [
                      {
                          label: '邮箱',
                          key: 'email',
                          type: 'text',
                          placeholder: '请输入邮箱',
                          disabled: true,
                          value: '123@qq.con',
                          hidden: true,
                          rules: {
                              required: true, message: '不能为空', trigger: 'blur'
                          }
                      },
                      {
                          label: '姓名',
                          key: 'name',
                          type: 'text',
                          value: '',
                          rules: {
                              required: true, message: '不能为空', trigger: 'blur'
                          }
                      },
                      {
                          label: '年龄',
                          key: 'age',
                          type: 'number',
                          value: '',
                      },
                      {
                          label: '生日生日',
                          key: 'birthday',
                          type: 'date',
                          value: '',
                          placeholder: '请选择日期',
                          rules: {
                              required: true, message: '不能为空', trigger: 'blur'
                          }
                      },
                      {
                          label: '区域区域',
                          key: 'area',
                          type: 'select',
                          value: 'nanchang',
                          disabled: true,
                          option: [
                              {
                                  label: '上海',
                                  value: 'shanghai',
                              },
                              {
                                  label: '南昌',
                                  value: 'nanchang',
                              },
                              {
                                  label: '武汉',
                                  value: 'wuhan',
                              },
                          ],
                          rules: {
                              required: true, message: '不能为空', trigger: 'blur'
                          }
                      },
                      {
                          label: '开启',
                          key: 'open',
                          type: 'switch',
                          value: false,
                      },
                      {
                          label: '年龄',
                          key: 'age',
                          type: 'number',
                          value: '',
                          hidden: {resource: '上海'}
                      },
                      {
                          label: '来源',
                          key: 'resource',
                          type: 'radio',
                          value: '上海',
                          // disabled: true,
                          option: [
                              {
                                  label: '上海',
                                  value: 'shanghai',
                              },
                              {
                                  label: '南昌',
                                  value: 'nanchang',
                              },
                              {
                                  label: '武汉',
                                  value: 'wuhan',
                              },
                          ],
                      },
                      {
                          label: '多选',
                          key: 'more',
                          type: 'checkbox',
                          value: [],
                          // disabled: true,
                          option: [
                              {
                                  label: '上海',
                                  value: 'shanghai',
                              },
                              {
                                  label: '南昌',
                                  value: 'nanchang',
                              },
                              {
                                  label: '武汉',
                                  value: 'wuhan',
                              },
                          ],
                      },
                      {
                          label: '年龄',
                          key: 'age',
                          type: 'number',
                          value: '',
                      },
                      {
                          label: '简介',
                          key: 'desc',
                          type: 'textarea',
                          value: '',
                          placeholder: '请输入',
                          rules: {
                              required: true, message: '不能为空', trigger: 'blur'
                          },
                      },
                  ],
              }
          },
          methods: {
              submit(val) {
                  console.log(val)
              },
              doChildMethod() {
                  this.$refs.child.submitForm()
              }
          }
      }
  </script>
  ```

### 步骤条

+ 界面效果
  ![步骤条](/images/steps.png)

+ 使用说明
  + 引入组件
    ```js
    import PageSteps from "@/components/PageSteps";
    ```
  + 映射成本地标签
    ```js
    components: {
            PageSteps
        },
    ```
  + 页面上使用标签
    ```html
     <page-steps :data="stepList"
                 :space="200">
     </page-steps>
    ```
+ 参数说明

| 参数 | 说明 | 类型 | 默认值 | 必选项 |
| :------: | :------: | :------: | :------: | :------: |
| data | 渲染的数据 | Array | - | yes |
| space | 每列的间距（可以为百分数 20%，也可以为具体数 100，100px） | Number or String  | 100 | - |

+ 事件说明

| 事件名 | 说明 | 参数 |
| :------: | :------: | :------: |
| change | 步骤项点击触发事件，参数（val 当前点击的数据, index当前点击的索引位置） | val（Object）, index（Number） |

+ `PageStepsDemo`
  ```html
  <template>
      <page-steps :data="stepList" :space="200"></page-steps>
  </template>

  <script>
      import PageSteps from "@/components/PageSteps";
      export default {
          name: "PageStepsDemo",
          components: {
              PageSteps
          },
          data() {
              return {
                  stepList: [
                      {
                          finish: true, // 处理过的
                          title: '招标立项'
                      },
                      {
                          finish: true,
                          title: '招标公告'
                      },
                      {
                          finish: true,
                          //active: true, // 当前选中的
                          title: '招标文件 2017-02-16'
                      },
                      {
                          finish: true,
                          title: '入围单位审批 2017-02-19'
                      },
                      {
                          finish: true,
                          title: '发标 2017-02-19'
                      },
                      {
                          finish: true,
                          title: '答疑'
                      },
                      {
                          finish: true,
                          title: '回标'
                      },
                      {
                          finish: true,
                          title: '开标 2017-02-25'
                      },
                      {

                          title: '评标'
                      },
                      {

                          title: '清标'
                      },
                      {

                          title: '二轮开标 2017-02-26'
                      },
                      {

                          title: '定标 2017-02-28'
                      },
                  ],
              }
          }
      }
  </script>
  ```

### 动态树

+ 组件依赖Element-ui

+ 界面效果
![动态树](/images/tree.png)

+ 使用说明
  + 引入组件
    ```js
    import PageTree from "@/components/PageTree";
    ```
  + 映射成本地标签
    ```js
    components: {
            PageTree
        },
    ```
  + 页面上使用标签
    ```html
   <page-tree :treeData="data"
              :checkbox="true"
              ref="pageTree"
              keyField="id"
              @change="change"
              :prop="defaultProps">
    </page-tree>
    ```
+ 参数说明

| 参数 | 说明 | 类型 | 默认值 | 必选项 |
| :------: | :------: | :------: | :------: | :------: |
| keyField | 数据的标识字段名 | String | - | yes |
| treeData | 渲染树的数据 | Array | - | yes |
| prop | 渲染数据对应的字段名 | Object | - | - |
| checkbox | 是否显示选择框 | Boolean | false | - |
| iconClass | 节点的图标类名 | String | - | - |

+ 方法说明

| 方法名 | 说明 | 参数 |
| :------: | :------: | :------: |
| getCheckedNodes | 返回当前所有选中的节点信息 |- |
| getCheckedKeys | 返回当前所有选中的节点标识字段（keyField）信息 |- |
| resetChecked | 取消选中的信息 |- |

+ 事件说明

| 事件名 | 说明 | 参数 |
| :------: | :------: | :------: |
| change | 点击`叶子节点`时传出当前节点的值 | val(Object) |

+ `PageTreeDemo`
  ```html
  <template>
     <div>
         <page-tree :treeData="data" :checkbox="true" ref="pageTree" keyField="id" @change="change" :prop="defaultProps"></page-tree>

         <el-button @click="getNode">点击执行子组件事件</el-button>
     </div>
  </template>

  <script>
      import PageTree from "@/components/PageTree";
      export default {
          name: "PageTreeDemo",
          components: {
              PageTree
          },
          data() {
              return {
                  data: [
                      {
                          id: 1,
                          name: '一级 11',
                          children: [
                              {
                                  id: 4,
                                  name: '二级 1-1',
                                  disabled: true,
                                  children: [
                                      {
                                          id: 9,
                                          name: '三级 1-1-1'
                                      }, {
                                          id: 10,
                                          name: '三级 1-1-2'
                                      }]
                              }]
                      },
                      {
                          id: 2,
                          name: '一级 2',
                          children: [{
                              id: 5,
                              name: '二级 2-1'
                          }, {
                              id: 6,
                              name: '二级 2-2'
                          }]
                      },
                      {
                          id: 3,
                          name: '一级 3',
                          children: [{
                              id: 7,
                              name: '二级 3-1'
                          }, {
                              id: 8,
                              name: '二级 3-2'
                          }]
                      }
                      ],
                  defaultProps: {
                      children: 'children',
                      label: 'name'
                  }
              }
          },
          methods: {
              getNode() {
                  console.log(this.$refs.pageTree.getCheckedKeys())
              },
              // 点击叶子节点时该节点的数据
              change(val){
                  console.log(val)
              }
          }
      }
  </script>
  ```
