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

<style scoped>

</style>
