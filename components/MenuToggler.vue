<template>
  <div id="call-menu" @click="toggleMenu">
    <img :src="showMenu ? '/menu_point_top_left.svg' : '/menu_point_bot_right.svg'" />
  </div>
</template>

<script>
export default {
  name: 'MenuToggler',
  props: {
    initialState: {
      type: Boolean,
      default: false
    }
  },
  data() {
    return {
      showMenu: this.initialState
    };
  },
  methods: {
    toggleMenu() {
      this.showMenu = !this.showMenu;
      this.$emit('toggle-menu', this.showMenu);
    }
  }
};
</script>

<style scoped>
#call-menu {
  cursor: pointer;
}

img {
  width: 2em;
  position: absolute;
  z-index: 1;
}
</style>
