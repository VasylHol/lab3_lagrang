<template>
  <div class="wrapper">
    <div style="margin-right: 10px">
      <div>
        Кількість елемнтів <br>
        <input type="number" name="size" id="size" v-model="size" />
      </div>
      <div>
        <div>x = <br>
          <input type="number" name="number" :id="x" v-model="x" />
        </div>
      </div>
    </div>
    <div>
      <div class="table">
        <div v-for="(el, id) in arr" :key="id" class="cell width">
          <input type="number" name="number" :id="id" v-model="arr1[id]" class="width2" />
        </div>
      </div>
      <div class="table">
        <div v-for="(el, id) in arrY" :key="id" class="cell width">
          <input type="number" name="number" :id="id" v-model="arr2[id]" class="width2" />
        </div>
      </div>
      <button @click="button1" style="margin-top:10px">calculate</button>
      <button @click="button2" style="margin-top:10px">Ньютон</button>
    </div>
    <div style="margin-left: 10px">
      <div>Лагранж Х = {{ LagrangX ? LagrangX : 0 }}</div>
      <div>Ньютон Х = {{ NewtonX ? NewtonX : 0 }}</div>
    </div>
    <div>
      <line-chart :chartData="chartData" />
    </div>
    <div>
    </div>
  </div>
</template>

<script>

import LineChart from './LineChart.vue'
export default {
  name: "TableCustom",
  data() {
    return {
      size: 4,
      arr1: [],
      arr2: [],
      x: 0,
      sum: 0,
      n: 10,
      a: 4,
      b: 5,
      m: 10000,
      max: 0,
      arrGraphX: [],
      arrGraphY: [],
    };
  },
  components: {
    LineChart
  },
  methods: {
    button2() {
      this.size = this.n + 1;

      let arrX = this.myRandomInts(this.n + 1, this.a, this.b);
      arrX.sort((a, b) => a - b);
      let arrY = arrX.map((el) => this.f(el));
      this.arr1 = [...arrX];
      this.arr2 = [...arrY];
      let max = 0;
      this.arrGraphX = []
      this.arrGraphY = []
      for (let i = 0; i < this.m; i++) {
        let el = this.a + ((this.b - this.a) / this.m) * i;
        let num1 = this.f(el);
        let num2 = this.Newton(el);
        let el2 = num1 - num2;
        this.arrGraphX.push(el);
        this.arrGraphY.push(num2);
        if (max < Math.abs(el2)) {
          max = Math.abs(el2);
        }
      }
      this.maxN = max;
    },
    createMatrix(arr) {
      this.arr1 = arr;
    },
    createMatrix2(arr) {
      this.arr2 = arr;
    },
    Newton(x) {
      let arrX = this.arr1;
      let arrY = this.arr2;
      let arrRes = [[...arrY]];
      for (let i = 0; i < this.size - 1; i++) {
        let arrT = [];
        for (let j = 0; j < this.size - i - 1; j++) {
          let el =
            (arrRes[i][j + 1] - arrRes[i][j]) / (arrX[j + 1 + i] - arrX[j]);
          arrT.push(el);
        }
        arrRes.push(arrT);
      }
      let N = 0;
      for (let i = 0; i < arrRes.length; i++) {
        let m = arrRes[i][0];
        for (let j = 0; j < i; j++) {
          m *= x - arrX[j];
        }
        N += m;
      }
      return N;
    },
    Lagrang(x) {
      let sum = 0;
      let arrX = this.arr1;
      let arrY = this.arr2;
      for (let i = 0; i < this.size; i++) {
        let arrX2 = [...arrX];
        arrX2.splice(i, 1);
        let a = 1;
        let b = 1;
        for (let j = 0; j < this.size - 1; j++) {
          a *= x - arrX2[j];
          b *= arrX[i] - arrX2[j];
        }
        sum += arrY[i] * (a / b);
      }
      return sum;
    },
    myRandomInts(quantity, min, max) {
      let arr = [];
      while (arr.length < quantity) {
        var candidateInt = Math.random() * (max - min) + min;
        if (arr.indexOf(candidateInt) === -1) arr.push(candidateInt);
      }
      return arr;
    },
    Cheshibova(i) {
      let el1 = (this.b + this.a) / 2
      let el2 = ((this.b - this.a) / 2) * Math.cos(((2 * i + 1) / (2 * (this.n + 1))) * Math.PI)
      return el1 + el2
    },
    f(x) {
      return Math.log(4 + 5 * Math.E ** x)
    },
    button1() {
      let arrF1 = []
      let arrCheshibova = this.arrCheshibova;
      for (let i = 0; i < this.n + 1; i++) {
        arrF1.push(this.f(arrCheshibova[i]))
      }
      this.arr1 = [...arrCheshibova];
      this.arr2 = [...arrF1];
      this.size = this.n + 1;
      let max = 0;
      this.arrGraphX = []
      this.arrGraphY = []
      for (let i = 0; i < this.m; i++) {
        let el = this.a + ((this.b - this.a) / this.m) * i
        let num1 = this.f(el);
        let num2 = this.Lagrang(el)
        let el2 = num1 - num2;
        this.arrGraphX.push(el);
        this.arrGraphY.push(num2);
        if (max < Math.abs(el2)) {
          max = el2
        }
      }
      this.max = max
    }
  },
  computed: {
    chartData() {
      return {
        labels: ['January', 'February', 'March'],
        datasets: [{ data: [40, 20, 12] }]
      }
    },
    arrGrafY2() {
      return this.arrGraphX.map((el) => this.f(el))
    },
    arr() {
      let arr = [];
      for (let i = 0; i < this.size; i++) {
        arr.push(this.arr1[i] ?? 0);
      }
      this.createMatrix(arr);
      return arr;
    },
    LagrangX() {
      return this.Lagrang(this.x);
    },
    NewtonX() {
      return this.Newton(this.x);
    },
    arrY() {
      let arr2 = [];
      for (let i = 0; i < this.size; i++) {
        arr2.push(this.arr2[i] ?? 0);
      }
      this.createMatrix2(arr2);
      return arr2;
    },
    arrCheshibova() {
      let arr = [];
      for (let i = 0; i < this.n + 1; i++) {
        arr.push(this.Cheshibova(i))
      }
      return arr;
    },
    sumEl() {
      return this.arr1
        .reduce(function (a, b) {
          return Number(a) + Number(b);
        });
    },
    sumEl2() {
      return this.arr2
        .reduce(function (a, b) {
          return Number(a) + Number(b);
        });
    },

  },
};
</script>
<style>
.wrapper {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 1000px;
}

.table {
  margin-top: 5px;
  display: flex;
  justify-content: center;
}

.cell {
  border: 2px solid red;
  margin: 2px
}

.width {
  max-width: 100px;
}

.width2 {
  max-width: 90px;
}
</style>