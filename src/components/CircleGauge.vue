<template>
  <div class="circle-gauge">
    <svg :width="size" :height="size" xmlns="http://www.w3.org/2000/svg">
      <!-- Create the circle background -->
      <circle
        :cx="size / 2"
        :cy="size / 2"
        :r="size / 2 - strokeWidth / 2"
        :stroke="backgroundColor"
        :stroke-width="strokeWidth"
        fill="none"
      />

      <!-- Create the intervals -->
      <path
        v-for="(interval, index) in intervals"
        :key="index"
        :d="getIntervalPath(interval, index)"
        :fill="interval.color"
      />
    </svg>
  </div>
</template>

<script lang="ts">
import { defineComponent, PropType } from "vue";

interface IntervalItem {
  value: number;
  color: string;
}

export default defineComponent({
  props: {
    size: {
      type: Number,
      required: true,
    },
    strokeWidth: {
      type: Number,
      default: 10,
    },
    backgroundColor: {
      type: String,
      default: "lightgray",
    },
    intervals: {
      type: Array as PropType<IntervalItem[]>,
      required: true,
    },
  },
  methods: {
    getIntervalPath(interval: IntervalItem, index: number): string {
      const radius = this.size / 2 - this.strokeWidth / 2;
      const startAngle =
        index === 0
          ? -Math.PI / 2
          : (this.intervals[index - 1].value * Math.PI) / 50 - Math.PI / 2;
      const endAngle = (interval.value * Math.PI) / 50 - Math.PI / 2;

      const x1 = this.size / 2 + radius * Math.cos(startAngle);
      const y1 = this.size / 2 + radius * Math.sin(startAngle);

      const x2 = this.size / 2 + radius * Math.cos(endAngle);
      const y2 = this.size / 2 + radius * Math.sin(endAngle);

      const largeArcFlag = endAngle - startAngle <= Math.PI ? 0 : 1;

      return `M ${this.size / 2},${
        this.size / 2
      } L ${x1},${y1} A ${radius},${radius} 0 ${largeArcFlag},1 ${x2},${y2} Z`;
    },
  },
});
</script>

<style scoped>
.circle-gauge {
  display: inline-block;
}
</style>
