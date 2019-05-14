### 弹窗使用说明文档

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
