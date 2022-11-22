<template>
    <Line :chart-data="chartData" width="100px" height="100px"/>
</template>

<script>
import { Line } from 'vue-chartjs'
import { Chart as ChartJs, LineElement, CategoryScale, LinearScale, PointElement } from 'chart.js'
ChartJs.register(LineElement, CategoryScale, LinearScale, PointElement)
export default {
    props: {
        chartData: { type: Object, default: () => { }, require: true },
    },
    components: {
        Line,
    },
    // data() {
    //     return {
    //         // chartData: {
    //         //     labels: ['January', 'February', 'March'],
    //         //     datasets: [{ data: [40, 20, 12] }]
    //         // },
    //     }
    // },
    created(){
        console.log('chartData ===',this.chartData)
    }
}
</script>

<style lang="scss" scoped>

</style>