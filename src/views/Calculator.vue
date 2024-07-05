<template>
    <h1 class="display-1 font-bold underline text-center">Calculadora</h1>
    <div class="container p-5">
        <div class="row">
            <div class="col-md-2"></div>
            <div class="col-md-8">
                <div class="card w-100">
                    <div class="card-title text-end p-3 overflow-x-hidden tacos m-3">
                        <div class="overflow-x-hidden"> {{ display }}</div>
                    </div>
                    <div class="card-body">
                        <div class="row mb-3">
                            <div class="col">
                                <button class="btn btn-dark w-100" @click="clear()">C</button>
                            </div>
                            <div class="col">
                                <button class="btn btn-danger w-100" @click="chooseOperation('/')">/</button>
                            </div>
                            <div class="col">
                                <button class="btn btn-danger w-100" @click="chooseOperation('*')">*</button>
                            </div>
                            <div class="col">
                                <button class="btn btn-danger w-100" @click="chooseOperation('-')">-</button>
                            </div>
                        </div>
                        <div class="row mb-3">
                            <div class="col">
                                <button class="btn btn-outline-secondary  w-100" @click="appendNumber('7')">7</button>
                            </div>
                            <div class="col">
                                <button class="btn btn-outline-secondary w-100" @click="appendNumber('8')">8</button>
                            </div>
                            <div class="col">
                                <button class="btn btn-outline-secondary w-100" @click="appendNumber('9')">9</button>
                            </div>
                            <div class="col">
                                <button class="btn btn-danger w-100" @click="chooseOperation('+')">+</button>
                            </div>
                        </div>
                        <div class="row mb-3">
                            <div class="col">
                                <button class="btn btn-outline-secondary w-100" @click="appendNumber('4')">4</button>
                            </div>
                            <div class="col">
                                <button class="btn btn-outline-secondary w-100" @click="appendNumber('5')">5</button>
                            </div>
                            <div class="col">
                                <button class="btn btn-outline-secondary w-100" @click="appendNumber('6')">6</button>
                            </div>
                            <div class="col">
                                <button class="btn btn-danger w-100" style="height: 60px; "
                                    @click="compute()">=</button>
                            </div>
                        </div>
                        <div class="row mb-3">
                            <div class="col">
                                <button class="btn btn-outline-secondary w-100" @click="appendNumber('1')">1</button>
                            </div>
                            <div class="col">
                                <button class="btn btn-outline-secondary w-100" @click="appendNumber('2')">2</button>
                            </div>
                            <div class="col">
                                <button class="btn btn-outline-secondary w-100" @click="appendNumber('3')">3</button>
                            </div>
                            <div class="col">
                                <button class="btn btn-danger w-100" @click="appendNumber('.')">.</button>
                            </div>
                        </div>
                        <div class="row mb-3">
                            <div class="col-9">
                                <button class="btn btn-outline-warning text-dark  w-100" @click="appendNumber('0')">0</button>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
            <div class="col-md-2"></div>
        </div>
    </div>
</template>

<script lang="ts">
import { defineComponent, ref } from 'vue';

export default defineComponent({
    name: 'Calculator',
    setup() {
        const display = ref('');
        const currentOperand = ref('');
        const previousOperand = ref('');
        const operation = ref<string | null>(null);

        const clear = () => {
            display.value = '';
            currentOperand.value = '';
            previousOperand.value = '';
            operation.value = null;
        };

        const appendNumber = (number: string) => {
            if (number === '.' && currentOperand.value.includes('.')) return;
            currentOperand.value += number;
            updateDisplay();
        };

        const chooseOperation = (op: string) => {
            if (currentOperand.value === '') return;
            if (previousOperand.value !== '') {
                compute();
            }
            operation.value = op;
            previousOperand.value = currentOperand.value;
            currentOperand.value = '';
        };

        const compute = () => {
            let computation: number;
            const prev = parseFloat(previousOperand.value);
            const curr = parseFloat(currentOperand.value);

            if (isNaN(prev) || isNaN(curr)) return;

            switch (operation.value) {
                case '+':
                    computation = prev + curr;
                    break;
                case '-':
                    computation = prev - curr;
                    break;
                case '*':
                    computation = prev * curr;
                    break;
                case '/':
                    computation = prev / curr;
                    break;
                default:
                    return;
            }

            currentOperand.value = computation.toString();
            operation.value = null;
            previousOperand.value = '';
            updateDisplay();
        };

        const updateDisplay = () => {
            display.value = currentOperand.value;
        };

        return {
            display,
            clear,
            appendNumber,
            chooseOperation,
            compute
        };
    }
});
</script>

<style scoped>
.tacos {
    background-color: black;
    color: white;
    border-radius: 30px 30px 10px 10px;
    height: 80px;
    font-size: 30px;
    font-family: 'Franklin Gothic Medium', 'Arial Narrow', Arial, sans-serif;
    font-weight: bold;

}
button {
    height: 60px;
    font-size: 30px;
    font-family: 'Franklin Gothic Medium', 'Arial Narrow', Arial, sans-serif;
    font-weight: bold;
    box-shadow: black 3px 3px 3px;
}
.card{
    
    border-radius: 50px;
    box-shadow: rgba(0, 0, 0, 0.414) 10px 3px 10px ;
}
</style>
