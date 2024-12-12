<template>
  <div>
    <MapSetup
      v-if="!isSetup"
      :worldName="worldName"
      :mapHeight="worldHeight"
      :mapWidth="worldWidth"
      @getWorldSetup="getWorldSetup"
    />

    <MapScaler
      v-if="isSetup && !scaleCalculated"
      :mapHeight="worldHeight"
      :mapWidth="worldWidth"
      @scaleCalculated="handleScaleCalculated"
    />

    <MapMenu
      v-if="scaleCalculated"
      :worldName="worldName"
      :mapHeight="worldHeight"
      :mapWidth="worldWidth"
      :scale="scale"
      :displayHeight="displayHeight"
      :displayWidth="displayWidth"
    />

    <AssetsPalette 
      v-if="scaleCalculated" 
      :selected="selectedAsset" 
      @update:selected="onAssetSelected" 
    />

    <MapCanvas 
      v-if="scaleCalculated" 
      :selectedAsset="selectedAsset" 
    />
  </div>
</template>

<script>
import { ref } from 'vue';
import MapScaler from '../components/MapScaler.vue';
import MapSetup from '../components/MapSetup.vue';
import MapMenu from '../components/MapMenu.vue';
import AssetsPalette from '../components/AssetsPalette.vue';
import MapCanvas from '../components/MapCanvas.vue';
import LandmassAsset from '../components/MapAssets/LandmassAsset.vue';

export default {
  name: 'MapPage',
  components: {
    MapSetup,
    MapScaler,
    MapMenu,
    AssetsPalette,
    MapCanvas
  },
  setup() {
    const worldName = ref('');
    const worldHeight = ref();
    const worldWidth = ref();
    const isSetup = ref(false);
    const scaleCalculated = ref(false);
    const scale = ref(1);
    const displayHeight = ref(0);
    const displayWidth = ref(0);
    const selectedAsset = ref(LandmassAsset);

    const getWorldSetup = (name, height, width) => {
      worldName.value = name;
      worldHeight.value = height;
      worldWidth.value = width;
      isSetup.value = true;
    };

    const handleScaleCalculated = (calculatedScale, dispHeight, dispWidth) => {
      scale.value = calculatedScale;
      displayHeight.value = dispHeight;
      displayWidth.value = dispWidth;
      scaleCalculated.value = true;
    };

    const onAssetSelected = (asset) => {
      selectedAsset.value = asset;
    };

    return {
      worldName,
      worldHeight,
      worldWidth,
      isSetup,
      scaleCalculated,
      scale,
      displayHeight,
      displayWidth,
      selectedAsset,
      getWorldSetup,
      handleScaleCalculated,
      onAssetSelected
    };
  }
};
</script>