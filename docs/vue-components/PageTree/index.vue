<!-- 待完善 动态树形 -->
<template>
    <div class="tree-box">
        <el-tree
                class="tree"
                :data="treeData"
                :icon-class="iconClass"
                :show-checkbox="checkbox"
                :node-key="keyField"
                ref="tree"
                highlight-current
                :props="prop"
                @node-click="nodeClick">
        </el-tree>
    </div>
</template>

<script>
    export default {
        name: "PageTree",
        props: {
            treeData: {
                type: Array, // 渲染树的数据
                required: true
            },
            prop: {
                type: Object, // 数据的属性对应值
            },
            checkbox: {
                type: Boolean, // 是否显示多选框
                default: false
            },
            iconClass: {
                type: String, // 树图标类名
                default: ''
            },
            keyField: {
                type: String, // 数据的标识  用于getCheckedKeys方法
                required: true
            },
        },
        methods: {
            nodeClick(data, node) {
                if (node.isLeaf) {
                    this.$emit('change', data) // 叶子节点点击传出值
                }
            },
            /**
             * 获取选中的节点
             * @returns {*}
             */
            getCheckedNodes() {
                return this.$refs.tree.getCheckedNodes()
            },
            /**
             * 获取选中的节点key值  （key 为keyField属性的值）
             * @returns {*}
             */
            getCheckedKeys() {
                return this.$refs.tree.getCheckedKeys()
            },
            /**
             * 取消选择
             */
            resetChecked() {
                this.$refs.tree.setCheckedKeys([]);
            }
        }
    }
</script>

<style lang="scss" scoped>
    .tree-box {
        border: 1px solid #d8dce5;
        padding: 10px;
        min-height: 450px;
        min-width: 150px;
    }
</style>