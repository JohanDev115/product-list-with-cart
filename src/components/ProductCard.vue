<script setup>
import { computed, reactive } from "vue"

const emit = defineEmits(['submitProduct'])

const props = defineProps({
  product: Object,
  quantity: Number,
})

const product = reactive(props.product);

const addProducttoCart = () => {
  product.quantity++;
  emit('submitProduct', product)
}

const removeProductfromCart = () => {
  product.quantity--;
  emit('submitProduct', product)
}

const formatedPrice = computed(() => { return new Intl.NumberFormat('en-US', { style: 'currency', currency: 'USD', currencySign: "accounting", }).format(
    product.price)});

</script>

<template>
  <div class="product-card">
    <div class="product-image">
      <img :src="product.image.mobile" :alt="product.name">
      <button class="product-card__button">
        <span v-if="product?.quantity == 0" class="addtocart-button" @click="addProducttoCart">
          <span>
            <img width="16" height="16" src="../assets/images/icon-add-to-cart.svg" alt="add to cart">
          </span>
          Add to Cart
        </span>
        <span v-if="product?.quantity > 0" class="quantity-controller">
          <button class="quantity-icon" @click="removeProductfromCart">
            <img width="10" height="10" src="../assets/images/icon-decrement-quantity.svg" alt="decrement quantity">
          </button>
          {{ product?.quantity }}
          <span class="quantity-icon" @click="addProducttoCart">
            <img width="10" height="10" src="../assets/images/icon-increment-quantity.svg" alt="increment quantity">
          </span>
        </span>
      </button>
    </div>
    <div class="product-description">
      <span class="product-description__type">{{ product.category }}</span>
      <h3 class="product-description__name">{{ product.name }}</h3>
      <span class="product-description__price">{{ formatedPrice }}</span>
    </div>
  </div>
</template>

<style scoped>

.product-card {
  width: 240px;
  height: 260px;
  margin-bottom: 20px;
}

.product-image {
  position: relative;
  width: 100%;
  border-radius: 16px;
  margin-bottom: 24px;
}

.product-image > img {
  width: 100%;
  object-fit: cover;
  border-radius: 16px;
}

.product-card__button {
  position: absolute;
  width: 150px;
  height: 35px;
  left: 50%;
  bottom: -9%;
  transform: translateX(-50%);
  border-radius: 20px;
  font-size: 14px;
  font-weight: 600;
  border: none;
}

.addtocart-button, .quantity-controller {
  width: 100%;
  height: 100%;
  display: flex;
  align-items: center;
  border-radius: 20px;
}

.addtocart-button {
  justify-content: center;
  gap: 6px;
  background-color: #fff;
  border: 1px solid #555;
  cursor: pointer;
}

.quantity-controller {
  background-color: var(--red);
  justify-content: space-around;
  color: #fff;
}

.quantity-icon {
  border: 1px solid #fff;
  display: flex;
  justify-content: center;
  align-items: center;
  border-radius: 100%;
  padding: 5px;
  cursor: pointer;
  background-color: transparent;
}

.quantity-icon:hover {
  background-color: #fff;
}

.quantity-icon:hover > img {
  filter: invert(100%);
}

.product-description__type {
  font-size: 14px;
  color: var(--rose-500);
  margin-bottom: 12px;
}

.product-description__name {
  color: var(--rose-900);
  margin-bottom: 4px;
}

.product-description__price {
  color: var(--red);
  font-weight: 600;
}

</style>