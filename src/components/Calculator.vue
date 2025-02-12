<template>
    <div class="px-2 rounded-sm px-2 py-2" style="max-width: 400px; margin: 40px auto; background: #234">
        <div class="w-full rounded-sm mt-2 mb-2 px-2 text-right font-bold text-white">
            {{ calculatorValue || 0 }}
        </div>

        <div class="grid grid-cols-4 gap-1">
            <div v-for="n in calculatorElements" :key="n">
                <div class="text-white text-center mt-1 py-3 bg-vue-dark rounded-md hover:bg-vue-hover"
                    :class="{ 'bg-vue-green': ['C', '*', '/', '-', '%', '='].includes(n) }" @click="action(n)">
                    {{ n }}
                </div>
            </div>
        </div>
    </div>
</template>

<script setup>
import { ref } from 'vue'

// State management using Composition API
const calculatorValue = ref('')
const calculatorElements = [
    'C', '*', '/', '-', '7', '8', '9', '+', '4', '5', '6', '%', '1', '2', '3', '=', '0', '.'
]
let operator = ref(null)
let previousCalculatorValue = ref('')

// Action function that handles button clicks
function action(n) {
    // If the button is a number or a dot, append it to the value
    if (!isNaN(n) || n === '.') {
        calculatorValue.value += n
    }

    // If the button is 'C', clear the calculator value
    if (n === 'C') {
        calculatorValue.value = ''
        operator.value = null
        previousCalculatorValue.value = ''
    }

    // If the button is '%', convert the value to percentage
    if (n === '%') {
        calculatorValue.value = (parseFloat(calculatorValue.value) / 100).toString()
    }

    // If the button is an operator, save it and the current value for the next step
    if (['/', '*', '-', '+'].includes(n)) {
        if (calculatorValue.value) {
            operator.value = n
            previousCalculatorValue.value = calculatorValue.value
            calculatorValue.value = ''
        }
    }

    // If the button is '=', evaluate the expression
    if (n === '=') {
        if (previousCalculatorValue.value && operator.value !== null && calculatorValue.value) {
            calculatorValue.value = calculate(previousCalculatorValue.value, operator.value, calculatorValue.value)
            operator.value = null // Reset operator after calculation
            previousCalculatorValue.value = '' // Reset previous value after calculation
        }
    }
}

// Function to calculate the result of an operation
function calculate(val1, operator, val2) {
    const num1 = parseFloat(val1)
    const num2 = parseFloat(val2)

    switch (operator) {
        case '+':
            return (num1 + num2).toString()
        case '-':
            return (num1 - num2).toString()
        case '*':
            return (num1 * num2).toString()
        case '/':
            if (num2 === 0) return 'Error' // handle division by zero
            return (num1 / num2).toString()
        default:
            return 'Error'
    }
}
</script>

<style scoped>
.bg-vue-dark {
    background-color: #333;
}

.bg-vue-hover:hover {
    background-color: #555;
}

.bg-vue-green {
    background-color: #4caf50;
}
</style>