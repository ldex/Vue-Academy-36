<template>
    <section v-if="errorMessage">
      {{errorMessage}}
    </section>
    <section v-else>
      <div v-if="isLoading">
        <div class="loader">Loading products...</div>
      </div>
      <div v-else>
        <product-list :products="products" :page-size="5">
          <template v-slot="slotProps">
                <span>{{ slotProps.product.name }}</span>
          </template>
        </product-list>
      </div>
    </section>
</template>

<script setup>
import { ref, onErrorCaptured } from 'vue'
import ProductList from '@/components/ProductList.vue';
import ProductService from '@/services/ProductService.js';

const products = ref([]);
const errorMessage = ref(null);
const isLoading = ref(false);

onErrorCaptured((error) => {
  console.error('Error in component: ', error.message);
  return true;
});

isLoading.value = true;
ProductService.getProducts()
      .then(data => products.value = data)
      .catch(error => {
        errorMessage.value = 'There was an error getting products from server, ' + error;
      })
      .finally(() => isLoading.value = false);
</script>

<style lang="scss" scoped>

</style>