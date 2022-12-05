<template>
  <div class="py-16 container mx-auto px-4">
    <div class="lg:flex gap-x-16 pt-16">
      <img :src="image" alt="" class="w-96 h-96 ">

      <div class="items-center space-y-4 pt-8">
        <h1 class="text-2xl font-bold">{{ name }}</h1>

        <div>
          <p v-if="inStock >= 5">In Stock ({{ inStock }})</p>
          <p v-else-if="inStock <= 5 && inStock !== 0">Few Stocks Left!</p>
          <p v-else>Out Of Stock</p>
        </div>

        <div class="">
          <p>{{ products.description }}</p>
          <p v-for="detail in products.details">{{ detail }}</p>
        </div>

        <div class="flex gap-x-4 items-center">
          <h1>Color:</h1>
          <div v-for="(variant, index) in products.variants"
               :key="variant.id"
               @mouseover="updateVariant(index)"
               class="">
            <div class="cursor-pointer rounded-full border border-2 border-slate-500 w-10 h-10"
                 :class="variant.color==='red'
                 ? `bg-${variant.color}-500`
                 : '' "></div>
          </div>
        </div>

        <div>
          <p class="text-xs">{{ shipping }}</p>
        </div>
        <div>
          <button :disabled="!inStock"
                  @click="addToCart"
                  :class="[inStock
                  ? 'bg-gradient-to-r from-cyan-500 to-blue-500 cursor-pointer'
                  : 'bg-gray-400 cursor-not-allowed' ]"
                  class="rounded-md px-4 py-2 text-white ">
            Add to cart
          </button>
        </div>
      </div>
    </div>
  </div>
</template>
<script setup>
import {computed, ref} from "vue";

const props = defineProps({
      premium: {
        type: Boolean,
        required: true,
      }
    }
)

const emit = defineEmits(['add-to-cart']);

const products = ref(
    {
      id: 1,
      brand: 'NIKO',
      name: 'Simple T-shirt',
      image: './src/assets/img/red.png',
      selectedVariant: 0,
      description: 'Lorem ipsum dolor sit amet, consectetur adipisicing elit. Accusantium ad culpa dolorum eum temporibus. Blanditiis culpa iste magnam reiciendis sint?',
      details: ['79% cotton', '21% wool'],
      variants: [
        {id: 2, color: 'red', image: './src/assets/img/red.png', quantity: 10},
        {id: 3, color: 'white', image: './src/assets/img/white.png', quantity: 5},
      ]
    }
)

const name = computed(() => {
  return products.value.brand + ' ' + products.value.name;
})

const image = computed(() => {
  return products.value.variants[products.value.selectedVariant].image;
})

const inStock = computed(() => {
  return products.value.variants[products.value.selectedVariant].quantity;
})

const shipping = computed(() => {
  if (props.premium) {
    return 'Free Shipping Worldwide'
  } else {
    return '$5 Shipping Cost Worldwide';
  }
})

function addToCart() {
  if (inStock) {
    products.value.variants[products.value.selectedVariant].quantity--;
    emit('add-to-cart', products.value.variants[products.value.selectedVariant].id)
  } else {
    return false;
  }
}

function updateVariant(index) {
  products.value.selectedVariant = index;
}


</script>
