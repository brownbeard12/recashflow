<script setup>
import { computed, ref } from "vue";
import InputItem from "./InputItem.vue";
import OutputItem from "./OutputItem.vue"

const purchaseInputs = ref([{
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
}, {
  label: 'Estimated Closing Cost Percent',
  step: 0.25,
  amount: 2,
  format: makePct,
}, {
  label: 'Interest Rate',
  step: 0.125,
  amount: 5,
  format: makePct,
}, {
  label: 'Loan Term',
  step: 5,
  amount: 30,
  format: year,
}, {
  label: 'Home Insurance',
  step: 25,
  amount: 200,
  format: makeCurr,
}, {
  label: 'Taxes',
  step: 5,
  amount: 200,
  format: makeCurr,
}, {
  label: 'HOA Fee',
  step: 5,
  amount: 100,
  format: makeCurr,
}, {
  label: 'Remodel Costs',
  step: 100,
  amount: 5000,
  format: makeCurr,
}, {
  label: 'Setup Costs',
  step: 100,
  amount: 1000,
  format: makeCurr,
}, {
  label: 'Furnishings',
  step: 250,
  amount: 10000,
  format: makeCurr,
}, {
  label: 'Legal Fees',
  step: 50,
  amount: 500,
  format: makeCurr,
},
])

const purchaseComp = computed(() => {
  return [
    {
      label: 'Closing Costs',
      amount: purchaseInputs.value[0].amount * purchaseInputs.value[3].amount / 100,
    }, {
      label: 'Loan Amount',
      amount: purchaseInputs.value[0].amount - purchaseInputs.value[1].amount,
    },
  ]
})

const purchaseOutputs = computed(() => {
  return [
    {
      label: 'Estimated Cash to Close',
      amount: Number(purchaseInputs.value[1].amount) + Number(purchaseComp.value[0].amount),
    }, {
      label: 'Principal & Interest Payment',
      amount: (purchaseInputs.value[4].amount / 1200 * purchaseComp.value[1].amount) / (1 - (1 + purchaseInputs.value[4].amount / 1200) ** (-12 * purchaseInputs.value[5].amount)),
    },
  ]
})

const monthlyPayment = computed(() => {
  return [
    {
      label: 'Monthly Payment',
      amount: Number(purchaseOutputs.value[1].amount) + Number(purchaseInputs.value[6].amount) + Number(purchaseInputs.value[7].amount) + Number(purchaseInputs.value[8].amount),
    },
  ]
})

const opInputs = ref([{
  label: 'Average Daily Rate',
  step: 5,
  amount: 150,
  format: makeCurr,
}, {
  label: 'Occupancy Rate',
  step: 0.5,
  amount: 60,
  format: makePct,
}, {
  label: 'Number of Units',
  step: 1,
  amount: 1,
  format: hold,
},
{
  label: 'Platform Fees',
  step: 0.25,
  amount: 3,
  format: makePct,
}, {
  label: 'Maintenance Cost Percentage',
  step: 0.5,
  amount: 3,
  format: makePct,
}, {
  label: 'Utilities',
  step: 25,
  amount: 300,
  format: makeCurr,
}, {
  label: 'Management Fee Percentage',
  step: 0.25,
  amount: 20,
  format: makePct,
}, 

])

const opOutputs = computed(() => {
  return [
    {
      label: 'Annual Gross Revenue',
      amount: Number(opInputs.value[0].amount) * Number(opInputs.value[1].amount / 100) * Number(opInputs.value[2].amount) * 365,
    },
  ]
})


function setDown(e) {
  //down_pmt.value = (e.target.value * down_pct.value) / 100;
  purchaseInputs.value[1].amount = (e.target.value * purchaseInputs.value[2].amount) / 100;
}

function setDownPmt(e) {
  //down_pmt.value = (e.target.value * pur_price.value) / 100;
  purchaseInputs.value[1].amount = (e.target.value * purchaseInputs.value[0].amount) / 100;
}

function setDownPct(e) {
  //down_pct.value = ((e.target.value / pur_price.value) * 100).toFixed(2);
  purchaseInputs.value[2].amount = ((e.target.value / purchaseInputs.value[0].amount) * 100).toFixed(2);
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

function hold(val) {
  return val
}

function year(val) {
  return val + ' years'
}
</script>

<template>
  <div class="grid grid-cols-1 md:grid-cols-3">
    <div class="calc">
      <p>Purchase & Startup</p>
      <p>Inputs</p>

      <div v-for="item in purchaseInputs" :key="item.label">
        <InputItem :label="item.label" :numSteps="item.step" v-model="item.amount" @change="item.function">
          {{ item.format(item.amount) }}
        </InputItem>
      </div>

      <p>Outputs</p>

      <div v-for="item in purchaseComp" :key="item.label">
        <OutputItem :label="item.label">
          {{ makeCurr(item.amount) }}
        </OutputItem>
      </div>

      <div v-for="item in purchaseOutputs" :key="item.label">
        <OutputItem :label="item.label">
          {{ makeCurr(item.amount) }}
        </OutputItem>
      </div>

      <div v-for="item in monthlyPayment" :key="item.label">
        <OutputItem :label="item.label">
          {{ makeCurr(item.amount) }}
        </OutputItem>
      </div>

    </div>
    <div class="calc">
      <p>Operation</p>
      <p>Inputs</p>

      <div v-for="item in opInputs" :key="item.label">
        <InputItem :label="item.label" :numSteps="item.step" v-model="item.amount" @change="item.function">
          {{ item.format(item.amount) }}
        </InputItem>
      </div>

      <p>Outputs</p>

      <div v-for="item in opOutputs" :key="item.label">
        <OutputItem :label="item.label">
          {{ makeCurr(item.amount) }}
        </OutputItem>
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
