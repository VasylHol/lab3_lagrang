<template>
    <div>
        <div>
            <div>
                <label for="a">а = </label>
                <input type="number" v-model="a" id="a" />
            </div>
            <div>
                <label for="b">b = </label>
                <input type="number" v-model="b" id="b" />
            </div>
            <div>
                <label for="E">E = </label>
                <input type="number" v-model="E" id="E" />
            </div>
        </div>
        <div style="margin-top: 30px; margin-bottom: 30px;">
            <button @click="rectangle" style="margin-right:10px">Прямокутники</button>
            <button @click="trapezium" style="margin-right:10px">Трапеції</button>
            <button @click="Simpson" style="margin-right:10px">Сімпсон</button>
        </div>
        <div>Наближєний інтеграл = {{ I }}, n = {{ n }}</div>
    </div>
</template>
  
<script>
export default {
    name: "LabFour",
    data() {
        return {
            a: 5,
            b: 15,
            n: 10000,
            E: 0.001,
            I: 0,
        };
    },
    methods: {
        f(x) {
            return 5 * (x ** 4)
        },
        rectangle() {
            let s = 0;
            this.n = 4;

            let h = (this.b - this.a) / this.n;
            for (let i = 0; i < this.n; i++) {
                s += this.f(this.a + h / 2 + i * h) * h;
            }
            let s1 = 0;

            while (Math.abs(s - s1) > this.E && this.n < 10000000) {
                s1 = s;
                s = 0;
                this.n *= 2;
                let h = (this.b - this.a) / this.n;
                for (let i = 0; i < this.n; i++) {
                    s += this.f(this.a + h / 2 + i * h) * h;
                }
            }
            this.I = s;
        },
        trapezium() {
            let s = 0;
            this.n = 2;

            let h = (this.b - this.a) / this.n;
            for (let i = 0; i < this.n; i++) {
                s += this.f(this.a + h / 2 + i * h) * h;
            }
            let s1 = 0;

            while (Math.abs(s - s1) > this.E && this.n < 10000000) {
                s1 = s
                s = 0;
                this.n *= 2;
                let h = (this.b - this.a) / this.n;
                for (let i = 1; i < this.n + 1; i++) {
                    s += ((this.f(this.a + (i - 1) * h) + this.f(this.a + i * h)) * h) / 2;
                }
            }
            this.I = s;
        },
        Simpson() {
            let s = 0;
            this.n = 4;

            s = (this.f(this.a) + this.f(this.b)) * 0.5
            let h = (this.b - this.a) / this.n;
            for (let i = 1; i < this.n; i++) {
                let x1 = this.a + h * i
                let x2 = this.a + h * (i - 1)
                s += this.f(x1) + 2 * this.f((x2 + x1) / 2);
            }
            let x = this.a + h * this.n;
            let x1 = this.a + h * (this.n - 1);
            s += 2 * this.f((x1 + x) / 2);
            s *= h / 3
            let s1 = 0;

            while (Math.abs(s - s1) > this.E && this.n < 10000000) {
                this.n *= 2;
                s1 = s
                s = (this.f(this.a) + this.f(this.b)) * 0.5
                let h = (this.b - this.a) / this.n;
                for (let i = 1; i < this.n; i++) {
                    let x1 = this.a + h * i
                    let x2 = this.a + h * (i - 1)
                    s += this.f(x1) + 2 * this.f((x2 + x1) / 2);
                }
                let x = this.a + h * this.n;
                let x1 = this.a + h * (this.n - 1);
                s += 2 * this.f((x1 + x) / 2);
                s *= h / 3
            }
            this.I = s;
        }
    },
    computed: {
    },
};
</script>
<style>
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
  