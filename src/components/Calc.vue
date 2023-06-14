<script setup>
import { ref } from 'vue'

const pur_price = ref(450000)
const down_pmt = ref(10000)
const down_pct = ref(10)

function setDown(e) {
  down_pmt.value = e.target.value * down_pct.value / 100
}

function setDownPmt(e) {
  down_pmt.value = e.target.value * pur_price.value / 100
}
 
function setDownPct(e) {
  down_pct.value = (e.target.value / pur_price.value * 100).toFixed(2)
}
 
function makeCurr(val) {
  return new Intl.NumberFormat('en-US', {style: 'currency', currency: 'USD'}).format(val)
}
 
function makePct(val) {
  return new Intl.NumberFormat('en-US', {style: 'percent', minimumFractionDigits: 2}).format(val)
}

</script>

<template>
  <label>Purchase Price </label><br/>
  <input type="number" step="500" v-model="pur_price" @change="setDown"> {{ makeCurr(pur_price) }}
  <br/>
  <label>Down Payment </label><br/>
  <input type="number" step="50" v-model="down_pmt" @change="setDownPct"> {{ makeCurr(down_pmt) }}
  <br/>
  <label>Down Payment Percent </label><br/>
  <input type="number" step="0.25" v-model="down_pct" @change="setDownPmt"> {{ makePct(down_pct/100) }}
 
</template>

<style>  
 
  input {
    width: 4.5rem;
  }

</style>