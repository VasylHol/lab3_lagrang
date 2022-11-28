<template>
    <div style="display: flex; flex-direction: column">
        <div>
            <label for="x0">x0 = </label>
            <input type="number" name="number" id="x0" v-model="x0" />
        </div>
        <div>
            <label for="y0">y0 = </label>
            <input type="number" name="number" id="y0" v-model="y0" />
        </div>
        <div>
            <label for="n">n = </label>
            <input type="number" name="number" id="n" v-model="n" />
        </div>
        <div>
            <label for="a">a = </label>
            <input type="number" name="number" id="a" v-model="a" />
        </div>
    </div>
    <div>
        <button @click="calculateEuler">Ейлер</button>
        <button @click="calculateRunne2">Рунге кут 2</button>
        <button @click="calculateRunne3">Рунге кут 3</button>
        <button @click="calculateRunne4">Рунге кут 4</button>
    </div>
    <div style="display: flex; justify-content: center; margin-top: 70px;">
        <div style="">
            <div class="table">
                <div v-for="(el, id) in arrY" :key="id" class="cell width">
                    {{ el.toFixed(4) }}
                </div>
            </div>
            <div class="table">
                <div v-for="(el, id) in arrYF" :key="id" class="cell width">
                    {{ el.toFixed(4) }}
                </div>
            </div>
            <div class="table">
                <div v-for="(el, id) in arrYf_Y" :key="id" class="cell width">
                    {{ isNaN(el) ? 0 : Math.abs(el.toFixed(4)) }}
                </div>
            </div>
            <div class="table">
                <div v-for="(el, id) in arrX" :key="id" class="cell width">
                    {{ isNaN(el) ? 0 : el.toFixed(4) }}
                </div>
            </div>
        </div>
    </div>


</template>
  
<script>
export default {
    name: "LabSeven",
    data() {
        return {
            x0: 0,
            y0: 1,
            n: 10,
            a: 1,
            arrX: [],
            arrY: [],
        };
    },
    methods: {
        myFunc(x, y) {
            return x * y ** 3 - y
        },
        y(x) {
            return (1) / Math.sqrt(0.5 * Math.E ** (2 * x) + x + 0.5);
        },
        calculateEuler() {
            this.arrX = [this.x0]
            this.arrY = [this.y0]
            for (let i = 1; i < this.n + 1; i++) {
                let funcRes = this.myFunc(this.arrX[i - 1], this.arrY[i - 1])
                this.arrX[i] = this.arrX[0] + this.h * i;
                console.log('arrX i', this.arrX[i])
                this.arrY[i] = this.arrY[i - 1] + this.h * this.myFunc(this.arrX[i - 1] + this.h / 2, this.arrY[i - 1] + funcRes * this.h / 2)
            }
        },
        runne2(x, y) {
            let k1 = this.myFunc(x, y)
            let k2 = this.myFunc(x + this.h / 2, y + k1 * this.h / 2)
            return y + k2 * this.h
        },
        runne3(x, y) {
            let k1 = this.myFunc(x, y)
            let k2 = this.myFunc(x + this.h / 3, y + k1 * this.h / 3)
            let k3 = this.myFunc(x + (2 / 3) * this.h, y + (2 / 3) * this.h * k2)
            return y + ((k1 + 3 * k3) / 4) * this.h;
        },
        runne4(x, y) {
            let k1 = this.myFunc(x, y);
            let k2 = this.myFunc(x + this.h / 2, y + k1 * (this.h / 2));
            let k3 = this.myFunc(x + this.h / 2, y + k2 * (this.h / 2));
            let k4 = this.myFunc(x + this.h, y + k3 * this.h);

            return y + ((k1 + 2 * k2 + 2 * k3 + k4) / 6) * this.h;
        },

        calculateRunne2() {
            this.arrX = [this.x0]
            this.arrY = [this.y0]
            for (let i = 1; i < this.n + 1; i++) {
                this.arrY[i] = this.runne2(this.arrX[i - 1], this.arrY[i - 1]);
                this.arrX[i] = this.arrX[0] + this.h * i
            }
        },
        calculateRunne3() {
            this.arrX = [this.x0]
            this.arrY = [this.y0]
            for (let i = 1; i < this.n + 1; i++) {
                this.arrY[i] = this.runne3(this.arrX[i - 1], this.arrY[i - 1]);
                this.arrX[i] = this.arrX[0] + this.h * i
            }
        },
        calculateRunne4() {
            this.arrX = [this.x0]
            this.arrY = [this.y0]
            for (let i = 1; i < this.n + 1; i++) {
                this.arrY[i] = this.runne4(this.arrX[i - 1], this.arrY[i - 1]);
                this.arrX[i] = this.arrX[0] + this.h * i
            }
        }

    },
    computed: {
        h() {
            return this.a / this.n;
        },
        arrXF() {
            return new Array(this.n + 1)
                .fill(0)
                .map((el, id) => this.x0 + this.h * id);
        },
        arrYF() {
            return this.arrXF
                .map((el) => this.y(el))
        },
        arrYf_Y() {
            return this.arrXF
                .map((el) => this.y(el))
                .map((el, id) => el - this.arrY[id]);
        }
    },
};
</script>
<style>
.table {
    display: flex;
}

.cell {
    border: 2px solid red;
    margin: 2px;
}

.width {
    max-width: 100px;
}

.width2 {
    max-width: 90px;
}
</style>
  