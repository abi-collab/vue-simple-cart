<template>
  <header class="bg-gradient-to-r from-cyan-500 to-blue-500 py-4 px-4 fixed w-full">
    <section class="flex justify-between items-center">
      <div class="flex container mx-auto items-center">
        <img :src="img" alt="logo" width="48" height="48">
        <h1 class="text-white font-bold">Vue JS 3 Simple Cart</h1>
      </div>
      <h1 class="text-white">Cart({{ cartItems.length }})</h1>
    </section>
  </header>
  <ProductList :member="isMember" @add-to-cart="addToCart"/>
</template>

<script setup>
import {ref} from "vue";
import ProductList from "@/ProductList.vue";

const img = '../src/assets/logo.svg'
const isMember = ref(false);
const cartItems = ref([]);

function addToCart(id) {
  cartItems.value.push(id)
}

</script>
