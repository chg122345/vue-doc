### 动态输入框使用说明文档

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
