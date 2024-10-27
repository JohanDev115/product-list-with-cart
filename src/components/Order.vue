<script setup>
import { computed } from "vue"
import ProductSummaryOrder from "./ProductSummaryOrder.vue";

const emit = defineEmits(['newOrder'])

const props = defineProps({
  cart: Object
})

const cart = props.cart;
const totalPrice = computed(() => cart.map(el => el.quantity * el.price).reduce((total, currentvalue) => total + currentvalue, 0))
const formatedPrice = (price) => { return new Intl.NumberFormat('en-US', { style: 'currency', currency: 'USD', currencySign: "accounting", }).format(price)};

const newOrder = () => emit('newOrder');

</script>

<template>
  <div class="overlay">
    <div class="order">
      <img src="../assets/images/icon-order-confirmed.svg" alt="order_confirmed">
      <h2 class="order__title">Order Confirmed</h2>
      <p class="order__subtitle">We hope you enjoy your food!</p>
      <div>
        <div class="order-summary-container">
          <ProductSummaryOrder v-for="product in cart" :key="product" :product="product"  />
        </div>
        <div class="total-order">
          <p>Order Total</p>
          <b class="total-order__amount">{{ formatedPrice(totalPrice) }}</b>
        </div>
        <button class="purchase-button" @click="newOrder">Start New Order</button>
      </div>
    </div>
  </div>
</template>

<style scoped>
.overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5); /* Color negro con transparencia */
  z-index: 999;
  display: flex;
  justify-content: center;
  align-items: center;
  overflow: hidden;
  padding: 20px;
}

.order {
  width: 100%;
  max-width: 580px;
  max-height: 90vh;
  background-color: #fff;
  padding: 30px;
  height: min-content;
  border-radius: 30px;
  overflow-y: auto;
}

.order__title {
  font-size: 34px;
  color: var(--rose-900);
  margin-top: 16px;
}

.order__subtitle {
  margin-bottom: 30px;
  color: var(--rose-900);
}

.order-summary-container {
  margin-bottom: 40px;
  display: flex;
  justify-content: center;
  flex-direction: column;
  background-color: var(--rose-50);
}

.total-order {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 26px;
}

.total-order__amount {
  font-size: 22px;
  color: var(--rose-900);
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

.purchase-button {
  margin-top: 20px;
  width: 100%;
  border: none;
  border-radius: 30px;
  font-size: 16px;
  background-color: var(--red);
  color: #fff;
  padding: 14px;
  cursor: pointer;
  transition: all .1s;
}

.confirm-order__button:hover {
  filter: brightness(50%);
}

.empty-cart {
  text-align: center;
  color: var(--rose-500);
}

@media (max-width: 375px) {
  .order {
    position: absolute;
    bottom: 0;
    border-bottom-left-radius: 0%;
    border-bottom-right-radius: 0%;
  }
}

</style>