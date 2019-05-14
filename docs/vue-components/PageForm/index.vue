<!-- 待完善 动态表单 -->
<template>
    <el-form :model="dynamicValidateForm" :ref="refName" class="dynamic-form">
        <div class="form-box">
            <div class="form-item"
                 :style="formItemWidth"
                 v-for="(item, i) in formList"
                 :key="i"
                 v-show="formItemHidden(item.hidden)">
                <el-form-item
                        :label="item.label"
                        :key="item.key"
                        :prop="'domains.' + i + '.value'"
                        :rules="item.rules"
                        :style="formItemSpace">
                    <el-date-picker
                            v-if="item.type === 'date'"
                            type="date"
                            :placeholder="item.placeholder"
                            v-model="item.value"
                            :editable="false"
                            :clearable="false"
                            :disabled="item.disabled"
                            style="width: 100%;">
                    </el-date-picker>

                    <el-select
                            v-else-if="item.type === 'select'"
                            v-model="item.value"
                            :placeholder="item.placeholder"
                            :disabled="item.disabled"
                            clearable>
                        <el-option
                                v-for="(itm,index) of item.option"
                                :key="index"
                                :label="itm.label"
                                :value="itm.value">
                        </el-option>
                    </el-select>

                    <el-radio-group
                            v-else-if="item.type === 'radio'"
                            v-model="item.value"
                            :placeholder="item.placeholder"
                            :disabled="item.disabled">
                        <el-radio
                                v-for="(itm,index) of item.option"
                                :key="index"
                                :label="itm.label"
                                :value="itm.value">
                        </el-radio>
                    </el-radio-group>

                    <el-checkbox-group
                            v-else-if="item.type === 'checkbox'"
                            v-model="item.value"
                            :placeholder="item.placeholder"
                            :disabled="item.disabled">
                        <el-checkbox
                                v-for="(itm,index3) of item.option"
                                :key="index3"
                                :label="itm.label"
                                :name="'domains.' + i + '.value'">
                        </el-checkbox>
                    </el-checkbox-group>

                    <el-switch v-else-if="item.type === 'switch'" v-model="item.value"></el-switch>
                    <el-input
                            v-else
                            v-model="item.value"
                            :type="item.type"
                            :placeholder="item.placeholder"
                            :disabled="item.disabled"
                            clearable>
                    </el-input>
                </el-form-item>
            </div>
        </div>
        <div class="form-box"
             v-for="(item,index) of textareaList"
             :key="item.label"
             v-show="formItemHidden(item.hidden)">
            <div class="form-item textarea-box">
                <el-form-item
                        :label="item.label"
                        :key="item.key"
                        :prop="'domains.' + (textareaIndex[index]) + '.value'"
                        :rules="item.rules"
                        :style="textItemSpace">
                    <el-input
                            v-model="item.value"
                            :type="item.type"
                            :placeholder="item.placeholder"
                            :disabled="item.disabled"
                            :rows="4">
                    </el-input>
                </el-form-item>
            </div>
        </div>
        <div>
            <el-form-item v-if="showBtn">
                <el-button type="primary" @click="submitForm">提交</el-button>
                <el-button @click="resetForm">重置</el-button>
            </el-form-item>
        </div>
    </el-form>
</template>

