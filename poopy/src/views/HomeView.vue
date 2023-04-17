<script setup>
import { ref } from 'vue'
let year = ref('')
let race = ref('')
let cause = ref('')
let sex = ref('')
let deaths = ref('')
</script>
<template>
  <label>year:</label>
  <select>
    <option>2019</option>
    <option>2018</option>
    <option>2017</option>
  </select>
</template>
