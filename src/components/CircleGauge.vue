<template>
  <div class="circle-gauge">
    <svg :width="size" :height="size" viewBox="0 0 100 100">
      <circle
        cx="50"
        cy="50"
        :r="radius"
        fill="transparent"
        stroke="#ccc"
        stroke-width="10"
      />
      <circle
        cx="50"
        cy="50"
        :r="radius"
        fill="transparent"
        :stroke="getGaugeColor(100)"
        stroke-width="10"
        :stroke-dasharray="dashArray"
        :stroke-dashoffset="dashOffset"
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
    intervals: {
      type: Array as PropType<IntervalItem[]>,
      required: true,
    },
    size: {
      type: Number,
      default: 200,
    },
    value: {
      type: Number,
      required: true,
    },
  },
  computed: {
    radius(): number {
      return (this.size - 10) / 2;
    },
    dashArray(): number {
      return 2 * Math.PI * this.radius;
    },
    dashOffset(): number {
      const percentage = (this.value / 100) * this.dashArray;
      return this.dashArray - percentage;
    },
  },
  methods: {
    getGaugeColor(val: number): string {
      for (const interval of this.intervals) {
        if (val <= interval.value) {
          return interval.color;
        }
      }
      return this.intervals[this.intervals.length - 1].color;
    },
  },
});
</script>

<style scoped>
.circle-gauge {
  display: inline-block;
}
</style>
