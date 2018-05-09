<template>
  <div class="hello container">
    <div class="bucket">
      <img src="/static/bucket.png" width="50" alt="">
    </div>
    <div class=" is-size-2 center">WareHouse</div>
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
              <div class="columns">
                <span class="column title is-6 has-text-left has-text-danger">฿{{Product.price}}
                </span>
                <span class="column title is-6 has-text-right">Amount {{Product.amount}}
                </span>
              </div>
            </div>
          </div>
          <!-- ----------------------edit------------------------- -->
          <div class="card-content" v-if="key === updateKey">
            <div>
              <div class="title is-4 has-text-left"><input v-model="updateName" class="input is-focused" type="text" placeholder="Edit Name">
              </div>
              <div class="columns">
                <span class="column title is-6 has-text-left has-text-danger"> <input v-model="updatePrice" class="input is-focused" type="number" placeholder="Edit Price">
                </span>
                <span class="column title is-6 has-text-right"> <input v-model="updateAmount" class="input is-focused" type="number" placeholder="Edit Amount">
                </span>
              </div>
            </div>
          </div>

          <footer class="card-footer">
            <div @click="updateToProduct (updateName, updatePrice,updateAmount, Product.pic)" class="card-footer-item">Save</div>
            <div @click="setUpdateProduct (key, Product)" class="card-footer-item">Edit</div>
            <div @click="deleteProduct (key)" class="card-footer-item">Delete</div>
          </footer>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'GetProduct',
  data () {
    return {
      database: {},
      storageRef: {},
      productRef: {},
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
      await this.storageRef.child(file.name).put(file)
      let data = {
        name: name,
        price: price,
        amount: amount,
        pic: file.name
      }
      await this.productRef.push(data)
      this.name = ''
      this.price = ''
      this.amount = ''
      this.files = []
    },
    deleteProduct (key) {
      this.productRef.child(key).remove()
    },
    setUpdateProduct (key, Product) {
      this.updateKey = key
      this.updateName = Product.name
      this.updatePrice = Product.price
      this.updateAmount = Product.amount
    },
    updateToProduct (name, price, amount, file) {
      this.productRef.child(this.updateKey).update({
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
  created () {
    this.database = this.$firebase.database()
    this.storageRef = this.$firebase.storage().ref()
    this.productRef = this.database.ref('/Products')
    this.productRef.on('value', snapshot => {
      this.Products = snapshot.val()
    })
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
  background-color: hsl(0, 0%, 96%);
}
</style>
