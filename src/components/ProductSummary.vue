<script setup>
import { reactive, ref, watch } from "vue"

const emit = defineEmits(['removeProduct'])

const props = defineProps({
  product: Object,
})

const product = reactive(props.product);
let totalPrice = product.price * product.quantity;

watch(product, () => {
  totalPrice = product.price * product.quantity;
})

const formatedPrice = (price) => { return new Intl.NumberFormat('en-US', { style: 'currency', currency:'USD', currencyDisplay: 'symbol' }).format(price)};

const removeItem = () => emit('removeProduct', product)
</script>

<template>
  <div class="product-summary">
    <div>
      <p class="product-summary__name">{{ product.name }}</p>
      <b class="product-summary__quantity">{{ product.quantity }}x</b>
      <span class="product-summary__i-price">@ {{ formatedPrice(product.price) }}</span>
      <span class="product-summary__t-price">{{ formatedPrice(totalPrice) }}</span>
    </div>
    <span class="product-summary__remove-btn" @click="removeItem">
      <img width="14" height="14" src="../assets/images/icon-remove-item.svg" alt="remove item">
    </span>
  </div>
</template>

<style scoped>

.product-summary {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 26px;
}

.product-summary__name {
  color: var(--rose-900);
  font-weight: bold;
  margin-bottom: 8px;
}

.product-summary__quantity {
  color: var(--red);
  margin-right: 16px;
}

.product-summary__i-price {
  color: var(--rose-400);
  margin-right: 8px;
}

.product-summary__t-price {
  color: var(--rose-500);
  font-weight: bold;
}

.product-summary__remove-btn {
  border: 1px solid var(--rose-400);
  display: flex;
  justify-content: center;
  align-items: center;
  border-radius: 100%;
  padding: 5px;
  cursor: pointer;
  background-color: transparent;
}

</style>