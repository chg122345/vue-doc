<template>
    <page-steps :data="stepList" :space="200"></page-steps>
</template>

<script>
    import PageSteps from "@/components/PageSteps";
    export default {
        name: "PageStepsDemo",
        components: {
            PageSteps
        },
        data() {
            return {
                stepList: [
                    {
                        finish: true, // 处理过的
                        title: '招标立项'
                    },
                    {
                        finish: true,
                        title: '招标公告'
                    },
                    {
                        finish: true,
                        //active: true, // 当前选中的
                        title: '招标文件 2017-02-16'
                    },
                    {
                        finish: true,
                        title: '入围单位审批 2017-02-19'
                    },
                    {
                        finish: true,
                        title: '发标 2017-02-19'
                    },
                    {
                        finish: true,
                        title: '答疑'
                    },
                    {
                        finish: true,
                        title: '回标'
                    },
                    {
                        finish: true,
                        title: '开标 2017-02-25'
                    },
                    {
                      
                        title: '评标'
                    },
                    {

                        title: '清标'
                    },
                    {

                        title: '二轮开标 2017-02-26'
                    },
                    {

                        title: '定标 2017-02-28'
                    },
                ],
            }
        }
    }
</script>

<style scoped>

</style>
