<template>
  <div class="screen">
    <h1>Congratulations</h1>
    <p>
      Total playing time: <mark>{{ Math.round(timer / 1000) }} seconds</mark>
    </p>
    <button @click="onStartAgain">Start Again</button>
  </div>
</template>

<script>
export default {
  name: "ResultScreen",
  props: {
    timer: {
      type: Number,
      require: true,
    },
  },
  methods: {
    onStartAgain() {
      this.$emit("onStartAgain");
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
  margin: 35px 0 40px 0;
}

.screen button {
  padding: 20px 25px;
  border: 1px solid var(--light);
  font-size: 1.2rem;
  border-radius: 5px;
  background: transparent;
  color: var(--light);
  cursor: pointer;
}

.screen button:hover {
  background-color: var(--light);
  color: var(--dark);
}
</style>
