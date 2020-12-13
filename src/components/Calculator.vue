<template>
  <div class="calculator">
      <div class="result" style="grid-area: result">
        {{equation}}
      </div>
  
      <button style="grid-area: ac" @click="clear">AC</button>
      <button style="grid-area: plus-minus" @click="calculateToggle">+/-</button>
      <button style="grid-area: percent" @click="calculatePercentage">%</button>
      <button style="grid-area: add" @click="append('+')">+</button>
      <button style="grid-area: subtract" @click="append('-')">-</button>
      <button style="grid-area: multiply" @click="append('×')">×</button>
      <button style="grid-area: divide" @click="append('÷')">÷</button>
      <button style="grid-area: equal" @click="calculate">=</button>
  
      <button style="grid-area: number-1" @click="append(1)">1</button>
      <button style="grid-area: number-2" @click="append(2)">2</button>
      <button style="grid-area: number-3" @click="append(3)">3</button>
      <button style="grid-area: number-4" @click="append(4)">4</button>
      <button style="grid-area: number-5" @click="append(5)">5</button>
      <button style="grid-area: number-6" @click="append(6)">6</button>
      <button style="grid-area: number-7" @click="append(7)">7</button>
      <button style="grid-area: number-8" @click="append(8)">8</button>
      <button style="grid-area: number-9" @click="append(9)">9</button>
      <button style="grid-area: number-0" @click="append(0)">0</button>
  
      <button style="grid-area: dot" @click="append('.')">.</button>
  </div>
</template>

<script>
export default {
  name: 'Calculator',
  data () {
      return {
        equation:'0',
        isDecimalAdded: false,
        isOperatorAdded: false,
        isStarted: false
      }
    },
    methods:{   
         // Check if the character is + / - / × / ÷
        isOperator(character) {
          return ['+', '-', '×', '÷'].indexOf(character) > -1
        },
        // When pressed Operators or Numbers
        append(character) {
            //开始输入第一个字符
            //如果显示为0并且第一个按下的不是运算符，则是有效输入，可以显示
            if (this.equation === '0' && !this.isOperator(character)) {
                //如果第一次按下的是小数点，就保留0，显示0.  否则显示输入的数字，覆盖原来的0
                if (character=== '.'){
                    this.equation += ''+character
                    this.isDecimalAdded=true
                }else{
                    this.equation = ''+character //这里的空字符是为了把character自动转化为字符串类型
                }
                this.isStarted=true
                return
            }
            //输入后续的数字
            if (!this.isOperator(character)){
                if (character=== '.' && this.isDecimalAdded){
                    return
                }
                if (character=== '.'){
                    this.isDecimalAdded=true
                    this.isOperatorAdded=true    //表示输入小数点后不能直接输入运算符
                }else{
                    this.isOperatorAdded=false   //表示可以输入运算符
                }
                this.equation+=''+character
            }

            //输入运算符时候
            if (this.isOperator(character) && !this.isOperatorAdded){
                this.equation+=''+character
                this.isDecimalAdded=false
                this.isOperatorAdded=true
            }

          
        },
        // When pressed '='
        calculate() {
            if (!this.isOperatorAdded){
                let result = this.equation.replace(new RegExp('×', 'g'), '*').replace(new RegExp('÷', 'g'), '/')
      
                this.equation = parseFloat(eval(result).toFixed(9)).toString()
                this.isDecimalAdded = false
                this.isOperatorAdded = false
            }
        
        },
        // When pressed '+/-'
        calculateToggle() {
            if (this.isOperatorAdded || !this.isStarted) {
                return
            }
              
            this.calculate()
            this.equation = ''+ this.equation*-1
        },
        // When pressed '%'
        calculatePercentage() {
            if (this.isOperatorAdded || !this.isStarted) {
                return
            }
            this.calculate()
            this.equation = '' + this.equation * 0.01
          
        },




        // 按下AC复位
        clear() {
            this.equation = '0'
            this.isDecimalAdded = false
            this.isOperatorAdded = false
            this.isStarted = false
          }
    }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.calculator{
  display:grid;
  grid-template-areas: "result result result result"
    "ac plus-minus percent divide"
    "number-7 number-8 number-9 multiply"
    "number-4 number-5 number-6 subtract"
    "number-1 number-2 number-3 add"
    "number-0 number-0 dot equal";
  grid-template-columns: repeat(4,80px);
  grid-template-rows: repeat(6,80px);
  /*对计算器的外框的拟态化设计*/
  box-shadow: -8px -8px 16px -10px rgba(255, 255, 255, 1),
           8px 8px 16px -10px rgba(0, 0, 0, .15);

  padding: 24px;
  border-radius: 20px;
}
.calculator button{
    margin: 8px;
    padding: 0;
    border: 0;
    display:block;
    outline: none;
    border-radius: 40px;
    font-size: 24px;
    font-family: Helvetica;
    color: #999;
    background:linear-gradient(135deg, rgba(230,230,230,1) 0%, rgba(246,246,246,1) 100%);
    box-shadow: -4px -4px 10px -8px rgba(255, 255, 255, 1), 4px 4px 10px -8px rgba(0, 0, 0, .3);
}
/**设置按下的时候按钮的阴影由外部阴影变为内部阴影*/
.calculator button:active{
    box-shadow: -4px -4px 10px -8px rgba(255, 255, 255, 1), 4px 4px 10px -8px rgba(0, 0, 0, .3) inset;
}

.result{
    text-align: right;
    line-height:80px;
    font-size: 48px;
    font-family: Helvetica;
    padding: 0 20px;
    color: #666;

}
</style>
