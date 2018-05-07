<template>
  <div class="hello container">
    <div class=" is-size-2 center">WareHouse</div>
    <div class="columns">
      <div class="column is-offset-2 is-2">
        <b-field label="Product Name">
          <b-input v-model="name"></b-input>
        </b-field>
      </div>

      <div class="column is-2">
        <b-field label="Price per Unit" expanded>
          <b-input type="number" v-model="price"></b-input>
        </b-field>
      </div>

      <div class="column is-2">
        <b-field label="Amount" expanded>
          <b-input type="number" v-model="amount"></b-input>
        </b-field>
      </div>

      <div class="column is-2">
        <b-field class="file buttonImage">
          <b-upload v-model="files">
            <a class="button is-primary">
              <b-icon icon="upload"></b-icon>
              <span>Click to upload</span>
            </a>
          </b-upload>
          <span class="file-name" v-if="files && files.length">
            {{ files[0].name }}
          </span>
        </b-field>
      </div>

    </div>
<!-- -------------------------------------------------- -->
    <div class="columns is-centered">
      <div class="column is-4">
        <a class="button is-success" @click="insertProduct(name, price, amount,files[0])">Success</a>
      </div>
    </div>
    <div>
    </div>

    <div class="columns is-centered is-multiline">
      <div class="column is-5" :key="key" v-for="(Product, key) in Products">
        <div class="card widthcard is-centered">
          <div class="card-image">
            <figure class="image is-4by3">
              <img v-url={filename:Product.pic} />
            </figure>
          </div>
          <div class="card-content" v-if="key !== updateKey">
            <div>
                <div class="title is-4 has-text-left">{{Product.name}}
                </div>
                <div class = "columns">
                  <span class="column title is-6 has-text-left has-text-danger">฿{{Product.price}}
                  </span>
                  <span class="column title is-6 has-text-right">Amount  {{Product.amount}}
                  </span>
                </div>
            </div>
          </div>
          <!-- ----------------------edit------------------------- -->
          <div class="card-content" v-if="key === updateKey">
            <div>
                <div class="title is-4 has-text-left"><input v-model="updateName" class="input is-focused" type="text" placeholder="Edit Name">
                </div>
                <div class = "columns">
                  <span class="column title is-6 has-text-left has-text-danger"> <input v-model="updatePrice" class="input is-focused" type="number" placeholder="Edit Price">
                  </span>
                  <span class="column title is-6 has-text-right"> <input v-model="updateAmount" class="input is-focused" type="number" placeholder="Edit Amount">
                  </span>
                </div>
            </div>
          </div>

          <footer class="card-footer">
            <div @click="updateToProduct (updateName, updatePrice,updateAmount, Product.pic)" class="card-footer-item">Save</div>
            <div  @click="setUpdateProduct (key, Product)" class="card-footer-item">Edit</div>
            <div @click="deleteProduct (key)" class="card-footer-item">Delete</div>
          </footer>
        </div>
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
      files: [],
      updateKey: '',
      updateName: '',
      updatePrice: '',
      updateAmount: ''
    }
  },
  methods: {
    async insertProduct (name, price, amount, file) {
      await storageRef.child(file.name).put(file)
      let data = {
        name: name,
        price: price,
        amount: amount,
        pic: file.name
      }
      await productRef.push(data)
      this.name = ''
      this.price = ''
      this.amount = ''
      this.files = []
    },
    deleteProduct (key) {
      productRef.child(key).remove()
    },
    setUpdateProduct (key, Product) {
      this.updateKey = key
      this.updateName = Product.name
      this.updatePrice = Product.price
      this.updateAmount = Product.amount
    },
    updateToProduct (name, price, amount, file) {
      productRef.child(this.updateKey).update({
        name: name,
        price: price,
        amount: amount,
        pic: file
      })
      this.updateKey = ''
      this.updateName = ''
      this.updatePrice = ''
      this.updateAmount = ''
    }
  },
  mounted () {
    productRef.on('value', snapshot => {
      this.Products = snapshot.val()
    })
  },
  directives: {
    url: {
      async bind (el, binding) {
        let url = await storageRef
          .child(binding.value.filename)
          .getDownloadURL()
        el.src = url
      }
    }
  }
}
</script>

<style>
.center {
  padding-bottom: 20px;
}
.center2 {
  padding-bottom: 50px;
}
.bb {
  border-bottom: 1px solid hsl(0, 0%, 71%);
  margin-top: 50px;
  margin-right: 20px;
  margin-left: 20px;
}
.buttonImage {
  margin-top: 30px;
}
.productLine {
  border-bottom: 1px solid hsl(0, 0%, 71%);
  margin-right: 20px;
  margin-left: 20px;
}
.widthcard {
  width: 500px;
}
.card-footer-item {
  cursor: pointer;
}
.card-footer-item:hover {
  background-color:hsl(0, 0%, 96%);
}
</style>
