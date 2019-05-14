<template>
    <div class="big-box">
        <div class="container-box" :style="getWidth">
            <div class="item-line"></div>
            <div class="container">
                <div class="container-item" v-for="(item,index) of data"
                     :key="index"
                     :class="{finish:item.finish,active:item.active}"
                     @click="change(item,index)">
                    <div class="dot"></div>
                    <div class="text">{{item.title}}</div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        name: "PageSteps",
        props: {
            data: {
                type: Array,
                required: true,
            },
            space: {
                type: String | Number,
                default: 100
            }
        },
        computed: {
            getWidth(){
                const length = this.data.length
                const space = this.space
                let width
                if (typeof(space) === 'string') {
                    let comWidth = length*parseFloat(space)
                    if (space.endsWith("%")){
                        if (comWidth > 100){
                            comWidth = 100
                        }
                        width = comWidth+'%'
                    } else {
                        if (comWidth > this.contentWidth){
                            comWidth = this.contentWidth
                        }
                        width = comWidth+'px'
                    }
                } else {
                    let comWidth = length*space
                    if (comWidth > this.contentWidth) {
                        comWidth = this.contentWidth
                    }
                    width = comWidth+'px'
                }
                return {
                    width: width
                }
            }
        },
        data() {
            return {
                contentWidth: document.body.clientWidth,
            }
        },
        watch:{
            contentWidth(val) {
                this.contentWidth = val
            }
        },
        mounted() {
            window.onresize = () =>{ // 定义窗口大小变更通知事件
                this.contentWidth = document.body.clientWidth //窗口宽度
            };
        },
        methods: {
            change(val, index) {
                console.log(val, index)
                this.$emit('change', val, index)
            }
        }
    }
</script>

<style scoped lang="scss">
    .big-box {
        display: flex;
        justify-content: center;
    }
    .container-box {
        position: relative;
        padding: 30px 0;
        overflow-x: auto;
        .item-line {
            position: absolute;
            top: 55px;
            left: 10px;
            width: calc(100% - 20px);
            border-bottom: 4px solid #e2e2e2;
        }
        //伪类画圆
        .container {
            border: 0;
            display: flex;
            align-items: center;
            justify-content: space-between;
            .container-item {
                cursor: not-allowed;
                pointer-events: none;
                display: flex;
                flex-direction: column;
                align-items: center;
                justify-content: center;
                .text {
                    height: 50px;
                    width: 60px;
                    text-align: center;
                    font-size: 14px;
                }
                .dot {
                    width: 50px;
                    height: 50px;
                    background-color: #e2e2e2;
                    border-radius: 50%;
                }
                .dot:after {
                    content: "";
                    display: block;
                    width: 26px;
                    height: 26px;
                    border-radius: 50%;
                    background-color: #9B9C9C;
                    position: relative;
                    top: 12px;
                    left: 12px;
                }
                .dots {
                    width: 50px;
                    height: 50px;
                    background-color: #f8f8f8;
                    border-radius: 50%;
                }
                .dots:after {
                    content: "";
                    display: block;
                    width: 25px;
                    height: 25px;
                    border-radius: 50%;
                    background-color: green;
                    position: relative;
                    top: 12px;
                    left: 13px;
                }
            }
            .active {
                pointer-events: auto;
                cursor: pointer;
                .dot:after {
                    background-color: #E5A852;
                }
            }
            .finish {
                pointer-events: auto;
                cursor: pointer;
                .dot:after {
                    background-color: #87BB3F;
                }

            }
        }

    }
</style>