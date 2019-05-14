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

+ 详细信息查看`EditTableDemo`

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

+ 详细信息查看`PageFilterDemo`

### 动态表单

+ 组件依赖Element-ui

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

+ 详细信息查看`PageFormDemo`

### 步骤条使用说明文档

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

+ 详细信息查看`PageStepsDemo`

### 动态树使用说明文档

+ 组件依赖Element-ui

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

+ 详细信息查看`PageTreeDemo`



