<template>
    <div class="edit-table-box">
        <el-table border :data="filterData" style="width: 100%"
                  :cell-style="cellStyle"
                  :span-method="spanMethod"
                  @cell-click="cellClick"
                  @selection-change="handleSelectionChange">
            <el-table-column v-if="checkbox" type="selection" width="55"></el-table-column>
            <el-table-column label="序号" type="index"></el-table-column>
            <el-table-column v-if="enableEdit"
                            v-for="item in tableHead"
                             :label="item.label"
                             :property="item.property"
                             :width="item.width"
                             :key="item.label"
                             :column-key="item.columnKey">
                    <edit-table-head v-if="item.children && item.children.length"
                                     :tableHead="item.children"
                                     :enableEdit="true"/>
                <template slot-scope="scope">
                        <auto-textarea v-if="!item.type || item.type ==='text'"
                                       v-model="scope.row[scope.column.property]"
                                       :readonly="item.readonly"/>
                        <el-input type="number"
                                  v-else-if="item.type === 'number'"
                                  v-model="scope.row[scope.column.property]"
                                  :readonly="item.readonly"/>
                        <el-date-picker v-else-if="item.type === 'date'"
                                        v-model="scope.row[scope.column.property]"
                                        :editable="false"
                                        prefix-icon="-"
                                        :clearable="false"
                                        :readonly="item.readonly"/>
                        <el-select v-else-if="item.type === 'select'"
                                   v-model="scope.row[scope.column.property]"
                                   :disabled="item.readonly">
                            <el-option v-for="(itm,index) of item[scope.column.property]" :key="index"
                                       :label="itm.label"
                                       :value="itm.value">
                            </el-option>
                        </el-select>
                        <el-checkbox v-else-if="item.type === 'checkbox'"
                                     v-model="scope.row[scope.column.property]"
                                     :disabled="item.readonly">
                        </el-checkbox>
                        <!--<el-upload v-else-if="item.type === 'upload'"
                                   class="upload-box"
                                   ref="fileUpload"
                                   :action="item.action"
                                   :on-change="handleFileChange"
                                   :auto-upload="false"
                                   :multiple="false"
                                   :show-file-list="false"
                                   :disabled="item.readonly">
                            <div class="text-box">
                                <i class="el-icon-upload"></i>
                                <el-button size="small" type="text">点击上传</el-button>
                            </div>
                        </el-upload>-->
                    </template>
            </el-table-column>
            <el-table-column v-if="!enableEdit"
                             v-for="item in tableHead"
                             :label="item.label"
                             :property="item.property"
                             :width="item.width"
                             :key="item.label"
                             :column-key="item.columnKey">
                <edit-table-head v-if="item.children && item.children.length" :tableHead="item.children"/>
                <template slot-scope="scope">
                  {{scope.row[scope.column.property]}}
                </template>
            </el-table-column>
            <el-table-column label="操作" v-if="toolBar" :width="toolBarWidth">
                <template slot-scope="scope">
                    <slot :data="{scope}"></slot>
                </template>
            </el-table-column>
        </el-table>
        <div class="add-hang" v-if="showAdd" @mouseenter="flag=true" @mouseleave="flag=false" @click="addRow">
            <i class="iconfont icon-jia" :class="{active:flag}">&#xe616;</i>
            <span v-show="flag" :class="{active:flag}">新增行</span>
        </div>
        <div class="page-box" v-if="pageInfo && tableData.length">
            <el-pagination layout="prev, pager, next,slot,jumper"
                           :total="pageInfo.total"
                           :page-size="pageInfo.offset"
                           @current-change="handleCurrentChange">
                <span class="page">共{{getPages(pageInfo.total,pageInfo.offset)}}页</span>
            </el-pagination>
            <el-button type="primary" size="mini">
                确定
            </el-button>
        </div>
    </div>
</template>

