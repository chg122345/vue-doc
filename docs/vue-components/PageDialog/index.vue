<!-- 待完善 Dialog -->
<template>
    <el-dialog
            class="popup"
            :visible.sync="dialogShow"
            top
            :width="width"
            :show-close="false"
            :append-to-body="appendToBody">
        <div class="popup-box">
            <div class="popup-header" :class="{' has-title':title}">
               <div class="popup-title">{{title}}</div>
                <div class="popup-close-btn" @click="close">
                    <i class="el-icon-close"></i> 关闭
                </div>
            </div>
            <div class="slot-box" :style="{'height':contentHeight+'px'}">
                <slot name="content"></slot>
            </div>
        </div>
    </el-dialog>
</template>

<script>
    export default {
        name: "PageDialog",
        props: {
            show: {
                type: Boolean, // 是否弹出窗口
                required: true
            },
            title: {
                type: String   // 弹出的标题
            },
            width: {
                type: String,  // 弹出的宽度
                default: '100%'
            },
            appendToBody: {
                type: Boolean,  // 是否加在body上  多层弹出嵌套勾选
                default: false
            },
        },
        data() {
            return {
                dialogShow: this.show,
                contentHeight: document.documentElement.clientHeight - 90,
            };
        },
        watch: {
            show(val) {
                this.dialogShow = val;
            },
            dialogShow(val) {
                this.$emit("update:show", val);
            },
            contentHeight(val) {
                console.log(val)
                this.contentHeight = val
            },
        },
        mounted() {
           /* this.$nextTick(() =>{
                this.contentHeight = this.$viewHeight - 90
            })*/
            window.onresize = () =>{ // 定义窗口大小变更通知事件
                this.contentHeight = document.documentElement.clientHeight; //窗口高度
            };
        },
        methods: {
            close() {
                this.dialogShow = false;
            }
        }
    };
</script>

<style lang="scss" scoped>
    .popup {
        display: flex;
        justify-content: center;
        overflow: hidden;
        & /deep/ .el-dialog {
            position: fixed;
            top: 50px;
            width: 100%;
            margin: 0 auto;
            overflow: auto;
            /*height: 100%;*/
        }
        & /deep/ .el-dialog__body {
            padding: 0;
        }
        & /deep/ .el-dialog__header {
            padding: 0;
        }
        & /deep/ .el-dialog--center {
            padding: 0;
        }
    }

    .popup-box {
        background-color: #fff;
        .popup-header {
            /*position: relative;*/
            display: flex;
            align-items: center;
            padding: 10px 0;
            .popup-title {
                flex: 1;
                font-size: 14px;
                text-align: center;
                font-weight: bold;
            }
            .popup-close-btn {
                cursor: pointer;
                /*position: absolute;*/
                /*right: 20px;*/
                /*top: 50%;*/
                /*margin-top: 10px;
                margin-right: 20px;
                float: right;
                transform: translate(0, -50%);*/
                margin-right: 20px;
                background-color: #e9574f;
                padding: 4px 10px;
                border-radius: 3px;
                color: #fff;
                font-size: 12px;
            }
        }
        .has-title {
            background-color: #f2f3f6;
        }
        .slot-box {
            overflow: auto;
        }
    }
</style>