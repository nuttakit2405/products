<template>
  <div class="hello container">
    <div class="bucket">
      <img src="/static/bucket.png" width="35" alt="">
    </div>
    <div class=" is-size-2 center">WareHouse</div>
    <div class="columns is-centered is-multiline">
      <div class="column is-4" :key="key" v-for="(Product, key) in Products">
        <div class="card widthcard is-centered">
          <div class="card-image">
            <figure class="image is-4by3">
              <img v-url={filename:Product.pic} />
            </figure>
          </div>
          <div class="card-content">
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

          <footer class="card-footer">
            <div class="card-footer-item orange">เลือกสินค้า</div>
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
      Products: []
    }
  },
  methods: {},
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
.bucket {
  position: fixed;
  top: 20px;
  right: 20px;
  border: 2px solid #fc7713;
  cursor: pointer;
  padding: 8px 8px 5px 8px;
  border-radius: 15px;
  opacity: 0.7;
}
.bucket:hover {
  opacity: 1;
}
.orange:hover {
  background-color: #fc7713;
  color: white;
}
</style>
