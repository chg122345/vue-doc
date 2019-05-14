<template>
    <div>
        <el-table-column v-if="enableEdit"
                v-for="item in tableHead"
                         :label="item.label"
                         :property="item.property"
                         :width="item.width"
                         :key="item.label"
                         :column-key="item.columnKey">
            <edit-table-head v-if="item.children && item.children.length"
                             :tableHead="item.children" :enableEdit="enableEdit">
            </edit-table-head>
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
            <edit-table-head v-if="item.children && item.children.length"
                             :tableHead="item.children"
                             :enableEdit="enableEdit">
            </edit-table-head>
        </el-table-column>
    </div>

</template>

<script>
    import AutoTextarea from '../common/AutoTextarea'
    export default {
        name: "EditTableHead",
        components: {AutoTextarea},
        props: {
            tableHead: {
                type: Array,
                required: true,
            },
            enableEdit: {
                type: Boolean,
                default: true,
            }
        },
    }
</script>
