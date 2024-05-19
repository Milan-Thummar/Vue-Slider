<template>
  <div class="slider__btn-container">
    <div class="slider__timelines">
      <div
        v-for="(_, index) in products"
        :key="index"
        class="slider__timeline"
        :style="{ width: timelineWidth + '%' }"
      >
        <div
          class="slider__timeline-fill"
          :style="{
            transitionDuration: index === currentIndex ? '3s' : '0s',
            width: index === currentIndex ? '100%' : '0',
          }"
        ></div>
      </div>
    </div>
    <div class="slide__prev-next">
      <button class="prev" @click="handlePrevButtonClick">&#10094;</button>
      <button class="next" @click="handleNextButtonClick">&#10095;</button>
    </div>
  </div>
</template>

<script lang="ts">
import "./SliderButtons.scss";
import {
  defineComponent,
  ref,
  watch,
  onMounted,
  onUnmounted,
  watchEffect,
} from "vue";

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
    const timelineWidth = ref(0);

    const calculateLineWidth = () => {
      const slidesCount = props.products.length;
      let width = 100 / slidesCount;
      if (window.innerWidth >= 480 && width > 20) {
        width = 18;
      }
      timelineWidth.value = width;
    };

    const updateLineWidth = () => {
      const timeLines =
        document.querySelectorAll<HTMLElement>(".slider__timeline");
      timeLines.forEach((timeLine) => {
        timeLine.style.width = `${timelineWidth.value}%`;
      });
    };

    const handleResize = () => {
      calculateLineWidth();
      updateLineWidth();
    };

    onMounted(() => {
      calculateLineWidth();
      updateLineWidth();
      window.addEventListener("resize", handleResize);
    });

    onUnmounted(() => {
      window.removeEventListener("resize", handleResize);
    });

    watch(
      () => props.products.length,
      () => {
        calculateLineWidth();
        updateLineWidth();
      }
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
      timelineWidth,
      handlePrevButtonClick,
      handleNextButtonClick,
    };
  },
});
</script>