<script>
    import AutoTextarea from '../common/AutoTextarea'
    import EditTableHead from './EditTableHead'
    export default {
        name: "EditTable",
        components: {
            AutoTextarea,EditTableHead
        },
        props: {
            checkbox: {
                type: Boolean, // 是否显示多选框
                default: false
            },
            showAdd: {
                type: Boolean, // 是否显示新增行按钮
                default: false
            },
            enableEdit: {
                type: Boolean, // 是否启用可编辑表格
                default: true
            },
            tableHead: {
                type: Array, // 表头数据
                required: true
            },
            tableData: {
                type: Array, // 表体数据
                default: []
            },
            toolBar: {
                type: Boolean, // 是否显示操作栏
                default: true
            },
            toolBarWidth: {
                type: Number | String,  // 操作栏的宽度
            },
            pageInfo: {
                type: Object,  // 分页信息
                // default: () =>({
                //     total: 10, // 总记录数
                //     offset: 10, // 每页显示多少条
                // })
            },
            cellStyle: {
                type: Function // 单元格的样式 (参照element-ui)
            },
            spanMethod: {
                type: Function  // 单元格合并 (参照element-ui)
            },
            dataFilter: {
                type: Object,  // 数据过滤条件
            },
        },
        data() {
            return {
                flag: false,
                filterData: this.tableData,
            }
        },
        watch:{
            dataFilter:{
                handler(val){
                    const keys = Object.keys(val)
                    this.filterData = this.filterData.filter(i =>{
                        let flag = true
                        for (let j = 0; j < keys.length; j++) {
                            if (i[keys[j]] !== val[keys[j]]) {
                                flag = false
                                break
                            }
                        }
                        return flag
                    })
                },
                deep:true
            },
            filterData: {
                handler(val){
                    this.$emit('update:tableData',val)
                },
                deep:true
            },
             tableData: {
                handler(val){
                    this.filterData = val
                },
                deep:true
            }
        },
        methods: {
            addRow() {
                this.tableData.push({})
            },
            /**
             * 跳页事件
             * @param val
             */
            handleCurrentChange(val) {
                console.log(`当前页: ${val}`);
                this.$emit('handleCurrentChange', val)
            },
            /**
             * 多选框事件
             * @param val
             */
            handleSelectionChange(val) {
                this.$emit('handleSelectionChange', val)
            },
            /**
             * 单元格点击事件
             * @param row
             * @param column
             */
            cellClick(row, column) {
                console.log(row, column)
                this.$emit('cellClick',row, column)
            },
            handleFileChange(val) {
                console.log(val)
            },
            handleFileSubmit(){
                console.log(this.$refs.fileUpload.submit())
                // this.$refs.upload.submit()
            },
            getPages(total,offset) {
                if (total % offset === 0){
                    return total/offset
                } else {
                    return parseInt(total/offset) + 1
                }
            }
        }
    };
</script>

<style scoped lang="scss">
    .edit-table-box {
        .table-title {
            background-color: #4b9ad7;
            height: 25px;
            color: white;
            width: 150px;
        }
        //修改ele原生样式
        .el-table /deep/ thead {
            th {
                background-color: #4b9ad7;
                font-weight: normal;
                color: white;
                text-align: center;
            }
        }
        /deep/ .el-table__row {
            .el-input {
                width: 100%;
                height: 100%;
                .el-icon-arrow-up {
                    display: none;
                }
                input::-webkit-outer-spin-button,
                input::-webkit-inner-spin-button{
                    -webkit-appearance: none !important;
                    margin: 0;
                }
                input[type="number"]{-moz-appearance:textfield;}
            }
            .is-disabled {
                .el-input__inner {
                    color: #606266;
                    cursor: text;
                }
            }
            /deep/ .el-input__inner {
                width: 100%;
                padding: 0;
                border: none;
                background: none;
                text-align: center;
            }
            .el-date-editor {
                .el-input__inner {
                    cursor: pointer;
                }
            }
            .el-checkbox {
                .is-disabled {
                    .el-checkbox__inner {
                        cursor: pointer;
                    }
                }
            }
        }
        .el-table /deep/ tbody .el-table__row td {
            padding: 5px 0;
            text-align: center;
        }
        .el-table /deep/ .el-table__body {
            cursor: pointer;
        }
        .upload-box {
            /deep/ .el-upload {
                display: flow-root;
            }
            /deep/ .el-upload--text {
                width: 100%;
                height: 100%;
                border: none;
                background: none;
                .text-box {
                    .el-icon-upload {
                        font-size: 20px;
                        color: #4b9ad7;
                        vertical-align: middle;
                        margin: 0;
                    }
                }
            }
        }
    }

    .page-box {
        display: flex;
        justify-content: center;
        align-items: center;
    }

    .add-hang {
        font-size: 14px;
        display: flex;
        justify-content: center;
        border: 1px solid #e8e8e8;
        border-top: none;
        flex: 1;
        padding: 10px;
        text-align: center;
        cursor: pointer;
        .icon-jia {
            margin-right: 10px;
            font-size: 14px;
            margin-top: 2px;
        }
        .active {
            color: #7bbbf1;
            display: block;
        }
    }
</style>