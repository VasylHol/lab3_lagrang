<template>
    <div style="display: flex; justify-content: center;">
        <div>
            <div class="col">
                <label for="a">а</label>
                <input type="number" v-model="a" id="a" />
            </div>
            <div class="col">
                <label for="b">b</label>
                <input type="number" v-model="b" id="b" />
            </div>
            <div class="col">
                <label for="E">E</label>
                <input type="number" v-model="E" id="E" />
            </div>
        </div>
        <div style="margin-left: 20px;">
            <div style="margin-top: 30px; margin-bottom: 30px;">
                <button @click="metRectangle" style="margin-right:10px">Прямокутники</button>
                <button @click="trapezium" style="margin-right:10px">Трапеції</button>
                <button @click="simpsonMet" style="margin-right:10px">Сімпсон</button>
            </div>
            <div>Наближений інтеграл = {{ integral }}, n = {{ n }}</div>
        </div>

    </div>
</template>
  
<script>
export default {
    name: "LabFour",
    data() {
        return {
            a: 0,
            b: 0,
            E: 0.001,
            integral: 0,
            n: 0,
        }
    },
    methods: {
        myFunc(x) {
            return 2 ** x
        },
        metRectangle() {
            let s = 0;
            let s1 = 0
            this.n = 2;
            let h = (this.b - this.a) / this.n
            for (let i = 0; i < this.n; i++) {
                let xi = h / 2 + i * h
                s += this.myFunc(this.a + xi) * h
            }
            while (Math.abs(s - s1) > this.E) {
                s1 = s
                s = 0
                this.n *= 2
                let h = (this.b - this.a) / this.n;
                for (let i = 0; i < this.n; i++) {
                    let xi = h / 2 + i * h
                    s += this.myFunc(this.a + xi) * h;
                }
            }
            this.integral = s;
        },
        simpsonMet() {
            let sum1 = 0
            let sum2 = 0
            this.n = 4
            let h = (this.b - this.a) / this.n
            let x0 = (1 / 2) * this.myFunc(this.a)
            let x1 = (1 / 2) * this.myFunc(this.b)
            for (let i = 1; i <= this.n; i++) {
                let xi = this.a + i * h
                if (i <= this.n - 1) {
                    sum1 += this.myFunc(xi);
                }
                let xi_1 = this.a + (i - 1) * h
                sum2 += this.myFunc((xi + xi_1) / 2)
            }
            let res = h / 3 * (x0 + sum1 + 2 * sum2 + x1)
            this.integral = res
        },
        miniRect() {
            this.integral = (this.b - this.a) * (this.myFunc((this.a + this.b) / 2))
        },
        trapezium() {
            let s = 0;
            this.n = 2;

            let h = (this.b - this.a) / this.n;
            for (let i = 0; i < this.n; i++) {
                s += this.myFunc(this.a + h / 2 + i * h) * h;
            }
            let s1 = 0;

            while (Math.abs(s - s1) > this.E) {
                s1 = s
                s = 0;
                this.n *= 2;
                let h = (this.b - this.a) / this.n;
                for (let i = 1; i < this.n + 1; i++) {
                    s += ((this.myFunc(this.a + (i - 1) * h) + this.myFunc(this.a + i * h)) * h) / 2;
                }
            }
            this.integral = s;
        },
    }
}
    
</script>
<style>
.col {
    display: flex;
    flex-direction: column;
    justify-content: flex-start;
}

.table {
    display: flex;
}

.cell {
    border: solid;
}

.width {
    max-width: 100px;
}

.width2 {
    max-width: 90px;
}
</style>
  