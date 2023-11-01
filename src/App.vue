<template>
  <div>
    <circle-gauge :intervals="intervals" :size="200" :value="75" />
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import CircleGauge from "@/components/CircleGauge.vue";

export default defineComponent({
  components: {
    CircleGauge,
  },
  data() {
    return {
      intervals: [
        { value: 0, color: "#E1CA46" },
        { value: 50, color: "#75C952" },
        { value: 100, color: "#469ED5" },
      ],
    };
  },
});
</script>
