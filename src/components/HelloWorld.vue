<template>
  <div class="hello">
    <div class = " is-size-2 center">WareHouse</div>
    <div class = "columns">
      <div class = "column is-offset-one-quarter is-2">
        <b-field label="Product Name">
            <b-input v-model="name"></b-input>
        </b-field>
      </div>

      <div class = "column is-2">
        <b-field label="Price per Unit" expanded>
            <b-input type="number" v-model="price"></b-input>
        </b-field>
      </div>

      <div class = "column is-2">
        <b-field label="Amount" expanded>
            <b-input type="number" v-model="amount"></b-input>
        </b-field>
      </div>
    </div>

    <div class = "columns is-centered">
       <div class = "column is-2">
        <a class="button is-success" @click="insertProduct(name, price, amount)">Success</a>
      </div>
    </div>
      <div>
      </div>

    <div class = "columns bb">
       <div class = "column is-4">
          <div class = " is-size-5">Product Name</div>
      </div>
      <div class = "column is-4">
          <div class = " is-size-5">Price per Unit</div>
      </div>
      <div class = "column is-3">
          <div class = " is-size-5">Amount</div>
      </div>
    </div>
<!-- <ul :key="key" v-for="(Product, key) in Products" style= "text-align: center;">{{Product}}</ul> -->
<div class = "columns" :key="key" v-for="(Product, key) in Products">
       <div class = "column is-4">
          <div class = " is-size-5">{{Product.name}}</div>
      </div>
      <div class = "column is-4">
          <div class = " is-size-5">{{Product.price}}</div>
      </div>
      <div class = "column is-3">
          <div class = " is-size-5">{{Product.amount}}</div>
      </div>
    </div>

  </div>
</template>

<script>

import * as firebase from 'firebase'
var config = {
  databaseURL: 'https://product-firebase-a9a0e.firebaseio.com'
}
firebase.initializeApp(config)
var database = firebase.database()
var productRef = database.ref('/Products')

export default {
  name: 'HelloWorld',
  data () {
    return {
      Products: [],
      name: '',
      price: '',
      amount: ''
    }
  },
  methods: {
    insertProduct (name, price, amount) {
      let data = {
        name: name,
        price: price,
        amount: amount
      }
      productRef.push(data)
      this.name = ''
      this.price = ''
      this.amount = ''
    }
  },
  mounted () {
    productRef.on('value', (snapshot) => {
      this.Products = snapshot.val()
    })
  }
}
</script>

<style>
.center{
  padding-bottom: 20px;
}
.center2{
  padding-bottom: 50px;
}
.bb{
    border-bottom: 1px solid hsl(0, 0%, 71%);
    margin-top: 50px;
}
</style>
