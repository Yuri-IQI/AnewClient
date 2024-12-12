<template>
  <div class="drawing-container" @mousedown="startDrawing" @mousemove="draw" @mouseup="stopDrawing">
    <svg ref="svg" width="100%" height="100%">
      <polyline
        v-if="isDrawing"
        :points="temporaryLinePoints"
        stroke="green"
        stroke-width="2"
        fill="none"
      />
      <component
        v-for="(asset, index) in assets"
        :is="asset.component"
        :key="index"
        :x="asset.x"
        :y="asset.y"
        :properties="asset.properties"
      />
    </svg>
  </div>
</template>

<script>
import { ref, computed, defineComponent, onMounted } from 'vue';

export default defineComponent({
  name: 'MapCanvas',
  props: {
    selectedAsset: {
      type: Object, // Expect the actual component or a metadata object describing the asset
      required: true
    }
  },
  setup(props) {
    const svg = ref(null);
    const assets = ref([]); // Changed from reactive([]) to ref([])
    const isDrawing = ref(false);
    const currentPath = ref([]); // Changed from reactive([]) to ref([])
    const startX = ref(0);
    const startY = ref(0);
    const threshold = 10; // Distance threshold to close the loop

    const temporaryLinePoints = computed(() => {
      return currentPath.value.map((point) => `${point.x},${point.y}`).join(' ');
    });

    const startDrawing = (event) => {
      if (props.selectedAsset.name !== 'LandmassAsset') return;

      const rect = svg.value.getBoundingClientRect();
      const x = event.clientX - rect.left;
      const y = event.clientY - rect.top;

      isDrawing.value = true;
      startX.value = x;
      startY.value = y;
      currentPath.value = [{ x, y }];
    };

    const draw = (event) => {
      if (!isDrawing.value) return;

      const rect = svg.value.getBoundingClientRect();
      const x = event.clientX - rect.left;
      const y = event.clientY - rect.top;

      const lastPoint = currentPath.value[currentPath.value.length - 1];
      if (x !== lastPoint.x || y !== lastPoint.y) {
        currentPath.value.push({ x, y });
      }
    };

    const stopDrawing = (event) => {
      if (!isDrawing.value) return;

      const rect = svg.value.getBoundingClientRect();
      const x = event.clientX - rect.left;
      const y = event.clientY - rect.top;

      const distanceToStart = Math.sqrt(
        Math.pow(x - startX.value, 2) + Math.pow(y - startY.value, 2)
      );

      if (distanceToStart <= threshold) {
        // Close the loop and add the landmass
        addAsset([...currentPath.value]);
      }

      isDrawing.value = false;
      currentPath.value = [];
    };

    const addAsset = (path) => {
      assets.value.push({
        component: props.selectedAsset,
        x: 0, // Not used for landmass; pass it as part of properties instead
        y: 0, // Not used for landmass; pass it as part of properties instead
        properties: {
          path
        }
      });
    };

    onMounted(() => {
      svg.value = document.querySelector('svg');
    });

    return {
      svg,
      assets,
      isDrawing,
      currentPath,
      startX,
      startY,
      temporaryLinePoints,
      startDrawing,
      draw,
      stopDrawing
    };
  }
});
</script>

<style scoped>
.drawing-container {
  display: flex;
  justify-content: center;
  align-items: center;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  background-color: #f9f9f9;
  border: 2px solid black;
  border-radius: 10px;
  width: 820px;
  height: 620px;
  overflow: hidden;
}
</style>
