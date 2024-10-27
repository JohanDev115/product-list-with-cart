<script setup>
import { computed, reactive, ref } from 'vue';
import ProductCard from './components/ProductCard.vue'
import Order from './components/Order.vue';
import Cart from './components/Cart.vue';
import data from './data.json';

const products = ref(data);

products.value.map(el => el.quantity = 0);

let orderCompleted = ref(false);

const cart = ref([]);

const getProductInfo = (product) => {

  // Find if the object already exists in the array
  const index = cart.value.findIndex(el => el.name == product.name);
  // If the object exists, update the quantity
  if (index !== -1) {
    cart.value[index].quantity = product.quantity;
    // If the quantity reaches 0, remove the object
    if (cart.value[index].quantity === 0) {
      cart.value.splice(index, 1);
    }
  } else {
    // If the object doesn't exist, add it with the specified quantity
    cart.value.push(product);
  }
}

const removeProduct = (product) => {
  const index = cart.value.findIndex(el => el.name == product.name);
  cart.value.splice(index, 1);

  const i = products.value.findIndex(el => el.name == product.name);
  products.value[i].quantity = 0;
}

</script>

<template>
  <div class="products">
    <h2 class="products__title">Desserts</h2>
    <div class="product-list">
      <ProductCard v-for="product in products" :key="product" @submitProduct="getProductInfo" :product="product" :quantity="0" :cart="cart" />
    </div>
  </div>
  <Cart :cart="cart" :removeProduct="removeProduct" @showOrder="() => orderCompleted = true" />
  <Order v-if="orderCompleted" :cart="cart" @newOrder="() => orderCompleted = false" />
</template>

<style scoped>

.products__title {
  font-size: 30px;
  color: var(--rose-900);
  margin-bottom: 20px;
}

.product-list {
  width: 100%;
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  gap: 30px;
  align-items: center;
}

@media (min-width: 900px) {
  .product-list { justify-content: space-between}
}

</style>
