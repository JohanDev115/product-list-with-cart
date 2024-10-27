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

console.log(product)
</script>

<template>
  <div class="product-summary">
    <img :src="product.image.thumbnail" width="50" height="50px" alt="">
    <div>
      <p class="product-summary__name">{{ product.name }}</p>
      <b class="product-summary__quantity">{{ product.quantity }}x</b>
      <span class="product-summary__i-price">@ {{ formatedPrice(product.price) }}</span>
    </div>
    <span class="product-summary__t-price">{{ formatedPrice(totalPrice) }}</span>
  </div>
</template>

<style scoped>

.product-summary {
  display: grid;
  grid-template-columns: 60px 1fr auto;
  padding: 14px;
  border-bottom: 1px solid var(--rose-100);
}

.product-summary img {
  border-radius: 10px;
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
  font-size: 18px;
  font-weight: bold;
  display: block;
  align-self: center;
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