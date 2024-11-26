<template>
  <div>
    <MenuToggler id="toggle-palette" :initialState="showMenu" @toggle-menu="handleToggleMenu" />

    <div id="assets-palette" :class="{ 'menu-hidden': !showMenu }">
      <div 
        v-for="asset in assets" 
        :key="asset.name"
        :class="{ selected: selectedAsset === asset.name }"
        @click="selectAsset(asset.name)"
      >
        {{ asset.name }}
      </div>
    </div>
  </div>
</template>

<script>
import MenuToggler from './MenuToggler.vue';
import LandmassAsset from './MapAssets/LandmassAsset.vue'
import RiverAsset from './MapAssets/RiverAsset.vue'
import CityAsset from './MapAssets/CityAsset.vue'
import LandRouteAsset from './MapAssets/LandRouteAsset.vue'

export default {
  name: 'AssetsPalette',
  components: {
    MenuToggler
  },
  props: {
    selected: { type: Object, required: true },
    onMenu: { type: Boolean, default: false }
  },
  data() {
    return {
      showMenu: this.onMenu,
      assets: [
        {name: 'Landmass', component: LandmassAsset},
        {name: 'River', component: RiverAsset},
        {name: 'City', component: CityAsset},
        {name: 'Land Route', component: LandRouteAsset}
      ],
      selectedAsset: this.selected || LandmassAsset
    };
  },
  methods: {
    selectAsset(assetName) {
      this.selectedAsset = assetName;
      const selected = this.assets.find(asset => asset.name === assetName);
      this.$emit('update:selected', selected.component);
    },
    handleToggleMenu(isVisible) {
      this.showMenu = isVisible;
    }
  }
};
</script>

<style scoped>
#assets-palette {
  position: fixed;
  right: 0.5em;
  top: 50%;
  transform: translateY(-50%);
  background-color: antiquewhite;
  padding: 1em;
  border-radius: 8px;
  border: 2px solid black;
  display: flex;
  flex-direction: column;
  gap: 1em;
  width: 10%;
  transition: transform 0.3s ease;
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