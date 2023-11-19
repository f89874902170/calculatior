<template>
 <div class="w-full grid-cols-2 grid gap-1">
      <div class="">
        <input type="text" disabled="disabled" v-model="message" class="input input-bordered w-full" />
        <div class="keyboard w-full grid-cols-4 grid gap-1 mt-1">
          <button class="btn btn-accent w-20" @click="addAction('/')">/</button>
          <button class="btn btn-accent w-20" @click="addAction('*')">*</button>
          <button class="btn btn-accent w-20" @click="addAction('+')">+</button>
          <button class="btn btn-accent w-20" @click="addAction('-')">-</button>

          <button class="btn btn-accent w-20" @click="addNumber('1')">1</button>
          <button class="btn btn-accent w-20" @click="addNumber('2')">2</button>
          <button class="btn btn-accent w-20" @click="addNumber('3')">3</button>
          <button class="btn btn-accent w-20" @click="calculate">=</button>

          <button class="btn btn-accent w-20" @click="addNumber('4')">4</button>
          <button class="btn btn-accent w-20" @click="addNumber('5')">5</button>
          <button class="btn btn-accent w-20" @click="addNumber('6')">6</button>
          <button class="btn btn-neutral w-20" @click="remove()">ac</button>

          <button class="btn btn-accent w-20" @click="addNumber('7')">7</button>
          <button class="btn btn-accent w-20" @click="addNumber('8')">8</button>
          <button class="btn btn-accent w-20" @click="addNumber('9')">9</button>
          <button class="btn btn-accent w-20" @click="addNumber('0')">0</button>

          <button class="btn btn-accent w-20" @click="addPi()">π</button>
          <button class="btn btn-accent w-20" @click="addSqrt()">√</button>
          <button class="btn btn-accent w-20" @click="addAction('^')">^</button>
          <button class="btn btn-accent w-20" @click="calculateFactorial()">!</button>
        </div>
      </div>
      <div class="w-80">
        <div v-for="item in history"
          class="grid w-full h-7 border-zinc-950 border rounded bg-success text-accent-content place-content-left px-2">
          {{ item.length > 35 ? item.slice(0, 35) + '...' : item }}</div>
      </div>
    </div>
</template>

<script setup>
import { ref } from 'vue';

 //message: 'Hello friend!',
 const message = ref("hello friends!") ;
 const firstNumb = ref("");
 const secondNumb = ref("");
 const action = ref("");
 const piValue = ref("3.14159");
 const history = ref([]);

 function addNumber (number) {
          if (action.value !== "") {
            secondNumb.value += number;
            message.value = secondNumb.value;
          } else {
            firstNumb.value += number;
            message.value = firstNumb.value;
          }
        };

function addSqrt () {
          if (firstNumb.value !== "") {
            const num = parseFloat(firstNumb.value);
          if (num < 0) {
            message.value = "нельзя вычислять корень из отрицательного числа";
          } else {
            message.value = Math.sqrt(num).toString();
            addHistory(`√ ${firstNumb.value} = ${message.value}`);
            firstNumb.value = message.value;
            }
          }
        };
function calculateFactorial () {
          let num = parseInt(firstNumb.value);
          let result = 1;
          if (num < 0) {
            message.value = "факториал используют только для положительных чисел";
          } else {
            for (let i = 2; i <= num; i++) {
              result *= i;
            }
            message.value = result.toString();
            addHistory(`${firstNumb.value}! = ${message.value}`);
            firstNumb.value = message.value;
          }
        };
function addPi () {
          if (action.value !== "") {
            secondNumb.value += piValue.value;
            message.value = secondNumb.value;
          } else {
            firstNumb.value += piValue.value;
            message.value = firstNumb.value;
          }
        };
function remove () {
          firstNumb.value = "";
          secondNumb.value = "";
          action.value = "";
          message.value = '0';
        };
function addAction (operator) {
          if (firstNumb.value === "") {
            return;
          }
          if (action.value === "") {
            action.value = operator;
            message.value += operator;
          } else {
            calculate();
            action.value = operator;
            firstNumb.value = message.value;
            secondNumb.value = "";
            message.value += operator;
          }
        };
function addHistory (item) {
          history.value.push(item)
          if (history.value.length > 11) {
            history.value.shift();
          }
        };
function calculate () {
  if (firstNumb.value === "" || secondNumb.value === "") {
    return;
  }
  const first = parseFloat(firstNumb.value);
  const second = parseFloat(secondNumb.value);
  let result;
  switch (action.value) {
    case '+':
      result = first + second;
      break;
    case '-':
      result = first - second;
      break;
    case '*':
      result = first * second;
      break;
    case '^':
      result = first ** second;
      break;
    case '/':
      if (second === 0) {
        message.value = "ошибка деления на 0";
        return;
      } else {
        result = first / second;
      }
      break;
  }
  message.value = result.toString();
  addHistory(`${firstNumb.value} ${action.value} ${secondNumb.value} = ${message.value}`);
  firstNumb.value = message.value;
  secondNumb.value = "";
  action.value = "";
};

</script>
