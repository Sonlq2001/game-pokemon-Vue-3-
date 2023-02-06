<template>
  <div class="screen">
    <h1>POKEMON</h1>
    <p>Select mode start game</p>

    <div class="actions">
      <button @click="onStart(16)">
        <span>4x4</span>
        <span>Easy</span>
      </button>
      <button @click="onStart(36)">
        <span>6x6</span>
        <span>Normal</span>
      </button>
      <button @click="onStart(64)">
        <span>8x8</span>
        <span>Hard</span>
      </button>
      <button @click="onStart(100)">
        <span>10x10</span>
        <span>Super Hard</span>
      </button>
    </div>
  </div>
</template>

<script>
export default {
  name: "MainScreen",
  components: {},
  methods: {
    onStart(totalOfBlocks) {
      this.$emit("onStart", { totalOfBlocks });
    },
  },
};
</script>

<style scoped>
.screen h1 {
  font-size: 100px;
}

.screen p {
  font-size: 30px;
}

.actions {
  margin-top: 2.5rem;
  display: flex;
  align-items: center;
  gap: 15px;
}

button {
  padding: 1.4rem;
  font-size: 1.2rem;
  width: 150px;
  height: 150px;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  border-radius: 10px;
  border: 1px solid var(--light);
  cursor: pointer;
  background: transparent;
  color: var(--light);
}

button:hover {
  color: var(--dark);
  background: var(--light);
}
</style>
