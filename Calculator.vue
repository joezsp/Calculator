<template>
  <div class="calculator">
    <div class="result">
      <div class="input">{{ inputs }}</div>
      <div class="value">{{ result }}</div>
    </div>
    <table>
      <tr>
        <td @click="input('＋')" class="operator">＋</td>
        <td @click="input('－')" class="operator">－</td>
        <td @click="input('×')" class="operator">×</td>
        <td @click="input('÷')" class="operator">÷</td>
      </tr>
      <tr>
        <td @click="input(7)">7</td>
        <td @click="input(8)">8</td>
        <td @click="input(9)">9</td>
        <td @click="input('back')" class="backspace">⌫</td>
      </tr>
      <tr>
        <td @click="input(4)">4</td>
        <td @click="input(5)">5</td>
        <td @click="input(6)">6</td>
        <td @click="input('clear')" class="clear">C</td>
      </tr>
      <tr>
        <td @click="input(1)">1</td>
        <td @click="input(2)">2</td>
        <td @click="input(3)">3</td>
        <td @click="input('=')" rowspan="2" class="equal">＝</td>
      </tr>
      <tr>
        <td @click="input(0)" colspan="2">0</td>
        <td @click="input('.')">.</td>
      </tr>
    </table>
  </div>
</template>

<script>
export default {
  name: 'Calculator',
  data() {
    return {
      inputs: '',
      result: 0
    }
  },
  methods: {
    input(key) {
      switch (key) {
        case 'back':
          this.backspace()
          break
        case 'clear':
          this.clear()
          break
        case '＋':
        case '－':
        case '×':
        case '÷':
        case '.':
          // 避免重复输入符号
          if (isNaN(this.inputs.charAt(this.inputs.length - 1))) {
            this.inputs = this.inputs.slice(0, this.inputs.length - 1)
          }
          // 避免符号开头
          if (this.inputs.length > 0) {
            this.inputs += key
          }
          break
        case '=':
          this.calc()
          this.inputs = this.result.toString()
          break
        case 0:
          // 避免重复输入0
          if (!this.beforeIsZero()) {
            this.inputs += key
            this.calc()
          }
          break
        default:
          // 避免数字前面是0
          if (this.beforeIsZero()) {
            this.inputs = this.inputs.slice(0, this.inputs.length - 1)
          }
          this.inputs += key
          this.calc()
      }
    },
    backspace() {
      if (this.inputs.length > 0) {
        this.inputs = this.inputs.slice(0, this.inputs.length - 1)
        this.calc()
      }
    },
    clear() {
      this.inputs = ''
      this.result = 0
    },
    calc() {
      let expression = this.inputs
      if (expression.length > 0) {
        expression = expression.replace(/＋/g, '+')
        expression = expression.replace(/－/g, '-')
        expression = expression.replace(/×/g, '*')
        expression = expression.replace(/÷/g, '/')
        try {
          this.result = eval(expression)
        } catch (err) {
          this.result = ''
        }
      } else {
        this.result = 0
      }
    },
    beforeIsZero() {
      let s = this.inputs
      let n = s.length
      if (n < 1 || s.charAt(n - 1) !== '0') return false
      // 查找最后一个运算符的位置
      let p = Math.max(
        s.lastIndexOf('＋'),
        s.lastIndexOf('－'),
        s.lastIndexOf('×'),
        s.lastIndexOf('÷')
      )
      if (p > 0) {
        s = s.slice(p + 1)
      }
      return s.charAt(0) === '0' && s.charAt(1) !== '.'
    }
  }
}
</script>

<style lang="scss" scoped>
.calculator {
  background-color: #3e3f40;
  .result {
    padding: 8px;
    .input {
      font-size: 40px;
      line-height: 50px;
      min-height: 50px;
    }
    .value {
      font-size: 48px;
      line-height: 60px;
      min-height: 60px;
      text-align: right;
    }
  }
  table {
    border-spacing: 1px;
    width: 100%;
    font-size: 30px;
    td {
      cursor: pointer;
      background-color: #818182;
      display: table-cell;
      text-align: center;
      vertical-align: middle;
      width: 25%;
      height: 2.5em;
      line-height: 2.5em;
      &:active {
        opacity: 0.7;
      }
    }
    .operator {
      background-color: #ff9e0b;
    }
    .backspace {
      background-color: #666;
      font-size: 24px;
    }
    .clear {
      background-color: #666;
      color: #ff9e0b;
    }
    .equal {
      background-color: #027be3;
    }
  }
}
</style>
