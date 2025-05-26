<template>
    <div class="calculator">
        <div class="display">{{ currentDisplay }}</div>

        <div @click="clear" class="btn">C</div>
        <div @click="sign" class="btn">+/-</div>
        <div @click="percent" class="btn">%</div>
        <div @click="setOp('/')" class="btn operator">/</div>

        <div @click="append('7')" class="btn">7</div>
        <div @click="append('8')" class="btn">8</div>
        <div @click="append('9')" class="btn">9</div>
        <div @click="setOp('*')" class="btn operator">X</div>

        <div @click="append('4')" class="btn">4</div>
        <div @click="append('5')" class="btn">5</div>
        <div @click="append('6')" class="btn">6</div>
        <div @click="setOp('-')" class="btn operator">-</div>

        <div @click="append('1')" class="btn">1</div>
        <div @click="append('2')" class="btn">2</div>
        <div @click="append('3')" class="btn">3</div>
        <div @click="setOp('+')" class="btn operator">+</div>

        <div @click="append('0')" class="btn zero">0</div>
        <div @click="append('.')" class="btn">.</div>
    </div>
</template>

<script>
export default {
    data() {
        return {
            frist: '',
            second: '',
            op: null,
            waiting: false,
        };
    },

    computed: {
        currentDisplay() {
            if (this.op && this.second !== '') {
                return this.calculate();
            } else {
                return this.waiting ? this.second : this.frist || '0';
            }
        }
    },

    methods: {
       append(char) {
        if (char === '.' && ((this.waiting ? this.second : this.frist).includes('.'))) {
            return;
        }
        if (!this.waiting) {
            this.frist += char;
        } else {
            this.second += char;
        }
       },
       setOp(o) {
        if (this.op && this.second !== '') {
            const res = this.calculate();
            this.frist = String(res);
            this.second = '';
        }
        this.op = o;
        this.waiting = true;
       },
       calculate() {
        const n1 = parseFloat(this.frist);
        const n2 = parseFloat(this.second);
        if (isNaN(n1) || isNaN(n2)) {
            return this.frist;
        }
        switch (this.op) {
            case '+':
                return n1 + n2;
            case '-':
                return n1 - n2;
            case '*':
                return n1 * n2;
            case '/':
                return n2 === 0 ? 'Error' : n1 / n2;
            default:
                return this.frist;
        }   
       },
         clear() {
          this.frist = '';
          this.second = '';
          this.op = null;
          this.waiting = false;
         },
         sign() {
            if (this.waiting) {
                this.second = String(-parseFloat(this.second) || 0);
            } else {
                this.frist = String(-parseFloat(this.frist) || 0);
            }
         },
         percent() {
            if (this.waiting) {
                this.second = String(parseFloat(this.second) / 100);
            } else {
                this.frist = String(parseFloat(this.frist) / 100);
            }
         },
    },
}
</script>

<style scoped>

    .calculator {
        width: 300px;
        margin: 50px auto;
        font-size: 40px;
        display: grid;
        grid-template-columns: repeat(4, 1fr);
        grid-auto-rows: minmax(50px, auto);
        box-shadow: 0 0 10px rgba(0, 0, 0, 0.8);
    }

    .display {
        grid-column: 1 / 5;
        background-color: #333;
        color: white;
        text-align: right;
        padding: 10px;
        height: 80px;
    }

    .zero {
        grid-column: 1 / 4;
    }

    .btn {
        background-color: #f2f2f2;
        border: 1px solid #999;
        border-radius: 0;
    }

    .operator {
        background-color: #f90;
        color: white;
    }
</style>