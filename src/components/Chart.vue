<template>
    <div class="wrapper" ref="wrapper">chart</div>
</template>

<script lang="ts">
    import {Component, Prop, Vue, Watch} from 'vue-property-decorator';
    import * as echarts from 'echarts';

    @Component
    export default class Chart extends Vue {
        @Prop() options?: any;
        chart?: any;

        mounted() {
            if (this.options === undefined) {
                return console.error('options 为空');
            }
            this.chart = echarts.init(this.$refs.wrapper as HTMLDivElement);
            this.chart.setOption(this.options);
        }

        @Watch('options')
        onOptionsChange(newValue) {
            this.chart!.setOption(newValue);
        }
    }
</script>

<style scoped lang="scss">
    .wrapper {
        height: 400px;
    }
</style>