<template>
  <div>
    <MenuToggler id="toggle-info" :initialState="showMenu" @toggle-menu="handleToggleMenu" />

    <div id="data-menu" :class="{ 'menu-hidden': !showMenu }">
      <h1>{{ worldName }}</h1>
      <div class="exhibit-sizes">
        <h3>Real Sizes</h3>
        <span>
          <p>Height: {{ mapHeight.toFixed(2) }} Km</p>
          <p>Width: {{ mapWidth.toFixed(2) }} Km</p>
        </span>
      </div>
      <h3>Scale: 1 cm = {{ scale.toFixed(2) }} meters</h3>
      <div class="exhibit-sizes">
        <h3>Display Sizes</h3>  
        <span>
          <p>Height: {{ displayHeight.toFixed(2) }} cm</p>
          <p>Width: {{ displayWidth.toFixed(2) }} cm</p>  
        </span>
      </div>
    </div>
  </div>
</template>

<script>
import MenuToggler from './MenuToggler.vue';

export default {
  name: 'MapMenu',
  components: {
    MenuToggler
  },
  props: {
    worldName: { type: String, required: true },
    mapHeight: { type: Number, required: true },
    mapWidth: { type: Number, required: true },
    scale: { type: Number, required: true },
    displayHeight: { type: Number, required: true },
    displayWidth: { type: Number, required: true },
    onMenu: { type: Boolean, default: false }
  },
  data() {
    return {
      showMenu: this.onMenu
    };
  },
  methods: {
    handleToggleMenu(isVisible) {
      this.showMenu = isVisible;
    }
  }
};
</script>

<style scoped>
#data-menu {
  display: flex;
  flex-direction: column;
  width: 18%;
  gap: 1em;
  background-color: antiquewhite;
  padding: 1em;
  height: 96vh;
  position: fixed;
  justify-content: center;
  transition: transform 0.3s ease;
  top: 50%;
  transform: translate(-50%, -50%);
  left: 10%;
  border-radius: 8px;
  border: 2px solid black;
}

.menu-hidden {
  transform: translate(-153%, -152%) !important;
}

h1, h3 {
  text-align: center;
}

span {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-around;
}
</style>
