<template>
  <div>
    <p>Calculating map scale...</p>
  </div>
</template>

<script>
import { onMounted } from 'vue';

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
  setup(props, { emit }) {
    const calculateScale = () => {
      const windowWidthCm = (window.innerWidth / 96) * 2.54;
      const windowHeightCm = (window.innerHeight / 96) * 2.54;

      const widthScale = (props.mapWidth * 1000) / windowWidthCm;
      const heightScale = (props.mapHeight * 1000) / windowHeightCm;
      const scale = Math.min(widthScale, heightScale);

      emit('scaleCalculated', scale, windowHeightCm, windowWidthCm);
    };

    onMounted(() => {
      calculateScale();
    });
  },
};
</script>