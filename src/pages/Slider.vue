<template>
  <div class="slider">
    <div class="slider__overlay"></div>
    <div
      class="slider__slides"
      @touchstart="handleTouchStart"
      @touchmove="handleTouchMove"
      @touchend="handleTouchEnd"
    >
      <SlideImage
        v-for="(product, index) in products"
        :key="product.id"
        :src="product.thumbnail"
        :alt="product.title"
        :isActive="index === currentIndex"
      />
    </div>
    <SlideContent :products="products" :currentIndex="currentIndex" />
    <SliderButtons
      :products="products"
      :currentIndex="currentIndex"
      @handleBtnClick="handleBtnClick"
    />
  </div>
</template>

<script lang="ts">
import "./Slider.scss";
import { defineComponent, ref, watch, onUnmounted } from "vue";
import SlideImage from "../components/SlideImage/SlideImage.vue";
import SlideContent from "../components/SlideContent/SlideContent.vue";
import SliderButtons from "../components/SliderButtons/SliderButtons.vue";
import { Product } from "../types/Product";

export default defineComponent({
  name: "Slider",
  components: {
    SlideImage,
    SlideContent,
    SliderButtons,
  },
  props: {
    products: {
      type: Array as () => Product[],
      required: true,
    },
  },
  setup(props) {
    const currentIndex = ref(0);
    let intervalId: ReturnType<typeof setInterval> | null = null;
    const touchStartX = ref<number>(0);
    const touchEndX = ref<number>(0);

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

    const prevSlide = () => {
      currentIndex.value =
        (currentIndex.value - 1 + props.products.length) %
        props.products.length;
    };

    const handleBtnClick = (direction: "next" | "prev") => {
      stopInterval();
      direction === "next" ? nextSlide() : prevSlide();
      startInterval();
    };

    const handleTouchStart = (event: TouchEvent) => {
      touchStartX.value = event.touches[0].clientX;
    };

    const handleTouchMove = (event: TouchEvent) => {
      touchEndX.value = event.touches[0].clientX;
    };

    const handleTouchEnd = () => {
      const swipeThreshold = 50;
      const touchDiff = touchEndX.value - touchStartX.value;
      if (touchDiff > swipeThreshold) {
        prevSlide();
      } else if (touchDiff < -swipeThreshold) {
        nextSlide();
      }
      startInterval();
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
      handleBtnClick,
      handleTouchStart,
      handleTouchMove,
      handleTouchEnd,
    };
  },
});
</script>
