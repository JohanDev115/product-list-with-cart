<script setup>
import { computed, ref } from "vue"
import ProductSummary from './ProductSummary.vue';

const emit = defineEmits(['showOrder'])

const props = defineProps({
  removeProduct: Function,
  cart: Object
})


const removeProduct = props.removeProduct;
const cart = props.cart;
const cartCount = computed(() => cart.map(obj => obj?.quantity).reduce((total, currentvalue) => total + currentvalue, 0))
const totalPrice = computed(() => cart.map(el => el.quantity * el.price).reduce((total, currentvalue) => total + currentvalue, 0))
const formatedPrice = (price) => { return new Intl.NumberFormat('en-US', { style: 'currency', currency: 'USD', currencySign: "accounting", }).format(price)};

const showOrder = () => emit("showOrder");

</script>

<template>
  <div class="cart">
    <h2 class="cart-counter">Your Cart ({{ cartCount }})</h2>
    <div v-if="cart.length > 0">
      <div class="product-summary-container">
        <ProductSummary v-for="product in cart" :key="product" :product="product" @removeProduct="removeProduct" />
      </div>
      <div>
        <div class="total-order">
          <p>Order Total</p>
          <b class="total-order__amount">{{ formatedPrice(totalPrice) }}</b>
        </div>
        <div class="confirm-order">
          <p> <img src="../assets/images/icon-carbon-neutral.svg" alt=""> This is a <b>carbon-neutral</b> delivery</p>
          <button class="confirm-order__button" @click="showOrder">Confirm Order</button>
        </div>
      </div>
    </div>
    <div class="empty-cart" v-else>
      <img src="../assets/images/illustration-empty-cart.svg" alt="empty cart">
      <p>Your added items will appear here</p>
    </div>
  </div>
</template>

<style scoped>

.cart {
  background-color: #fff;
  padding: 30px;
  height: min-content;
}

.cart-counter {
  color: var(--red);
  margin-bottom: 16px;
}

.product-summary-container {
  margin-bottom: 40px;
}

.total-order {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.total-order__amount {
  font-size: 22px;
  color: var(--rose-900);
  margin-bottom: 26px;
}

.confirm-order {
  display: flex;
  flex-direction: column;
  justify-content: center;
}

.confirm-order > p {
  width: 100%;
  text-align: center;
  padding: 16px;
  font-size: 14px;
  background-color: var(--rose-50);
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 4px;
}

.confirm-order__button {
  margin-top: 20px;
  border: 1px solid var(--red);
  border-radius: 30px;
  font-size: 16px;
  font-weight: bold;
  background-color: #fff;
  color: var(--red);
  padding: 16px;
  cursor: pointer;
  transition: all .1s;
}

.confirm-order__button:hover {
  background-color: var(--red);
  color: #fff;
}

.empty-cart {
  text-align: center;
  color: var(--rose-500);
}

</style>