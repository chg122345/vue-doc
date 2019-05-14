### 动态表单使用说明文档

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
