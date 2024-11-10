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
    
    <MapHeader
      v-if="scaleCalculated"
      :worldName="worldName"
      :mapHeight="worldHeight"
      :mapWidth="worldWidth"
      :scale="scale"
      :displayHeight="displayHeight"
      :displayWidth="displayWidth"
    />
  </div>
</template>

<script setup>
import { ref } from 'vue';
import MapSetup from '../components/MapSetup.vue';
import MapScaler from '../components/MapScaler.vue';
import MapHeader from '../components/MapHeader.vue';

const worldName = ref('');
const worldHeight = ref(null);
const worldWidth = ref(null);
const isSetup = ref(false);
const scaleCalculated = ref(false);
const scale = ref(1);
const displayHeight = ref(0);
const displayWidth = ref(0);

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
</script>