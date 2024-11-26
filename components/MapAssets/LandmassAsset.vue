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
export default {
  name: 'LandmassAsset',
  props: {
    properties: {
      type: Object,
      required: true
    }
  },
  computed: {
    polygonPoints() {
      return this.properties.path.map((point) => `${point.x},${point.y}`).join(" ");
    },
    color() {
      return this.properties.color || "lightgreen";
    }
  }
};
</script>