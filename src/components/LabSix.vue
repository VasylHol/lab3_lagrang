<template>
    <div style="display: flex">
        <line-chart :chartData="chartData" style="width:1000px; height: 200px;" />
        <button @click="calculateByMethodHord">Метод Хорд</button>
        <button @click="combinedMethod">Комбінований метод</button>

        <div>Уточнене значення за допомогою методів хорд {{ historyOfXn[historyOfXn.length - 1] }}</div>
        <div>Уточнене значення за допомогою комбінованого методу {{ answerForCombined }}</div>
        <div>Кількість ітерацій {{ historyOfDiveded.length }}</div>
    </div>
</template>

<script>
import LineChart from './LineChart.vue';
export default {
    components: {
        LineChart,
    },
    methods: {
        myFunc(x) {
            const result = Number((Math.sqrt(x) - Math.cos(0.2 * x)).toFixed(4))
            return result
        },
        pervFunc(x) {
            return 1 / (2 * Math.sqrt(x)) + (Math.sin(x / 5)) / 5
        },
        calculateNewXB(b, x) {
            const result = (x - (this.myFunc(x) * (b - x)) / (this.myFunc(b) - this.myFunc(x))).toFixed(4)
            return result
        },
        calculateNewXA(a, x) {
            const result = (x - (this.myFunc(x) * (x - a)) / (this.myFunc(x) - this.myFunc(a))).toFixed(4)
            return Number(result)
        },

        calcFuncDivededByM1(x, m1) {
            const result = Number((Math.abs(this.myFunc(x)) / m1).toFixed(4))
            return result
        },
        niutonMethod() {

        },
        combinedMethod() {
            let xp1 = 1;
            let xp2 = 0.9;
            while (xp2 - xp1 > this.E) {
                let xn1 = xp1 - (xp1 * (xp2 - xp1)) / xp2 - xp1;
                let xn2 = xp2 - xp2 / xp2;
                xp1 = xn1;
                xp2 = xn2;
            }
            console.log((xp1 + xp2) / 2);
            this.answerForCombined = (xp1 + xp2) / 2
            return (xp1 + xp2) / 2;
        },
        calculatePopolam() {

        },
        calculateByMethodHord() {
            const limits = [0.9, 1]
            let usableFuncForX = null
            if (this.myFunc(limits[0]) < 0) {
                usableFuncForX = this.calculateNewXA
                let a = limits[0]
                let x = [limits[1]]
                let m1 = this.pervFunc(x[0])
                for (let i = 0; i < 1000; i++) {
                    const diveded = this.calcFuncDivededByM1(x[i], m1)
                    this.historyOfDiveded.push(diveded)
                    x[i + 1] = usableFuncForX(a, x[i])
                    this.historyOfXn.push(x[i + 1])
                    if (diveded <= this.E) {
                        break
                    }
                }
            }
            else {
                usableFuncForX = this.calculateNewXB
                let b = limits[1]
                let x = [limits[0]]
                let m1 = this.pervFunc(x[0])
                for (let i = 0; i < 1000; i++) {
                    const diveded = this.calcFuncDivededByM1(x[i], m1)
                    this.historyOfDiveded.push(diveded)
                    x[i + 1] = usableFuncForX(b, x[i])
                    if (diveded <= this.E) {
                        break
                    }
                }
            }
        }
    },
    created() {
        this.chartData = {
            labels: Array(3000).fill(1).map((el, index) => index),
            datasets: [{ data: Array(3000).fill(1).map((el, index) => Math.sqrt(index)) }, { data: Array(3000).fill(1).map((el, index) => Math.cos(0, 2 * index)) }]
        }
    },
    data() {
        return {
            chartData: {
                labels: null,
                datasets: null
            },
            E: 0.001,
            answerForCombined: '',
            chartData1: {
                datasets: [{
                    data: [{ x: 2, y: 0.92 }, { y: 0.36, x: 6 }, { y: 0.16, x: 7 }, { y: -0.41, x: 10 }]
                }]
            },
            historyOfDiveded: [],
            historyOfXn: []
        }
    },
}
</script>

<style lang="scss" scoped>

</style>