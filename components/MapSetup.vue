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
          v-model.number="localMapHeight"
          type="number"
          placeholder="Map Height (Km)"
        />
        <input
          id="real-x"
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
      :class="{ 'disabled-button': isisSetup }"
      :disabled="isisSetup"
    >
      Create
    </button>
  </main>
</template>

<script>
export default {
  name: 'MapSetup',
  props: {
    worldName: {
      type: String,
      required: true,
    },
    mapHeight: {
      type: Number,
      required: true,
    },
    mapWidth: {
      type: Number,
      required: true,
    },
    isSetup: {
      type: Boolean,
      default: false,
    }
  },
  data() {
    return {
      localWorldName: this.worldName,
      localMapHeight: this.mapHeight,
      localMapWidth: this.mapWidth,
    };
  },
  computed: {
    isisSetup() {
      return this.isSetup || !this.localWorldName || this.localMapHeight <= 0 || this.localMapWidth <= 0;
    },
  },
  methods: {
    emitWorldSetup() {
      this.$emit('getWorldSetup', this.localWorldName, this.localMapHeight, this.localMapWidth);
    },
  },
  watch: {
    worldName(newVal) {
      this.localWorldName = newVal;
    },
    mapHeight(newVal) {
      this.localMapHeight = newVal;
    },
    mapWidth(newVal) {
      this.localMapWidth = newVal;
    },
  },
};
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
</style>
