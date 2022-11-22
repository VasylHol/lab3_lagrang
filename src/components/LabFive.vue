<template>
    <div>
        <button @click="calculateDots">calculateDots</button>
        <line-chart :chart-data="dotsForGraph"></line-chart>
    </div>
</template>

<script>
import LineChart from './LineChart.vue'
export default {
    data() {
        return {
            a: 4,
            b: 5,
            n: 10,
            M: 25,
            N: 50,
            dots: [{ x: 4, y: 240 }],
        }
    },
    components: {
        LineChart,
    },
    computed: {
        dotsForGraph() {
            return {
                labels: this.dots.map((el) => {
                    return el.x
                }),
                datasets: [{ data: this.dots.map((el) => el.y) }]
            }
        }
    },
    methods: {
        myFunc(x) {
            return Number((x ** 4 - x ** 2).toFixed(2))
        },
        calculateDots() {
            for (let i = 0; i < this.n; i++) {
                let ourNewX = Number(this.dots[i].x + 0.1)
                this.dots.push({ x: Number(ourNewX.toFixed(2)), y: this.myFunc(ourNewX) })
            }

            console.log('DOTS', this.dots)
        },
        calculate() {

        }
    },
}
</script>

<style lang="scss" scoped>

</style>