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
