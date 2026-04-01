<script setup>
import {h, onMounted, reactive, ref} from 'vue'
import { Chart } from 'chart.js/auto'

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
let intercept = ref(0)
let slope = ref(0)
let slope_atas = ref(0)
let slope_bawah = ref(0)
let intercept_kiri = ref(0)
let intercept_kanan = ref(0)
let lable_x = ref("Diameter pohon")
let lable_y = ref("Tinggi Pohon")

let chartRef = ref(null);
let chartInstance = reactive()
let regressionLine  = ref([])


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

const stringtoArray = (string) =>{
  return string.split(',')
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

  slope_atas.value = (total_n.value * total_xy.value) - (total_x.value * total_y.value)
  slope_bawah.value = (total_n.value * total_x2.value) - (total_x.value * total_x.value) 
  slope.value = (slope_atas.value/slope_bawah.value)

  intercept_kiri.value = (total_y.value / total_n.value)
  intercept_kanan.value = (slope.value * total_x.value / total_n.value)
  intercept.value = intercept_kiri.value - intercept_kanan.value
    // render chart
  to_chart()
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
    intercept.value = 0
    slope.value = 0
    slope_atas.value = 0
    slope_bawah.value = 0

    intercept_kiri.value = 0
    intercept_kanan.value = 0
}
let to_chart = ()=>{
  let data_xx = data_x.value.split(',')
  let data_yy = data_y.value.split(',')

  const regressionLine = data_xx.map(xi => slope.value*xi + intercept.value);

  if (chartInstance) {
    chartInstance.destroy();
  }
  
  chartInstance = new Chart(chartRef.value, {
        type: "scatter",
        data: {
          datasets: [
            {
                label: 'Data',
                data: data_xx.map((xi, i) => ({x: xi, y: data_yy[i]})),
                backgroundColor: 'blue'
            },{
                label: 'Garis Regresi',
                data: data_xx.map((xi, i) => ({x: xi, y: regressionLine[i]})),
                type: 'line',
                borderColor: 'red',
                fill: false
            }
          ]
        },
        options: {
          scales: {
            x: {
              type: "linear",
              position: "bottom",
              title: {
                display: true,
                text: lable_x.value
              }
            },
            y: {
              title: {
                display: true,
                text: lable_y.value
              }
            }
          }
        }
      });
}
onMounted(()=>{
  calculate() 
  to_chart()  
})

</script>

