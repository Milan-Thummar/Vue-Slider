<template>
  <div class="slide__content">
    <div
      v-for="(product, index) in products"
      :key="product.id"
      :style="{ display: index === currentIndex ? 'block' : 'none' }"
    >
      <h1>{{ product.title }}</h1>
      <p>{{ product.description }}</p>
      <button class="more-info">More Info</button>
      <button class="contact">Contact</button>
    </div>
  </div>
</template>

<script lang="ts">
import "./SlideContent.scss";
import { defineComponent } from "vue";
import { Product } from "../../types/Product";

export default defineComponent({
  name: "SlideContent",
  props: {
    products: {
      type: Array as () => Product[],
      required: true,
    },
    currentIndex: {
      type: Number,
      required: true,
    },
  },
});
</script>
