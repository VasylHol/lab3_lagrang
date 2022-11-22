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
        <button @click="button1">Ейлер</button>
        <button @click="button2">Рунге кут 2</button>
        <button @click="button3">Рунге кут 3</button>
        <button @click="button4">Рунге кут 4</button>
    </div>
    <div style="display: flex; justify-content: center; margin-top: 70px;">
        <div style="">
            <div class="table">
                <div v-for="(el, id) in arrY" :key="id" class="cell width">
                    {{ el.toFixed(4) }}
                </div>
            </div>
            <div class="table">
                <div v-for="(el, id) in arrY" :key="id" class="cell width">
                    {{ el.toFixed(4) }}
                </div>
            </div>
            <div class="table">
                <div v-for="(el, id) in arrYf_Y" :key="id" class="cell width">
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
        f(x, y) {
            return x * y ** 3 - y;
        },
        _y(x) {
            return (1) / Math.sqrt(0.5 * Math.E ** (2 * x) + x + 0.5);
        },
        runne2(x, y) {
            let k1 = this.f(x, y);
            let k2 = this.f(x + this.h / 2, y + k1 * (this.h / 2));
            return y + k2 * this.h;
        },
        runne3(x, y) {
            let k1 = this.f(x, y);
            let k2 = this.f(x + this.h / 3, y + k1 * (this.h / 3));
            let k3 = this.f(x + (2 * this.h) / 3, y + 2 * k2 * (this.h / 3));
            return y + ((k1 + 3 * k3) / 4) * this.h;
        },
        runne4(x, y) {
            let k1 = this.f(x, y);
            let k2 = this.f(x + this.h / 2, y + k1 * (this.h / 2));
            let k3 = this.f(x + this.h / 2, y + k2 * (this.h / 2));
            let k4 = this.f(x + this.h, y + k3 * this.h);

            return y + ((k1 + 2 * k2 + 2 * k3 + k4) / 6) * this.h;
        },
        button1() {
            this.arrX = [this.x0];
            this.arrY = [this.y0];
            let y0 = this.y0;
            let x0 = this.x0;
            for (let i = 1; i < this.n + 1; i++) {
                let fi = this.f(x0, y0);
                let y1 = y0 + this.h * this.f(x0 + this.h / 2, y0 + fi * (this.h / 2));
                x0 = this.x0 + this.h * i;
                y0 = y1;
                this.arrX.push(x0);
                this.arrY.push(y0);
            }
        },
        button2() {
            this.arrX = [this.x0];
            this.arrY = [this.y0];
            let y0 = this.y0;
            let x0 = this.x0;
            for (let i = 1; i < this.n + 1; i++) {
                let y1 = this.runne2(x0, y0);
                x0 = this.x0 + this.h * i;
                y0 = y1;
                this.arrX.push(x0);
                this.arrY.push(y0);
            }
        },
        button3() {
            this.arrX = [this.x0];
            this.arrY = [this.y0];
            let y0 = this.y0;
            let x0 = this.x0;
            for (let i = 1; i < this.n + 1; i++) {
                let y1 = this.runne3(x0, y0);
                x0 = this.x0 + this.h * i;
                y0 = y1;
                this.arrX.push(x0);
                this.arrY.push(y0);
            }
        },
        button4() {
            this.arrX = [this.x0];
            this.arrY = [this.y0];
            let y0 = this.y0;
            let x0 = this.x0;
            for (let i = 1; i < this.n + 1; i++) {
                let y1 = this.runne4(x0, y0);
                x0 = this.x0 + this.h * i;
                y0 = y1;
                this.arrX.push(x0);
                this.arrY.push(y0);
            }
        },
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
                .map((el) => this._y(el))
        },
        arrYf_Y() {
            return this.arrXF
                .map((el) => this._y(el))
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
  