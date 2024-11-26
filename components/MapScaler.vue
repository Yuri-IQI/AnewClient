<template>
  <div>
    <p>Calculating map scale...</p>
  </div>
</template>

<script>
export default {
  name: 'MapScaler',
  props: {
    mapHeight: {
      type: Number,
      required: true,
    },
    mapWidth: {
      type: Number,
      required: true,
    },
  },
  mounted() {
    this.calculateScale();
  },
  methods: {
    calculateScale() {
      const windowWidthCm = (window.innerWidth / 96) * 2.54;
      const windowHeightCm = (window.innerHeight / 96) * 2.54;

      const widthScale = this.mapWidth * 1000 / windowWidthCm;
      const heightScale = this.mapHeight * 1000 / windowHeightCm;
      const scale = Math.min(widthScale, heightScale);

      this.$emit('scaleCalculated', scale, windowHeightCm, windowWidthCm);
    },
  },
};
</script>