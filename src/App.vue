<template>
  <div class="app">
    <template v-if="isLoading">
      <div class="spinner-container">
        <div class="spinner"></div>
      </div>
    </template>
    <template v-else>
      <Slider :products="products" />
    </template>
  </div>
</template>

<script lang="ts">
import "./App.scss";
import { defineComponent, ref, onMounted } from "vue";
import Slider from "./pages/Slider.vue";
import { Product } from "./types/Product";
import { fetchData } from "./components/fetchData/FetchData";

export default defineComponent({
  name: "App",
  components: {
    Slider,
  },
  setup() {
    const products = ref<Product[]>([]);
    const isLoading = ref(true);

    const initializeSlider = async () => {
      try {
        const data = await fetchData("https://dummyjson.com/products", 5);
        products.value = data;
      } catch (error) {
        console.error("fetching data error:", error);
      } finally {
        isLoading.value = false;
      }
    };

    onMounted(() => {
      initializeSlider();
    });

    return {
      products,
      isLoading,
    };
  },
});
</script>
