<template>
  <div class='calculator'>
    <div class='discountpriceBox columns is-mobile'>
      <div class="column is-8">netDiscount</div><div class="column">{{ netDiscount }}</div>
    </div>
    <div v-for='(item) in itemDisplay' :key='item.id' class="columns is-mobile">
      <div class="column is-6">{{ item.key }}</div>
      <div class="column"><button @click='decrease(item.key)' class="button is-danger is-large" > - </button></div>
      <div class="column">{{ promotions[item.key].amount }}</div>
      <div class="column"><button @click='increase(item.key)' class ="button is-primary is-large " > + </button></div>
    </div>
  </div>
</template>

<script>
import promotions from '../promotions/promotions.json'
const checkOut = require('../module/checkOut.js')
export default {
  name: 'calculator',
  data () {
    return {
      msg: 'asdasd',
      promotions,
      netDiscount: 0,
      itemDisplay: [],
      items: []
    }
  },
  methods: {
    decrease (key) {
      console.log(this.promotions[key].amount)
      if (this.promotions[key].amount < 1) {
        return
      }
      this.promotions[key].amount--
      var index = this.items.findIndex(item => item.price + '' === key + '')
      this.items.splice(index, 1)

      var resultNetDiscount = checkOut(this.items, promotions)
      console.log(resultNetDiscount)
      this.netDiscount = resultNetDiscount.reduce((sumPrice, product) => sumPrice + product.priceDiscount, 0)
    },
    increase (key) {
      this.promotions[key].amount++
      this.items.push({ price: promotions[key].tier1 })

      var resultNetDiscount = checkOut(this.items, promotions)
      console.log(resultNetDiscount)
      this.netDiscount = resultNetDiscount.reduce((sumPrice, product) => sumPrice + product.priceDiscount, 0)
    }
  },
  created () {
    Object.keys(promotions).forEach((k, v) => {
      this.itemDisplay.push({key: k})
    })
  }
}
</script>

<style>
@import url('https://fonts.googleapis.com/css?family=Roboto');
body {
  font-family: 'Roboto', sans-serif;
}
.discountpriceBox {
  font-family: 'Roboto', sans-serif;
  background-color: #c0c0c0;
  font-size: 30px;
}
</style>
