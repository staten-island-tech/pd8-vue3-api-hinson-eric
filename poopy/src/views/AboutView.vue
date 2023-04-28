<script setup>
import { ref, onMounted, reactive } from 'vue'

import { Pie } from 'vue-chartjs'
import { Chart as ChartJS, ArcElement, Tooltip, Legend } from 'chart.js'
ChartJS.register(ArcElement, Tooltip, Legend)

let yes = ref(false)
let year = ref('')
let race = ref('')
let cause = ref(['bro you did something wrong, remake the graph'])
let sex = ref('')
let deaths = ref(['69'])
let currentArray = ref([])
let ogArray = ref('')
let chartData2 = ref({
  labels: cause.value,
  datasets: [
    {
      label: 'Data One',
      backgroundColor: '#52b69a',
      data: deaths.value
    }
  ]
})

async function getDeaths() {
  let temp = await fetch('https://data.cityofnewyork.us/resource/jb7j-dtam.json')
  let temp2 = await temp.json()
  ogArray.value = temp2
}

onMounted(() => {
  getDeaths()
})

function pasteGraph(array) {}

function filterArray(array) {
  let filteredYear = ref([])
  let filteredSex = ref([])
  let filteredRace = ref('')
  if (year.value == 'Every Year') {
    filteredYear = ogArray
  } else {
    filteredYear = array.filter((element) => element.year == year.value)
  }
  if (sex.value == 'both') {
    filteredSex = filteredYear
  } else if (sex.value == 'Male') {
    filteredSex = filteredYear
      .filter((element) => element.sex == 'Male')
      .concat(filteredYear.filter((element) => element.sex == 'M'))
  } else if (sex.value == 'Female') {
    filteredSex = filteredYear
      .filter((element) => element.sex == 'Female')
      .concat(filteredYear.filter((element) => element.sex == 'F'))
  } else {
    filteredSex = []
  }
  if (race.value == 'All') {
    filteredRace = filteredSex
  } else {
    filteredRace = filteredSex.filter((element) => element.race_ethnicity == race.value)
  }
  currentArray.value = filteredRace
  let ways2die = []
  filteredRace.forEach((element) => ways2die.push(element.leading_cause))
  cause.value = ways2die
  let currentDeaths = []
  filteredRace.forEach((element) => currentDeaths.push(element.deaths))
  deaths.value = currentDeaths
  console.log(yes)

  chartData2 = ref({
    labels: ways2die,
    datasets: [
      {
        label: 'Data One',
        backgroundColor: [
          '#ffc8dd',
          '#52b69a',
          '#adc178',
          '#7209b7',
          '#f18701',
          '#ccc5b9',
          '#ee6055',
          '#ffd100',
          '#202020',
          '#772f1a',
          '#001d4a'
        ],
        data: currentDeaths
      }
    ]
  })
}
</script>

<template>
  <div id="duck">
    <h2>filter graph by</h2>
    <p>select year:</p>
    <select v-model="year">
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
      <option>Male</option>
      <option>Female</option>
    </select>
    <p>select race/ethnicity</p>
    <select v-model="race">
      <option>All</option>
      <option>Asian and Pacific Islander</option>
      <option>Hispanic</option>
      <option>Non-Hispanic White</option>
      <option>Non-Hispanic Black</option>
      <option>Other Race/ Ethnicity</option>
      <option>Not Stated/Unknown</option>
    </select>
    <button v-if="yes" @click=";(yes = !yes), window.location.reload()">
      make a new graph!!!!!
    </button>
    <button v-else @click=";(yes = !yes), filterArray(ogArray)">generate graph :D</button>

    <Pie :data="chartData2" v-if="yes" />
    <p v-else>click the button to create a graph!</p>
  </div>
</template>

<style>
#duck {
  display: flex;
  flex-direction: column;
}
#tw {
  margin-top: 20px;
  width: 100px;
  margin-left: 230px;
}
button {
  margin-bottom: 20px;
  --s: 0.25em; /* control the wave*/

  padding: 0.4em 0.5em;
  background-color: #00e898;
  color: black;
  --_s: calc(var(--s) * 4) 51% repeat-x;
  --_r: calc(1.345 * var(--s)) at left 50%;
  --_g1: #000 99%, #0000 101%;
  --_g2: #0000 99%, #000 101%;
  --mask: radial-gradient(var(--_r) top calc(var(--s) * 1.9), var(--_g1))
      calc(50% - 2 * var(--s) - var(--_i, 0px)) 0 / var(--_s),
    radial-gradient(var(--_r) top calc(var(--s) * -0.9), var(--_g2)) calc(50% - var(--_i, 0px))
      var(--s) / var(--_s),
    radial-gradient(var(--_r) bottom calc(var(--s) * 1.9), var(--_g1))
      calc(50% - 2 * var(--s) + var(--_i, 0px)) 100% / var(--_s),
    radial-gradient(var(--_r) bottom calc(var(--s) * -0.9), var(--_g2)) calc(50% + var(--_i, 0px))
      calc(100% - var(--s)) / var(--_s);
  -webkit-mask: var(--mask);
  mask: var(--mask);
  clip-path: polygon(
    calc(2 * var(--s) - var(--_i, 0px)) 0,
    calc(100% - var(--_i, 0px)) 0,
    calc(100% - var(--s)) 50%,
    calc(100% - 2 * var(--s) + var(--_i, 0px)) 100%,
    calc(0% + var(--_i, 0px)) 100%,
    var(--s) 50%
  );
  cursor: pointer;
  transition: 0.35s;
}
button.alt {
  clip-path: polygon(
    calc(0% - var(--_i, 0px)) 0,
    calc(100% - 2 * var(--s) - var(--_i, 0px)) 0,
    calc(100% - var(--s)) 50%,
    calc(100% + var(--_i, 0px)) 100%,
    calc(2 * var(--s) + var(--_i, 0px)) 100%,
    var(--s) 50%
  );
}
button:hover {
  --_i: calc(2 * var(--s));
}
button.alt:hover {
  --_i: calc(-2 * var(--s));
}
button:active {
  background-image: linear-gradient(#0004 0 0);
}
button:focus-visible {
  -webkit-mask: none;
  clip-path: none;
  outline-offset: 0.1em;
  padding-block: 0.2em;
  margin-block: 0.2em;
  transition: 0s;
}
</style>
