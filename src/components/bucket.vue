<template>
  <form action="">
    <div class="modal-card" style="width: 80vw">
      <header class="modal-card-head">
        <p class="modal-card-title">ตะกร้าสินค้า</p>
      </header>
      <section class="modal-card-body ">
        <div class="product" :key="product.id" v-for="product in data">
          <img width="100" class="image" v-url="{filename: product.pic}" alt="">
          <span class="">{{product.name}}</span>
          <input class="input" :value="product.amount" disabled id="">
          <span class="">{{product.price}}</span>
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
  flex-direction: column;
  border-bottom: 2px solid grey !important;
}
</style>
