<script setup>
import { ref } from "vue";

const pur_price = ref(450000);
const down_pmt = ref(10000);
const down_pct = ref(10);
const closing_pct = ref(2.0);

function setDown(e) {
  down_pmt.value = (e.target.value * down_pct.value) / 100;
}

function setDownPmt(e) {
  down_pmt.value = (e.target.value * pur_price.value) / 100;
}

function setDownPct(e) {
  down_pct.value = ((e.target.value / pur_price.value) * 100).toFixed(2);
}

function makeCurr(val) {
  return new Intl.NumberFormat("en-US", {
    style: "currency",
    currency: "USD",
  }).format(val);
}

function makePct(val) {
  return new Intl.NumberFormat("en-US", {
    style: "percent",
    minimumFractionDigits: 2,
  }).format(val);
}
</script>

<template>
  <div class="grid grid-cols-3">
    <div class="calc">
      <p>Purchase</p>
      <label>Purchase Price </label><br />
      <input type="number" step="500" v-model="pur_price" @change="setDown" />
      {{ makeCurr(pur_price) }}
      <br />
      <label>Down Payment </label><br />
      <input type="number" step="50" v-model="down_pmt" @change="setDownPct" />
      {{ makeCurr(down_pmt) }}
      <br />
      <label>Down Payment Percent </label><br />
      <input type="number" step="0.25" v-model="down_pct" @change="setDownPmt" />
      {{ makePct(down_pct / 100) }}
      <br />
      <label>Estimated Closing Cost Percent </label><br />
      <input type="number" step="0.25" v-model="closing_pct" />
      {{ makePct(closing_pct / 100) }}
      <br />
      <span>Estimated Closing Costs<br />{{
        makeCurr((closing_pct * pur_price) / 100)
      }}</span>
    </div>
    <div class="calc">
      <p>Operation</p>
      <label>Purchase Price </label><br />
      <input type="number" step="500" v-model="pur_price" @change="setDown" />
      {{ makeCurr(pur_price) }}
      <br />
      <label>Down Payment </label><br />
      <input type="number" step="50" v-model="down_pmt" @change="setDownPct" />
      {{ makeCurr(down_pmt) }}
      <br />
      <label>Down Payment Percent </label><br />
      <input type="number" step="0.25" v-model="down_pct" @change="setDownPmt" />
      {{ makePct(down_pct / 100) }}
      <br />
      <label>Estimated Closing Cost Percent </label><br />
      <input type="number" step="0.25" v-model="closing_pct" />
      {{ makePct(closing_pct / 100) }}
      <br />
      <span>Estimated Closing Costs<br />{{
        makeCurr((closing_pct * pur_price) / 100)
      }}</span>
    </div>
    <div class="calc">
      <p>Results</p>
    </div>
  </div>
</template>

<style>
input {
  width: 5rem;
}

.calc {
  line-height: 1.6;
  margin: 1rem 0;
  border: 1px solid rgb(29, 29, 29);
  background-color: rgb(250, 250, 250);
  padding: 1rem; 
  border-radius: 10px;
  max-width: 350px;
}
</style>
