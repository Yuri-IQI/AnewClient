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
      // Get window dimensions in centimeters (assuming 96 pixels per inch and 2.54 cm per inch)
      const windowWidthCm = (window.innerWidth / 96) * 2.54;
      const windowHeightCm = (window.innerHeight / 96) * 2.54;

      // Calculate scale: real size in km divided by display size in cm
      const widthScale = this.mapWidth * 1000 / windowWidthCm;
      const heightScale = this.mapHeight * 1000 / windowHeightCm;
      const scale = Math.min(widthScale, heightScale);

      // Emit the calculated scale and display dimensions to the parent
      this.$emit('scaleCalculated', scale, windowHeightCm, windowWidthCm);
    },
  },
};
</script>