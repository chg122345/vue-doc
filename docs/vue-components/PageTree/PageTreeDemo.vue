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

<style scoped>

</style>
