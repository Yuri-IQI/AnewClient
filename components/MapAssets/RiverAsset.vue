<template>
  <g>
    <polygon
      :points="polygonPoints"
      :fill="color"
      stroke="blue"
      stroke-width="2"
    />
  </g>
</template>

<script>
export default {
  name: 'RiverAsset',
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
      return this.properties.color || "lightblue";
    }
  }
};
</script>