<script>
    export default {
        name: "PageForm",
        props: {
            formId: {
                type: String, // 用来标识表单
                default: Math.random().toString()
            },
            colCount: {
                type: Number, // 一行排多少列
                default: 3
            },
            domains: {
                type: Array, // 表单数据
                required: true
            },
            showBtn: {
                type: Boolean, // 是否显示表单内置按钮
                default: true
            },
            space: {
                type: String,  // 表单单元格间距
                default: "20%"
            }
        },
        data() {
            return {
                dynamicValidateForm: {
                    domains: []
                },
                refName: "",
                textareaIndex: [],
                form: {}
            };
        },
        computed: {
            formList() {
                let arr = this.domains;
                arr = arr.filter(i => i.type !== "textarea");
                return arr;
            },
            textareaList() {
                const arr = this.domains;
                return arr.filter((i, index) => {
                    if (i.type === "textarea") {
                        this.textareaIndex.push(index);
                    }
                    return i.type === "textarea";
                });
            },
            formItemWidth() {
                const width = 100 / this.colCount;
                const obj = {
                    width: width + "%"
                };

                return obj;
            },
            formItemSpace() {
                const obj = {
                    "margin-right": `${this.space}`
                };
                return obj;
            },
            textItemSpace() {
                debugger
                const space = this.space
                let obj = {}
                if (space.endsWith("%")){
                    // const width = 100 / this.colCount;
                    const width = (100 / this.colCount)*(parseFloat(space)/100)
                    obj = {
                        'margin-right': `${width}%`
                    }
                } else {
                    obj = {
                        "margin-right": `${space}`
                    }
                }
                return obj;
            }
        },
        watch: {
            domains: {
                handler(val) {
                    let obj = Object.create(null);
                    val.forEach(item => {
                        if (item.key) {
                            obj[item.key] = item.value;
                        }
                    });
                    this.form = obj;
                },
                deep: true
            }
        },
        created() {
            this.refName = "dynamicForm" + this.formId;
            this.dynamicValidateForm.domains = this.domains;
            let obj = Object.create(null);
            this.domains.forEach(item => {
                if (item.key) {
                    obj[item.key] = item.value;
                }
            });
            this.form = obj;
        },
        methods: {
            /**
             * // 表单验证提交
             */
            submitForm() {
                this.$refs[this.refName].validate(valid => {
                    if (valid) {
                        this.submit();
                    } else {
                        console.log("error submit!!");
                        return false;
                    }
                });
            },
            /**
             * 表单重置
             */
            resetForm() {
                this.$refs[this.refName].resetFields();
            },
            submit() {
                this.$emit("submit", this.form);
            },
            formItemHidden(hidden) {
                if (typeof hidden === "boolean") {
                    return !hidden;
                } else if (hidden instanceof Object) {
                    const key = Object.keys(hidden)[0];
                    const val = hidden[key];
                    return this.form[key] == val;
                }
                return true;
            }
        }
    };
</script>

<style lang="scss" scoped>
    .dynamic-form {
        padding: 10px;
        display: flex;
        flex-direction: column;
        align-items: center;
        .form-box {
            width: 100%;
            display: flex;
            flex-wrap: wrap;
            align-items: center;
            .form-item {
                display: flex;
                justify-content: flex-end;
                align-items: center;
                /deep/ .el-form-item {
                    /*width: 100%;*/
                    display: flex;
                    align-items: center;
                    .el-form-item__label {
                        flex: 1;
                        line-height: 16px;
                    }
                    .el-form-item__content {
                        display: flex;
                        .el-input {
                            .el-input__inner {
                               /*padding-right: 15px;*/
                            }
                            input::-webkit-outer-spin-button,
                            input::-webkit-inner-spin-button{
                                -webkit-appearance: none !important;
                                margin: 0;
                            }
                            input[type="number"]{-moz-appearance:textfield;}
                        }
                        .el-checkbox-group {
                            .el-checkbox {
                                margin-right: 10px;
                            }
                        }
                        .el-radio-group {
                            .el-radio {
                                margin-right: 10px;
                            }
                        }
                    }
                    /deep/ .el-select {
                        width: 100%;
                    }
                }
            }
            .textarea-box {
                width: 100%;
                /deep/ .el-form-item {
                    width: 100%;
                    display: block;
                }
                /deep/ textarea {
                    font-family: "PingFang SC", "Helvetica Neue", Helvetica,
                    "microsoft yahei", arial, STHeiTi, sans-serif;
                }
            }
            .el-date-editor--date {
                /deep/ .el-input__inner {
                    padding: 0 15px 0 30px!important;
                }
            }
            .el-select {
                /deep/ .el-input__inner {
                    padding: 0 30px 0 15px;
                }
            }
        }
    }
</style>
