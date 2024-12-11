<template>
  <g>
    <polygon
      :points="polygonPoints"
      :fill="color"
      stroke="green"
      stroke-width="2"
    />
  </g>
</template>

<script>
import { defineComponent, computed } from 'vue';

export default defineComponent({
  name: 'LandmassAsset',
  props: {
    properties: {
      type: Object,
      required: true
    }
  },
  setup(props) {
    const polygonPoints = computed(() => {
      return props.properties.path.map((point) => `${point.x},${point.y}`).join(" ");
    });

    const color = computed(() => {
      return props.properties.color || "lightgreen";
    });

    return {
      polygonPoints,
      color
    };
  }
});
</script>