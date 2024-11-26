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
export default {
  name: 'MapCanvas',
  props: {
    selectedAsset: {
      type: Object, // Expect the actual component or a metadata object describing the asset
      required: true
    }
  },
  data() {
    return {
      assets: [], // Holds all the instantiated assets on the map
      isDrawing: false,
      currentPath: [], // Tracks the points of the current drawing path
      startX: 0,
      startY: 0,
      threshold: 10 // Distance threshold to close the loop
    };
  },
  computed: {
    temporaryLinePoints() {
      return this.currentPath.map((point) => `${point.x},${point.y}`).join(" ");
    }
  },
  methods: {
    startDrawing(event) {
      if (this.selectedAsset.name !== "LandmassAsset") return;

      const rect = this.$refs.svg.getBoundingClientRect();
      const x = event.clientX - rect.left;
      const y = event.clientY - rect.top;

      this.isDrawing = true;
      this.startX = x;
      this.startY = y;
      this.currentPath = [{ x, y }];
    },
    draw(event) {
      if (!this.isDrawing) return;

      const rect = this.$refs.svg.getBoundingClientRect();
      const x = event.clientX - rect.left;
      const y = event.clientY - rect.top;

      const lastPoint = this.currentPath[this.currentPath.length - 1];
      if (x !== lastPoint.x || y !== lastPoint.y) {
        this.currentPath.push({ x, y });
      }
    },
    stopDrawing(event) {
      if (!this.isDrawing) return;

      const rect = this.$refs.svg.getBoundingClientRect();
      const x = event.clientX - rect.left;
      const y = event.clientY - rect.top;

      const distanceToStart = Math.sqrt(
        Math.pow(x - this.startX, 2) + Math.pow(y - this.startY, 2)
      );

      if (distanceToStart <= this.threshold) {
        // Close the loop and add the landmass
        this.addAsset(this.currentPath);
      }

      this.isDrawing = false;
      this.currentPath = [];
    },
    addAsset(path) {
      this.assets.push({
        component: this.selectedAsset,
        x: 0, // Not used for landmass; pass it as part of properties instead
        y: 0, // Not used for landmass; pass it as part of properties instead
        properties: {
          path
        }
      });
    }
  }
};
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