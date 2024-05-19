<template>
  <div class="slider">
    <div class="slider__overlay"></div>
    <div class="slider__slides">
      <SlideImage
        v-for="(product, index) in products"
        :key="product.id"
        :src="product.thumbnail"
        :alt="product.title"
        :isActive="index === currentIndex"
      />
    </div>
    <SlideContent :products="products" :currentIndex="currentIndex" />
  </div>
</template>

<script lang="ts">
import "./Slider.scss";
import { defineComponent, ref, watch, onUnmounted } from "vue";
import SlideImage from "../components/SlideImage/SlideImage.vue";
import SlideContent from "../components/SlideContent/SlideContent.vue";
import { Product } from "../types/Product";

export default defineComponent({
  name: "Slider",
  components: { SlideImage, SlideContent },
  props: {
    products: {
      type: Array as () => Product[],
      required: true,
    },
  },
  setup(props) {
    const currentIndex = ref(0);
    let intervalId: ReturnType<typeof setInterval> | null = null;

    const startInterval = () => {
      stopInterval();
      intervalId = setInterval(nextSlide, 3000);
    };

    const stopInterval = () => {
      if (intervalId) {
        clearInterval(intervalId);
      }
    };

    const nextSlide = () => {
      currentIndex.value = (currentIndex.value + 1) % props.products.length;
    };

    watch(
      () => props.products,
      (newProducts) => {
        if (newProducts.length > 0) {
          startInterval();
        }
      }
    );

    onUnmounted(() => {
      stopInterval();
    });

    return {
      currentIndex,
    };
  },
});
</script>