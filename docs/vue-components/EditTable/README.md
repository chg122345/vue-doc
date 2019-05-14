### 动态可编辑表格使用说明文档

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
