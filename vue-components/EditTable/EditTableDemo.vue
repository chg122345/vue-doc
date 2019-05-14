<template>
    <div>
        <edit-table :tableHead="tableHead"
                    :tableData.sync="tableData"
                    :toolBar="toolBar"
                    ref="editTable"
                    :spanMethod="spanMethod"
                    :dataFilter="dataFilter"
                    :showAdd="true"
                    :checkbox="true"
                    :cellStyle="tableRowStyle"
                    @handleSelectionChange="handleSelectionChange">
            <template slot-scope="scope">
                <el-button @click="handleClick(scope,'edit')" type="text" size="small">编辑</el-button>
                <el-button  @click="handleClick(scope,'del')" type="text" size="small">删除</el-button>
                <el-button  @click="changeFilter" type="text" size="small">过滤</el-button>
            </template>
        </edit-table>
    </div>

</template>

<script>
    import EditTable from "@/components/EditTable";
    export default {
        name: "EditTableDemo",
        components: {
            EditTable
        },
        data() {
            return {
                dataFilter: {
                    name: '王小虎6666',
                },
                toolBar: true,
                pageInfo:{
                    total: 200, // 总记录数
                    offset: 5, // 每页显示多少条
                },
                tableHead:[
                    {
                        label:'个人信息',
                        property:'name',
                        width: 100,
                        readonly: true,
                        children: [
                            {
                                label:'姓名',
                                property:'name',
                                width: 100,
                                // readonly: true
                            },{
                                label:'年龄',
                                property:'age',
                                width: 100,
                                type: 'number',
                                children: [
                                    {
                                        label:'姓名',
                                        property:'name',
                                        width: 100,
                                        // readonly: true
                                    },{
                                        label:'年龄',
                                        property:'age',
                                        width: 100,
                                        type: 'number',
                                    },
                                ]
                            },
                        ]
                    }, {
                        label:'地址',
                        property:'address',
                        width: 250,
                        type: 'text',
                        columnKey: '00001', // 用于标识哪一列 用于单元格点击事件
                    },{
                        label:'日期',
                        property:'date',
                        width: 150,
                        type: 'date',
                        readonly: true,
                        columnKey: '00002', // 用于标识哪一列 用于单元格点击事件
                    },{
                        label:'是否开启',
                        property:'state',
                        width: 150,
                        type: 'checkbox',
                        // readonly: true,
                    },{
                        label:'选择框',
                        property:'select',
                        width: 150,
                        type: 'select',
                        // readonly: true,
                        select: [
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
                            },] // key 保持与 property 一致
                    },
                ],
                tableData: [{
                    date: '2016-05-02',
                    name: '王小虎6666',
                    age: 19,
                    address: '上海市普陀区金沙江路 1518 弄',
                    select: 'shanghai',
                    state: false

                }, {
                    date: '2016-05-04',
                    name: '王小虎45',
                    age: 19,
                    address: '上海市普陀区金沙江路 1517 弄',
                    select: 'nanchang',
                    state: true
                }, {
                    date: '2016-05-01',
                    name: '王小虎333',
                    age: 19,
                    address: '上海市普陀区金沙江路 1519 弄',
                    select: 'nanchang',
                }, {
                    date: '2016-05-03',
                    name: '王小虎222',
                    age: 19,
                    address: '上海市普陀区金沙江路 1519 弄',
                    select: 'nanchang',
                }],
            }
        },
        methods: {
            changeFilter() {
                this.dataFilter = {
                    address: '上海市普陀区金沙江路 1519 弄'
                }
            },
            handleClick(scope,type) {
                console.log(scope.data.scope,type)
                if (type === 'del'){
                    this.tableData.splice(scope.data.scope.$index,1)
                }
            },
            /**
             *
             * @param rowIndex  行索引
             * @param columnIndex  列索引
             * @returns {number[]}
             */
            spanMethod({rowIndex,columnIndex}) {
                if (columnIndex === 2) {
                    if (rowIndex % 2 === 0) {
                        return [2,1]  // 合并2行1列
                    } else {
                        return [0,0] // 不合并
                    }
                }
            },
            handleSelectionChange(vals) {
                console.log(vals)
            },
            tableRowStyle({rowIndex,columnIndex }) {
               /* if (rowIndex === 2){
                    return "background-color: green"
                }*/
                if (columnIndex === 1) {
                    return "color: red";
                } else if (columnIndex === 3) {
                    return "color: blue";
                }
                return "";
            },
        }
    }
</script>
