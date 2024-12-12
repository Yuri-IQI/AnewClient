<template>
  <main>
    <div id="inputs" class="container">
      <input
        id="world-name"
        v-model="localWorldName"
        placeholder="World Name"
      />
      <div id="sizes">
        <input
          id="real-y"
          class="dimension"
          v-model.number="localMapHeight"
          type="number"
          placeholder="Map Height (Km)"
        />
        <input
          id="real-x"
          class="dimension"
          v-model.number="localMapWidth"
          type="number"
          placeholder="Map Width (Km)"
        />
      </div>
    </div>
    <button
      id="bt-create"
      type="button"
      @click="emitWorldSetup"
      :class="{ 'disabled-button': hasBeenSetup }"
      :disabled="hasBeenSetup"
    >
      Create
    </button>
  </main>
</template>

<script>
import { defineComponent, ref, computed, watch } from 'vue';

export default defineComponent({
  name: 'MapSetup',
  props: {
    mapHeight: {
      type: Number,
      required: true,
      default: 0,
    },
    mapWidth: {
      type: Number,
      required: true,
      default: 0,
    },
    worldName: {
      type: String,
      required: true,
      default: '',
    },
    isSetup: {
      type: Boolean,
      default: false,
    }
  },
  setup(props, { emit }) {
    const localWorldName = ref(props.worldName);
    const localMapHeight = ref(props.mapHeight);
    const localMapWidth = ref(props.mapWidth);

    const hasBeenSetup = computed(() => {
      return props.isSetup || !localWorldName.value || localMapHeight.value <= 0 || localMapWidth.value <= 0;
    });

    const emitWorldSetup = () => {
      emit('getWorldSetup', localWorldName.value, localMapHeight.value, localMapWidth.value);
    };

    watch(() => props.worldName, (newVal) => {
      localWorldName.value = newVal;
    });

    watch(() => props.mapHeight, (newVal) => {
      localMapHeight.value = newVal;
    });

    watch(() => props.mapWidth, (newVal) => {
      localMapWidth.value = newVal;
    });

    return {
      localWorldName,
      localMapHeight,
      localMapWidth,
      hasBeenSetup,
      emitWorldSetup
    };
  }
});
</script>

<style scoped>
main {
  display: flex;
  flex-direction: column;
  position: absolute;
  top: 50%;
  transform: translate(-50%, -50%);
  left: 50%;
  width: 100%;
  height: 100%;
  align-items: center;
  font-size: xxx-large;
  background-color: #000000e0;
  justify-content: space-evenly;
}

.container {
  display: flex;
  flex-direction: column;
  gap: 1.5em;
  width: 80%;
}

input {
  text-align: center;
  color: #fff9;
  background-color: #333;
  border-radius: 5px;
  border: none;
  outline: none;
  padding: 0.2em;
}

input::-webkit-outer-spin-button,
input::-webkit-inner-spin-button {
  -webkit-appearance: none;
  margin: 0;
}

input[type=number] {
  -moz-appearance: textfield;
}

#sizes {
  display: flex;
  justify-content: space-between;
  gap: 1em;
}

#bt-create {
  background-color: #4f3f2e96;
  border-radius: 8px;
  color: #96969670;
  border: 4px solid #2f2724;
  padding: 0.5em;
}

#bt-create:hover {
  background-color: #ebd2bc;
  color: #323232a3;
}

.disabled-button {
  background-color: #444;
  color: #666;
  cursor: not-allowed;
}

.dimension {
  width: 50%;
}
</style>