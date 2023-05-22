<script setup>
//import DataViewLayoutOptions from 'primevue/dataviewlayoutoptions' // optional
import GridCard from '../components/GridCard.vue'
import Button from 'primevue/button'
</script>

<template>
  <main>
    <div class="surface-ground px-4 py-5 md:px-6 lg:px-8">
      <div class="header"><h1>Mortgage Calculator</h1></div>
      <div class="grid">
        <GridCard @updateValue="(value) => (purchasePrice = value * 10000)" type="price"
          >Purchase price:</GridCard
        >
        <GridCard @updateValue="(value) => (downPayment = value * 10000)" type="price"
          >Down payment:</GridCard
        >
        <GridCard @updateValue="(value) => (repaymentTime = Math.floor(value / 4))" type="time"
          >Repayment time:</GridCard
        >
      </div>
      <div class="grid">
        <GridCard @updateValue="(value) => (interestRate = value)" type="percentage"
          >Interest rate:</GridCard
        >
        <GridCard :loan="loanAmount" type="result">Loan amount:</GridCard>
        <GridCard :loan="estimatedMonthlyPayment" type="result">Estimated pr, month:</GridCard>
      </div>
      <div class="grid">
        <div class="col-6 col-offset-1">
          <Button @click="calculate" label="Get a mortgage quote"></Button>
        </div>
      </div>
    </div>
  </main>
</template>
<script>
export default {
  data() {
    return {
      purchasePrice: 0,
      downPayment: 0,
      repaymentTime: 0,
      interestRate: 0,
      loanAmount: 0,
      estimatedMonthlyPayment: 0
    }
  },
  methods: {
    calculate() {
      var p = this.purchasePrice - this.downPayment
      this.loanAmount = p
      var r = this.interestRate / 100 / 12
      var n = this.repaymentTime * 12

      var M = ((r * Math.pow(1 + r, n)) / (Math.pow(1 + r, n) - 1)) * p
      this.estimatedMonthlyPayment = M.toFixed(2)
    }
  }
}
</script>
