<template>
  <div class="slider__btn-container">
    <div class="slider__timelines"></div>
    <div class="slide__prev-next">
      <button class="prev" @click="handlePrevButtonClick">&#10094;</button>
      <button class="next" @click="handleNextButtonClick">&#10095;</button>
    </div>
  </div>
</template>

<script lang="ts">
import "./SliderButtons.scss";
import { defineComponent, watch, watchEffect } from "vue";

export default defineComponent({
  name: "SliderButtons",
  props: {
    products: {
      type: Array,
      required: true,
    },
    currentIndex: {
      type: Number,
      required: true,
    },
  },
  setup(props, { emit }) {
    watch(
      () => props.products.length,
      () => {}
    );

    const HandleBtnClick = (direction: "next" | "prev") => {
      emit("handleBtnClick", direction);
    };

    const handlePrevButtonClick = () => {
      HandleBtnClick("prev");
    };

    const handleNextButtonClick = () => {
      HandleBtnClick("next");
    };

    watchEffect(() => {
      const timer = setTimeout(() => {
        HandleBtnClick("next");
      }, 3000);

      return () => clearTimeout(timer);
    });

    return {
      handlePrevButtonClick,
      handleNextButtonClick,
    };
  },
});
</script>
