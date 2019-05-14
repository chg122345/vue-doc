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
