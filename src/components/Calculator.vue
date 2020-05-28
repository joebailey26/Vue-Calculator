<style scoped>
  .calculator {
		font-size: 32px;
		width: fit-content;
		display: grid;
		grid-template-columns: 1fr 1fr 1fr 1fr;
		grid-gap: .5em;
		background-color: #C0C0C0;
		padding: 1em;
		border-radius: .5em .5em 1em 1em;
		-moz-box-shadow: inset 0 0 5px 0 #8a8a8a;
    -webkit-box-shadow: inset 0 0 5px 0 #8a8a8a;
    box-shadow: inset 0 0 5px 0 #8a8a8a
	}
	.calculator--top {
		display: grid;
		grid-auto-flow: column;
		grid-column: 1 / 5;
		place-items: center;
		margin-left: -.5em;
		margin-right: -.5em
	}
	.calculator--top p {
		text-transform: uppercase;
		color: #555;
		font-family: sans-serif;
		font-weight: 900;
		margin: 0
	}
	.calculator--top_solar {
		display: grid;
		grid-template-columns: 1fr 1fr 1fr 1fr;
		border: .15em solid grey;
		border-radius: .25em;
		height: 1.66em;
		background-color: #364441;
		grid-gap: .1em;
		justify-self: end
	}
	.solar {
		background-color: #443232;
		width: 40px
	}
	.solar:first-of-type {
		border-radius: .1em 0 0 .1em
	}
	.solar:last-of-type {
		border-radius: 0 .1em .1em 0
	}
	.calculator--screen {
		grid-column: 1 / 5;
		font-family: 'Calculator';
		background-color: #006d59;
		border: .5em solid grey;
		border-radius: .25em .25em .5em .5em;
		margin-left: -.5em;
		margin-right: -.5em;
		display: grid;
		align-content: end;
		height: 140px
	}
	.calculator--screen_value {
		display: grid;
		justify-content: end;
		padding: 0 16px;
		padding-bottom: 8px;
		font-size: 64px;
		line-height: 64px
	}
	.calculator--screen_calc {
		display: grid;
		justify-content: end;
		padding-top: 8px;
		padding-right: 16px;
		font-size: 24px
	}
	.calculator--button {
		border-radius: .25em .25em .5em .5em;
		border-color: #111;
		background-color: #333;
		color: white;
		height: 2em;
		width: 2.33em;
		font-size: inherit;
		line-height: 100%;
		display: grid;
		place-items: center;
		font-family: 'Roboto Mono', monospace
	}
	.calculator--button:focus {
		outline: 0;
		-moz-box-shadow: inset 0 0 10px 0 #000000;
		-webkit-box-shadow: inset 0 0 10px 0 #000000;
		box-shadow: inset 0 0 10px 0 #000000
	}
	.add {
		grid-row: span 2;
		height: 100%
	}
</style>

<template>
  <div class="calculator">
	<div class="calculator--top">
		<p>Casio</p>
		<div class="calculator--top_solar">
			<div class="solar"></div>
			<div class="solar"></div>
			<div class="solar"></div>
			<div class="solar"></div>
		</div>
	</div>
	<div class="calculator--screen">
		<div class="calculator--screen_calc">
			{{equation.join('') || ''}}
		</div>
		<div class="calculator--screen_value">
			{{current || '0'}}
		</div>
	</div>
	<button @click="clear" class="calculator--button clear">
      <span>C</span>
    </button>
    <button @click="sign" class="calculator--button sign">
      <span>+/-</span>
    </button>
    <button @click="percent" class="calculator--button percent">
      <span>%</span>
    </button>
    <button @click="divide" class="calculator--button operator">
      <span>÷</span>
    </button>
    <button @click="append('7')"  class="calculator--button">
      <span>7</span>
    </button>
    <button @click="append('8')" class="calculator--button">
      <span>8</span>
    </button>
    <button @click="append('9')" class="calculator--button">
      <span>9</span>
    </button>
    <button @click="multiply" class="calculator--button operator">
      <span>×</span>
    </button>
    <button @click="append('4')" class="calculator--button">
      <span>4</span>
    </button>
    <button @click="append('5')" class="calculator--button">
      <span>5</span>
    </button>
    <button @click="append('6')" class="calculator--button">
      <span>6</span>
    </button>
    <button @click="subtract" class="calculator--button operator">
      <span>-</span>
    </button>
    <button @click="append('1')" class="calculator--button">
      <span>1</span>
    </button>
    <button @click="append('2')" class="calculator--button">
      <span>2</span>
    </button>
    <button @click="append('3')" class="calculator--button">
      <span>3</span>
    </button>
    <button @click="addition" class="calculator--button operator add">
      <span>+</span>
    </button>
    <button @click="append('0')" class="calculator--button zero">
      <span>0</span>
    </button>
    <button @click="dot" class="calculator--button">
      <span>.</span>
    </button>
    <button @click="equal" class="calculator--button equal operator">
      <span>=</span>
    </button>
  </div>
</template>

<script>
  export default {
    data: function () {
      return {
		equation: [],
		previous: null,
		current: '',
		operator: null,
		operatorClicked: false,
      }
    },
    methods: {
		clear() {
			this.current = ''
			this.equation = []
		},
		sign() {
			if (this.current !== '') {
				this.current = this.current.charAt(0) === '-' ? this.current.slice(1) : `-${this.current}`
			}
		},
		percent() {
			this.current = `${parseFloat(this.current) / 100.0}`
		},
		append(number) {
			if (this.operatorClicked) {
				this.current = ''
				this.operatorClicked = false
			}
			this.current = `${this.current}${number}`
			this.equation.push(number)
		},
		dot() {
			if (this.current.indexOf('.') === -1) {
				this.append('.')
			}
		},
		setPrevious(operator) {
			this.previous = this.current
			this.operatorClicked = true
			this.equation.push(operator)
		},
		divide() {
			this.operator = (a, b) => b / a
			this.setPrevious('÷')
		},
		multiply() {
			this.operator = (a, b) => a * b
			this.setPrevious('×')
		},
		addition() {
			this.operator = (a, b) => a + b
			this.setPrevious('+')
		},
		subtract() {
			this.operator = (a, b) => a - b
			this.setPrevious('-')
		},
		equal() {
			this.current = `${this.operator(
				parseFloat(this.current),
				parseFloat(this.previous),
			)}`
			this.previous = null
			this.equation = []
		},
    }
  }
</script>
