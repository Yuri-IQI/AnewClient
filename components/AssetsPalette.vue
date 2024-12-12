<template>
  <div :class="{ 'unseen': !showMenu }">
    <MenuToggler id="toggle-palette" :initialState="showMenu" @toggle-menu="handleToggleMenu" />

    <div id="assets-palette" :class="{ 'menu-hidden': !showMenu }">
      <div 
        v-for="asset in assets" 
        :key="asset.name"
        :class="{ selected: selectedAsset.value === asset.name }"
        @click="selectAsset(asset.name)"
      >
        {{ asset.name }}
      </div>
    </div>
  </div>
</template>

<script>
import { ref, computed, watch, defineComponent } from 'vue';
import MenuToggler from './MenuToggler.vue';
import LandmassAsset from './MapAssets/LandmassAsset.vue';
import RiverAsset from './MapAssets/RiverAsset.vue';
import CityAsset from './MapAssets/CityAsset.vue';
import LandRouteAsset from './MapAssets/LandRouteAsset.vue';

export default defineComponent({
  name: 'AssetsPalette',
  components: { MenuToggler },
  props: {
    selected: { type: Object, required: true },
    onMenu: { type: Boolean, default: false }
  },
  setup(props, { emit }) {
    const showMenu = ref(props.onMenu);
    const selectedAsset = ref(props.selected || 'Landmass');
    const assets = ref([
      { name: 'Landmass', component: LandmassAsset },
      { name: 'River', component: RiverAsset },
      { name: 'City', component: CityAsset },
      { name: 'Land Route', component: LandRouteAsset }
    ]);

    const selectAsset = (assetName) => {
      selectedAsset.value = assetName;
      const selected = assets.value.find(asset => asset.name === assetName);
      emit('update:selected', selected.component);
    };

    const handleToggleMenu = (isVisible) => {
      showMenu.value = isVisible;
    };

    watch(() => props.selected, (newSelected) => {
      selectedAsset.value = newSelected;
    });

    return {
      showMenu,
      selectedAsset,
      assets,
      selectAsset,
      handleToggleMenu
    };
  }
});
</script>

<style scoped>
#assets-palette {
	background-color: antiquewhite;
	border-radius: 8px;
	border: 2px solid black;
	display: flex;
	flex-direction: column;
	gap: 1em;
	transition: transform 0.3s ease;
	padding: 1em;
	margin: 1em;
}

.menu-hidden {
  transform: translate(140%, -190%) !important;
}

#assets-palette div {
  cursor: pointer;
  padding: 0.5em;
  text-align: center;
  border-radius: 4px;
  transition: background-color 0.3s ease;
}

#assets-palette div.selected {
  background-color: lightgreen;
  font-weight: bold;
}

#toggle-palette {
  right: 0em;
  position: absolute;
  transform: rotate(90deg);
  transform-origin: center;
  top: 0em;
}
</style>