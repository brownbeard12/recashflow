<script setup>
import { computed, ref } from "vue";
import InputItem from "./InputItem.vue";

const model = ref([{
  label: 'Purchase Price',
  step: 500,
  amount: 450000,
  function: setDown,
  format: makeCurr,
}, {
  label: 'Down Payment',
  step: 500,
  amount: 45000,
  function: setDownPct,
  format: makeCurr,
}, {
  label: 'Down Payment Percent',
  step: 0.25,
  amount: 10,
  function: setDownPmt,
  format: makePct,
}
])

const pur_price = ref(450000);
const down_pmt = ref(45000);
const down_pct = ref(10);
const down_pmt2 = ref(5000)
const closing_pct = ref(2.0);
const loan_amt = computed(() => {
  return pur_price.value - down_pmt.value
})
const remodel = ref(5000)
const setup = ref(2000)
const furnish = ref(12000)
const legal = ref(500)
const int_rate = ref(5.5)
const loan_term = ref(30)
const home_ins = ref(200)
const hoa_fee = ref(125)
const pi_pmt = computed(() => {
  return (int_rate.value / 1200 * loan_amt.value) / (1 - (1 + int_rate.value / 1200) ** (-12 * loan_term.value))
})

function setDown(e) {
  //down_pmt.value = (e.target.value * down_pct.value) / 100;
  model.value[1].amount = (e.target.value * model.value[2].amount) / 100;
}

function setDownPmt(e) {
  //down_pmt.value = (e.target.value * pur_price.value) / 100;
  model.value[1].amount = (e.target.value * model.value[0].amount) / 100;
}

function setDownPct(e) {
  //down_pct.value = ((e.target.value / pur_price.value) * 100).toFixed(2);
  model.value[2].amount = ((e.target.value / model.value[0].amount) * 100).toFixed(2);
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
  }).format(val / 100);
}
</script>

<template>
  <div class="grid grid-cols-1 md:grid-cols-3">
    <div class="calc">
      <p>Purchase & Startup</p>
      <p>Inputs</p>
      <label>Purchase Price </label><br />
      <input type="number" step="500" v-model="pur_price" @keyup="setDown" @change="setDown" />
      {{ makeCurr(pur_price) }}
      <InputItem label="Down Payment" numSteps="500" v-model="down_pmt" @change="setDownPct">
        {{ makeCurr(down_pmt) }}
      </InputItem>
      <InputItem label="Down Payment Percent" numSteps="0.25" v-model="down_pct" @change="setDownPmt">
        {{ makePct(down_pct / 100) }}
      </InputItem>
      <span>Loan Amount<br />{{
        makeCurr(loan_amt)
      }}</span>
      <br />
      <label>Estimated Closing Cost Percent </label><br />
      <input type="number" step="0.25" v-model="closing_pct" />
      {{ makePct(closing_pct / 100) }}
      <br />
      <span>Estimated Closing Costs<br />{{
        makeCurr((closing_pct * pur_price) / 100)
      }}</span>
      <br />
      <label>Remodel Costs </label><br />
      <input type="number" step="100" v-model="remodel" />
      {{ makeCurr(remodel) }}
      <br />
      <label>Mortgage Interest Rate </label><br />
      <input type="number" step="0.125" v-model="int_rate" />
      {{ makePct(int_rate / 100) }}
      <br>
      <span>Principal & Interest Payment<br />{{ makeCurr(pi_pmt)
      }}</span>
    </div>
    <div class="calc">
      <p>Operation</p>
      <div v-for="item in model" :key="item.label">
        <InputItem :label="item.label" :numSteps="item.step" v-model="item.amount" @change="item.function">
          {{ item.format(item.amount) }}
        </InputItem>
      </div>




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
  max-width: 300px;
}
</style>
