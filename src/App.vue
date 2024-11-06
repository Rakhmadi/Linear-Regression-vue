<script setup>
import HelloWorld from './components/HelloWorld.vue'
import {ref} from 'vue'


const data_x = ref('8,9,7,6,13,7,11,12')
const data_y = ref('35,49,27,33,60,21,45,51')


let total_n = ref(0)
let total_y = ref(0)
let total_x = ref(0)
let total_y2 = ref(0)
let total_x2 = ref(0)
let total_xy = ref(0)
let rumus_atas = ref(0) 
let akar1 = ref(0)
let akar2 = ref(0)          
let rumus_bawah = ref(0)
let R = ref(0)
let formula_1 = ref(0)

const checkKodelasiLable = (param)=>{
    if(param >= 0.00 && param <= 0.19){
        return "Sangat rendah"
    }else if(param >= 0.20 && param <= 0.399){
        return "Rendah"
    }else if(param >= 0.40 && param <= 0.599){
        return "Sedang"
    }else if(param >= 0.60 && param <= 0.799){
        return "Kuat"
    }else if(param >= 0.80 && param <= 1.00){
        return "Sangat kuat"
    }else{
        return undefined
    }
}

const calculate = ()=>{
    let data_xx = data_x.value.split(',')
    let data_yy = data_y.value.split(',')
    // clear
    total_n.value = 0
    total_y.value = 0
    total_x.value = 0
    total_y2.value = 0
    total_x2.value = 0
    total_xy.value = 0
        
    
    for (let index = 0; index < data_xx.length; index++) {
      total_n.value = data_xx.length
      total_y.value += Number(data_yy[index])
      total_x.value += Number(data_xx[index])
      total_y2.value += Number(data_yy[index]) * Number(data_yy[index])
      total_x2.value += Number(data_xx[index]) * Number(data_xx[index])
      total_xy.value += Number(data_xx[index]) * Number(data_yy[index])
    }

  rumus_atas.value =  total_n.value * total_xy.value - total_x.value * total_y.value
  akar1.value =  Math.sqrt((total_n.value * total_x2.value) - (total_x.value * total_x.value))
  akar2.value = Math.sqrt((total_n.value * total_y2.value) - (total_y.value * total_y.value))
  rumus_bawah.value =   akar1.value * akar2.value
  R.value = rumus_atas.value / rumus_bawah.value
    
} 

const clear =()=>{
  total_n.value = 0
    total_y.value = 0
    total_x.value = 0
    total_y2.value = 0
    total_x2.value = 0
    total_xy.value = 0

    rumus_atas.value = 0
    akar1.value = 0
    akar2.value = 0
    rumus_bawah.value = 0
    R.value = 0
}

</script>

<template>
  <div class="m-12">
    <div class="flex flex-col">
      <span class="text-3xl">Analisi Linear Regression</span>
      <span class="">By Rakhmadi</span>
    </div>
    <br>
    <label for="email" class="block mb-2 text-sm font-medium text-gray-900 ">Data (X)</label>
    <input type="text" v-model="data_x" id="email" class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-3/4 p-2.5"/>
    <label for="email" class="block mb-2 text-sm font-medium text-gray-900 ">Data (Y)</label>
    <input type="text" v-model="data_y" id="email" class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-3/4 p-2.5"/>
    <br>
    <button type="button" @click="calculate" class="text-white bg-blue-700 hover:bg-blue-800 focus:ring-4 focus:ring-blue-300 font-medium rounded-lg text-sm px-5 py-2.5 me-2 mb-2">Calculate</button>
    <button type="button" @click="clear" class="text-white bg-blue-700 hover:bg-blue-800 focus:ring-4 focus:ring-blue-300 font-medium rounded-lg text-sm px-5 py-2.5 me-2 mb-2">Clear</button>

  </div>
  <vue-latex class="text-sm" :expression="'r = \\frac{n\\sum_{}x y - (\\sum_{}x) (\\sum_{}y)  }{\\sqrt{(n\\sum_{}x^2 - (\\sum_{}x)^2)} \\sqrt{(n \\sum_{}y^2 - (\\sum_{}y)^2)}}'" display-mode />
  <vue-latex class="text-sm" :expression="`r = \\frac{${total_n}*${total_xy} - (${total_x}) * (${total_y})   }{\\sqrt{(${total_n} * ${total_x2} - (${total_x})^2)} \\sqrt{(${total_n} * ${total_y2} - (${total_y})^2)}}`" display-mode />
  <vue-latex class="text-sm" :expression="`r = \\frac{${rumus_atas}}{${akar1} * ${akar2}}`" display-mode />
  <vue-latex class="text-sm" :expression="`r = ${R}`" display-mode />

<div class="m-12">
  <table >
  <tr>
    <td>Korelasi</td>
    <td>{{ R }}</td>
  </tr>
  <tr>
    <td>Determinasi</td>
    <td>{{ R * R }}</td>
  </tr>
  <tr>
    <td>Keterangan</td>
    <td>{{ checkKodelasiLable(R) }}</td>
  </tr>
  <tr>
    <td>Percent</td>
    <td>{{ Math.floor((R * R) * 100) }}%</td>
  </tr>
  <tr>
    <td>Sisa Percent</td>
    <td>{{ 100 - Math.floor((R * R) * 100) }}%</td>
  </tr>
  <tr>
    <td>Artinya</td>
    <td>Kemampuan variabel X dalam  menerangkan keragaman variabel Y sebesar {{ Math.floor((R * R) * 100) }}% sedangkan sisanya yaitu {{ 100 - Math.floor((R * R) * 100) }}% oleh variabel lain.</td>
  </tr>
</table>
<blockquote class="mt-6 text-sm italic font-semibold text-gray-900">
    <p>"NOTE : Regresi linear adalah teknik analisis data yang memprediksi nilai data yang tidak diketahui dengan menggunakan nilai data lain yang terkait dan diketahui"</p>
</blockquote>
</div>

</template>
<style scoped>
table, th, td {
  border:1px solid black;
}

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
