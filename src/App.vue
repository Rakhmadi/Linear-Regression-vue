<script setup>
import HelloWorld from './components/HelloWorld.vue'
import {ref} from 'vue'


const data_x = ref('35,49,27,33,60,21,45,51')
const data_y = ref('8,9,7,6,13,7,11,12')


const calculate = ()=>{
  alert('asd')
}

const formula = ref('r = \frac{n\sum_{}x y - (\sum_{}x)   }{\sqrt{(n\sum_{}x^2 - (\sum_{}x^2))} \sqrt{(n \sum_{}y^2 - (\sum_{}y)^2)}}')
</script>

<template>
  <div class="m-12">
    <div class="flex flex-col">
      <span class="text-3xl">Linear Regression</span>
      <span class="">By Rakhmadi</span>
    </div>
    <br>
    <label for="email" class="block mb-2 text-sm font-medium text-gray-900 ">Data (X)</label>
    <input type="text" v-model="data_x" id="email" class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-3/4 p-2.5"/>
    <label for="email" class="block mb-2 text-sm font-medium text-gray-900 ">Data (Y)</label>
    <input type="text" v-model="data_y" id="email" class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-3/4 p-2.5"/>
    <br>
    <button type="button" @click="calculate()" class="text-white bg-blue-700 hover:bg-blue-800 focus:ring-4 focus:ring-blue-300 font-medium rounded-lg text-sm px-5 py-2.5 me-2 mb-2">Calculate</button>
  </div>
  <vue-latex class="text-sm" :expression="'r = \\frac{n\\sum_{}x y - (\\sum_{}x)   }{\\sqrt{(n\\sum_{}x^2 - (\\sum_{}x^2))} \\sqrt{(n \\sum_{}y^2 - (\\sum_{}y)^2)}}'" display-mode />

</template>

<style scoped>
.logo {
  height: 6em;
  padding: 1.5em;
  will-change: filter;
  transition: filter 300ms;
}
.logo:hover {
  filter: drop-shadow(0 0 2em #646cffaa);
}
.logo.vue:hover {
  filter: drop-shadow(0 0 2em #42b883aa);
}
</style>