<template>
  <div class="m-6">
    <div class="flex flex-col">
      <span class="text-3xl">Analisi Linear Regression</span>
      <span class="">By Rakhmadi</span>
    </div>
    <br>
    <div>
  <div class="grid grid-cols-2 gap-4">
    <div>
      <label for="email" class="block mb-2 text-sm font-medium text-gray-900 ">Nama Label (X)</label>
      <input type="text" v-model="lable_x" placeholder="example : Diameter Pohon" id="email" class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5"/>
    </div>
    <div>
      <label for="email" class="block mb-2 text-sm font-medium text-gray-900 ">Nama Label (Y)</label>
      <input type="text" v-model="lable_y" placeholder="example : Tinggi Pohon" id="email" class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5"/>
    </div>
    <div>
      <label for="email" class="block mb-2 text-sm font-medium text-gray-900 ">Data (X)</label>
      <input type="text" v-model="data_x" id="email" class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5"/>
    </div>
    <div>
      <label for="email" class="block mb-2 text-sm font-medium text-gray-900 ">Data (Y)</label>
      <input type="text" v-model="data_y" id="email" class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5"/>
    </div>
  </div>
    </div>
    <span>input data x dan y harus dipisahkan dengan koma contoh : 1,2,3,4,5 </span>
    <br>
    <button type="button" @click="calculate" class="text-white bg-blue-700 hover:bg-blue-800 focus:ring-4 focus:ring-blue-300 font-medium rounded-lg text-sm px-5 py-2.5 me-2 mb-2">Calculate</button>
    <button type="button" @click="clear" class="text-white bg-blue-700 hover:bg-blue-800 focus:ring-4 focus:ring-blue-300 font-medium rounded-lg text-sm px-5 py-2.5 me-2 mb-2">Clear</button>

  </div>
  <hr style="border: 0.5px solid black;">
  <div class="flex justify-center items-center">
    <table border="1">
    <tr>
      <th></th>
      <th>n</th>
      <th>X</th>
      <th>Y</th>
      <th>sX<sup>2</sup></th>
      <th>Y<sup>2</sup></th>
      <th>XY</th>
    </tr>
    <tr v-for="(item ,index) in stringtoArray(data_x) " :key="index">
      <td></td>
      <td>{{ index + 1 }}</td>
      <td>{{ stringtoArray(data_x)[index] }}</td>
      <td>{{ stringtoArray(data_y)[index] }}</td>
      <td>{{ stringtoArray(data_x)[index] ** 2 }}</td>
      <td>{{ stringtoArray(data_y)[index] ** 2 }}</td>
      <td>{{ stringtoArray(data_x)[index] * stringtoArray(data_y)[index]}}</td>
    </tr>
    <tr class="bg-red-200">
      <td>Total ∑ = </td>
      <td>{{ total_n }}</td>
      <td>{{ total_x }}</td>
      <td>{{ total_y }}</td>
      <td>{{ total_x2 }}</td>
      <td>{{ total_y2 }}</td>
      <td>{{ total_xy }}</td>
    </tr>
  </table>
  </div>
  <hr style="border: 0.5px solid black;">

  <vue-latex :fontsize="12" :expression="'r = \\frac{n\\sum_{}x y - (\\sum_{}x) (\\sum_{}y)  }{\\sqrt{(n\\sum_{}x^2 - (\\sum_{}x)^2)} \\sqrt{(n \\sum_{}y^2 - (\\sum_{}y)^2)}}'" display-mode />
  <vue-latex :fontsize="12" :expression="`r = \\frac{${total_n}*${total_xy} - (${total_x}) * (${total_y})   }{\\sqrt{(${total_n} * ${total_x2} - (${total_x})^2)} \\sqrt{(${total_n} * ${total_y2} - (${total_y})^2)}}`" display-mode />
  <vue-latex :fontsize="12" :expression="`r = \\frac{${rumus_atas}}{${akar1} * ${akar2}}`" display-mode />
  <vue-latex :fontsize="12" :expression="`r = ${R}`" display-mode />
  <hr style="border: 0.5px solid black;">
  <vue-latex :fontsize="12" :expression="`Slope (b) = \\frac{n \\sum_{} XY - (\\sum_{} X)(\\sum_{} Y)}{n \\sum_{} X^2 - (\\sum_{} X)^2}`" display-mode />
  <vue-latex :fontsize="12" :expression="`Slope (b) = \\frac{${total_n} * ${total_xy} - (${total_x}) * (${total_y})}{${total_n} * ${total_x2} - (${total_x} * ${total_x})}`" display-mode />
  <vue-latex :fontsize="12" :expression="`Slope (b) = \\frac{${slope_atas}}{${slope_bawah}}`" display-mode />
  <vue-latex :fontsize="12" :expression="`Slope (b) = ${slope}`" display-mode />
  <hr style="border: 0.5px solid black;">
  <vue-latex :fontsize="12" :expression="`Intercept (a) =  \\frac{(\\sum_{} Y )}{n} - \\frac{ b(\\sum_{} X)}{n}`" display-mode />
  <vue-latex :fontsize="12" :expression="`Intercept (a) =  \\frac{(${total_y})}{${total_n}} - \\frac{ ${intercept}(${total_x})}{${total_n}}`" display-mode />
  <vue-latex :fontsize="12" :expression="`Intercept (a) =  ${intercept_kiri} - ${intercept_kanan}`" display-mode />
  <vue-latex :fontsize="12" :expression="`Intercept (a) =  ${intercept}`" display-mode />
  <hr style="border: 0.5px solid black;">
  <vue-latex :fontsize="12" :expression="`Persamaan : Y = a+bX`" display-mode />
  <vue-latex :fontsize="12" :expression="`Persamaan : Y = ${intercept} + ${slope}X`" display-mode />
  <hr style="border: 0.5px solid black;">


<div class="m-6">
  <span>Hasil Grafik</span>
  <div class="w-[100%] md:w-[60%]">
    <canvas ref="chartRef"></canvas>
  </div>

  <span>Range Nilai Koefisien Korelasi</span>
  <img src="./assets/range_regresion.png" class="w-[100%] md:w-[20%]">
  <hr style="border: 0.5px solid black;">
  <span>Kesimpulan Hasil</span>
  <table class="text-sm">
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
    <td>Artinya (Koefisien determinasi)</td>
    <td>Kemampuan variabel X dalam  menerangkan keragaman variabel Y sebesar {{ Math.floor((R * R) * 100) }}% sedangkan sisanya yaitu {{ 100 - Math.floor((R * R) * 100) }}% oleh variabel lain.</td>
  </tr>
  <tr>
    <td>Artinya (Slope)</td>
    <td>{{ `Setiap kenaikan 1 ${(lable_x.trim() === "") ? "X" : lable_x} meningkatkan ${(lable_y.trim() === "") ? "Y" : lable_y} sekitar ${slope.toFixed(3)}` }}</td>
  </tr>
</table>
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
