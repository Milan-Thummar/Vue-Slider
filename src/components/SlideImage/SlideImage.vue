<template>
  <img
    :src="src"
    :alt="alt"
    :style="{ display: isActive ? 'block' : 'none' }"
  />
</template>

<script lang="ts">
import "./SlideImage.scss";
import { defineComponent } from "vue";

export default defineComponent({
  name: "SlideImage",
  props: {
    src: {
      type: String,
      required: true,
    },
    alt: {
      type: String,
      required: true,
    },
    isActive: {
      type: Boolean,
      required: true,
    },
  },
});
</script>
