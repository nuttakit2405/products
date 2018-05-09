<template>
  <form action="">
    <div class="modal-card" style="width: auto">
      <header class="modal-card-head">
        <p class="modal-card-title">ตะกร้าสินค้า</p>
      </header>
      <section class="modal-card-body ">
        <div class="product columns" :key="product.id" v-for="(product, index) in data">
          <div style="width: 100px; height: 100px;"><img width="100" class="image" v-url="{filename: product.pic}" alt=""></div>
          <span class="column has-text-left">{{product.name}} (฿{{product.price}})</span>
          <div class="amount column is-3">
            <div class="button" @click="$emit('declese', index)">
              <b-icon type="is-danger" icon="minus-circle"></b-icon>
            </div>
            <input style="width: 50px;" class="input" :value="product.amount" disabled id="">
            <div class="button" @click="$emit('inclese', index)">
              <b-icon type="is-success" icon="plus-circle"></b-icon>
            </div>
          </div>
          <span class="column is-2 has-text-right">฿ {{product.amount*product.price}}</span>
        </div>
      </section>
      <footer class="modal-card-foot">
        <button class="button" type="button" @click="$parent.close()">Close</button>
      </footer>
    </div>
  </form>
</template>

<script>
export default {
  props: {
    data: Array
  }
}
</script>

<style scoped>
.product {
  display: flex;
  flex-direction: row;
  align-items: center;
  border-bottom: 2px solid grey !important;
}
.amount {
  display: flex;
  flex-direction: row;
  align-items: center;
}
</style>
