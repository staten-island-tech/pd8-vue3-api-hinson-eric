<script setup>
import { ref } from 'vue'
let year = ref('')
let race = ref('')
let cause = ref('')
let sex = ref('')
let deaths = ref('')
let currentArray = ref([])
let funnyApiTemp = ref(`https://data.cityofnewyork.us/resource/jb7j-dtam.json`)
let funnyApiTemp2 = await fetch(funnyApiTemp)
let funnyApi = await funnyApiTemp2.json()

function filterArray() {
  console.log(funnyApi)
}
filterArray()
</script>
<template>
  <div id="duck">
    <h2>filter graph by</h2>
    <p>select year:</p>
    <select v-model="year">
      <option>all years</option>
      <option>2019</option>
      <option>2018</option>
      <option>2017</option>
      <option>2016</option>
      <option>2015</option>
      <option>2014</option>
      <option>2013</option>
      <option>2012</option>
    </select>
    <p>select sex:</p>
    <select v-model="sex">
      <option>both</option>
      <option>M</option>
      <option>F</option>
    </select>
    <p>select race/ethnicity</p>
    <select v-model="race">
      <option>asian</option>
      <option>hispanic</option>
      <option>non hispanic white</option>
      <option>black</option>
      <option>other</option>
    </select>
    <p>your year is {{ year }}, sex is {{ gender }}, races is {{ races }}</p>
  </div>
</template>

<style>
#duck {
  display: flex;
  flex-direction: column;
}
</style>
