<template>
  <div id="call-menu" @click="toggleMenu">
    <img :src="showMenu ? '/menu_point_top_left.svg' : '/menu_point_bot_right.svg'" />
  </div>
</template>

<script>
import { ref } from 'vue';

export default {
  name: 'MenuToggler',
  props: {
    initialState: {
      type: Boolean,
      default: false
    }
  },
  setup(props, { emit }) {
    const showMenu = ref(props.initialState);

    const toggleMenu = () => {
      showMenu.value = !showMenu.value;
      emit('toggle-menu', showMenu.value);
    };

    return {
      showMenu,
      toggleMenu
    };
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
  margin: 0.1em;
}
</style>
