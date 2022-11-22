<template>
    <div style="display: flex; justify-content: center;">
        <line-chart :chartData="chartData" style="width:1000px; height: 200px;" />
    </div>
    <button @click="calculateByMethodHord">Метод Хорд</button>
    <button @click="calculatePopolam">Метод пополам</button>
    <button @click="calculateNewton">Метод Ньютона</button>
    <button @click="calculateSimpleIteration">Метод простої ітерації</button>
    <button @click="calculateCombinedMethod">Комбінований метод</button>
    <div style="display: flex;">
        <div style="margin-left:20px">
            <div>Уточнене значення за допомогою методів хорд</div>
            {{ historyOfXn[historyOfXn.length - 1] }}
        </div>
        <div style="margin-left:20px">
            <div>Уточнене значення за допомогою методу пополам</div>
            {{ answerForPopolam }}
        </div>
        <div style="margin-left:20px">
            <div>Уточнене значення за допомогою методу Ньютона </div>
            {{ answerForNewton }}
        </div>
    </div>
    <div style="display: flex; justify-content: center; margin-top: 20px;">
        <div style="margin-left:20px">
            <div>Уточнене значення за допомогою простої ітерації</div>
            {{ answerForIteration }}
        </div>
        <div style="margin-left:20px">
            <div>Уточнене значення за допомогою комбінованого методу</div>
            {{ answerForCombined }}
        </div>
    </div>
    <div style="display: flex ;justify-content: center; margin-top: 20px;">
        <div>
            <div>Кількість ітерацій</div>
            {{ historyOfDiveded.length }}
        </div>
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
        secondPervFunc(x) {
            (Math.cos(x / 5) / 25) - (1 / (4 * x ** (4 / 3)))
        },
        calculateNewXB(b, x) {
            const result = (x - (this.myFunc(x) * (b - x)) / (this.myFunc(b) - this.myFunc(x))).toFixed(4)
            return result
        },
        calculateNewXA(a, x) {
            const result = (x - (this.myFunc(x) * (x - a)) / (this.myFunc(x) - this.myFunc(a))).toFixed(4)
            return Number(result)
        },
        calculateNewXForNewton(x) {
            return x - (this.myFunc(x) / this.pervFunc(x))
        },
        calculateNewton() {
            this.historyOfDiveded = []
            let a = 0.9
            let b = 1
            const x_ = []
            if (this.myFunc(a) * this.secondPervFunc(a) > 0) {
                x_.push(a)
            }
            else {
                x_.push(b)
            }
            for (let i = 0; i < 10000; i++) {
                x_.push(this.calculateNewXForNewton(x_[i]))
                this.historyOfDiveded.push(1)
                if (Math.abs(x_[i] - x_[i + 1]) < this.E) {
                    this.answerForNewton = x_[i + 1].toFixed(4)
                    break
                }
            }
        },
        SforIteration(x) {
            const tau = 0.01
            // const k = Math.abs(this.pervFunc(x)) / 2
            // console.log('k ==', k)
            // const res = x - (this.myFunc(x) / k)
            // console.log('res ===', res)
            const res = x + tau * this.myFunc(x)
            return res
        },
        calculateSimpleIteration() {
            const a = 0.9
            const b = 1
            const x0 = a
            const x_ = [x0]
            if (this.myFunc(a) * this.myFunc(b) > 0) {
                console.log('На даному інтервалі рівняння немає коренів')
                return
            }
            for (let i = 0; i < 10000; i++) {
                x_.push(this.SforIteration(x_[i]))
                if (Math.abs(x_[i] - x_[i + 1]) < this.E) {
                    console.log('answer ===', x_[i])
                    this.answerForIteration = x_[i]
                    break
                }
            }
        },
        calculateNewXForHordInCombined(x, x_) {
            const res = (x * this.myFunc(x_) - x_ * this.myFunc(x)) / (this.myFunc(x_) - this.myFunc(x))
            return res
        },
        calculateNewXForNewtonInCombined(x_) {
            const res = x_ - (this.myFunc(x_) / this.pervFunc(x_))
            return res
        },
        calculateCombinedMethod() {
            this.historyOfDiveded = []
            const a = 0.9
            const b = 1
            const x = [a]
            const x_ = [b]
            for (let i = 0; i < 10000; i++) {
                let newHordValue = this.calculateNewXForHordInCombined(x[i], x_[i])
                let newNewtonValue = this.calculateNewXForNewtonInCombined(x_[i])
                x.push(newHordValue)
                x_.push(newNewtonValue)
                this.historyOfDiveded.push(1)
                if (Math.abs(x[i + 1] - x_[i + 1]) < this.E) {
                    const res = (x[i + 1] + x_[i + 1]) / 2
                    this.answerForCombined = res
                    break
                }
            }
        },
        calcFuncDivededByM1(x, m1) {
            const result = Number((Math.abs(this.myFunc(x)) / m1).toFixed(4))
            return result
        },
        calculatePopolam() {
            this.historyOfDiveded = []
            let a = 0.9
            let b = 1
            let v = true
            let x_ = [(a + b) / 2]
            for (let i = 0; i < 10000; i++) {
                const limits = []
                if (this.myFunc(x_[i]) > 0 || v) {
                    if (x_[i - 1]) {
                        limits.push(...[x_[i], x_[i - 1]])
                    }
                    else {
                        limits.push(...[x_[i], b])
                    }
                }
                if (this.myFunc(x_[i]) < 0) {
                    if (x_[i - 1]) {
                        limits.push(...[x_[i - 1], x_[i]])
                    }
                    else {
                        limits.push(...[a, x_[i]])
                    }
                }
                x_.push((limits[1] + limits[0]) / 2)
                this.historyOfDiveded.push(1)
                if (Math.abs(limits[1] - limits[0]) < this.E) {
                    this.answerForPopolam = x_[i + 1]
                    break
                }
            }
        },
        calculateByMethodHord() {
            this.historyOfDiveded = []
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
        },

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
            answerForPopolam: '',
            answerForNewton: '',
            answerForIteration: '',
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