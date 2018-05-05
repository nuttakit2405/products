<template>
  <div class="hello">
    <div class = " is-size-2 center">WareHouse</div>
    <div class = "columns">
      <div class = "column is-offset-2 is-2">
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

      <div class = "column is-2">
        <b-field class="file buttonImage">
        <b-upload v-model="files">
            <a class="button is-primary">
                <b-icon icon="upload"></b-icon>
                <span>Click to upload</span>
            </a>
        </b-upload>
        <span class="file-name"
            v-if="files && files.length">
            {{ files[0].name }}
        </span>
    </b-field>
      </div>

    </div>

    <div class = "columns is-centered">
       <div class = "column is-2">
        <a class="button is-success" @click="insertProduct(name, price, amount,files[0])">Success</a>
      </div>
    </div>
      <div>
      </div>

    <div class = "columns bb">
       <div class = "column is-offset-1 is-2">
          <div class = " is-size-2"></div>
      </div>
       <div class = "column is-2">
          <div class = " is-size-5">Product Name</div>
      </div>
      <div class = "column is-2">
          <div class = " is-size-5">Price per Unit</div>
      </div>
      <div class = "column is-2">
          <div class = " is-size-5">Amount</div>
      </div>
    </div>

    <div class = "columns productLine " :key="key" v-for="(Product, key) in Products">
      <div class = "column is-offset-1 is-2">
        <img v-url = {filename:Product.pic} width="100" />
      </div>
       <div class = "column is-2">
          <div class = " is-size-5">{{Product.name}}</div>
      </div>
      <div class = "column is-2">
          <div class = " is-size-5">{{Product.price}}</div>
      </div>
      <div class = "column is-2">
          <div class = " is-size-5">{{Product.amount}}</div>
      </div>
    </div>

  </div>
</template>

<script>

import * as firebase from 'firebase'
var config = {
  databaseURL: 'https://product-firebase-a9a0e.firebaseio.com',
  storageBucket: 'product-firebase-a9a0e.appspot.com'
}
firebase.initializeApp(config)
var database = firebase.database()
var productRef = database.ref('/Products')
var storageRef = firebase.storage().ref()

export default {
  name: 'HelloWorld',
  data () {
    return {
      Products: [],
      name: '',
      price: '',
      amount: '',
      files: []
    }
  },
  methods: {
    insertProduct (name, price, amount, file) {
      let data = {
        name: name,
        price: price,
        amount: amount,
        pic: file.name
      }
      productRef.push(data)
      storageRef.child(file.name).put(file)
      this.name = ''
      this.price = ''
      this.amount = ''
      this.files = []
    }
  },
  mounted () {
    productRef.on('value', (snapshot) => {
      this.Products = snapshot.val()
    })
  },
  directives: {
    url: {
      async bind (el, binding) {
        let url = await storageRef.child(binding.value.filename).getDownloadURL()
        el.src = url
      }
    }
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
    margin-right:20px;
    margin-left:20px;
}
.buttonImage{
     margin-top: 30px;
}
.productLine{
   border-bottom: 1px solid hsl(0, 0%, 71%);
   margin-right:20px;
  margin-left:20px;
}
</style>
