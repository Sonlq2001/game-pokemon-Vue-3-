<template>
  <div class="screen">
    <main-screen
      v-if="statusMatch === 'default'"
      @onStart="onHandleBeforeStart"
    />
    <interact-screen
      v-if="statusMatch === 'match'"
      :cardsContext="settings.cardsContext"
      @onFinish="onGetResult"
    />

    <result-screen
      v-if="statusMatch === 'result'"
      :timer="timer"
      @onStartAgain="onHandleStartAgain"
    />
  </div>
</template>

<script>
import MainScreen from "./components/MainScreen.vue";
import InteractScreen from "./components/InteractScreen.vue";
import ResultScreen from "./components/ResultScreen.vue";

import { shuffled } from "./utils/array";

export default {
  name: "App",
  components: { MainScreen, InteractScreen, ResultScreen },
  data() {
    return {
      statusMatch: "default",
      settings: {
        totalOfBlocks: 0,
        cardsContext: [],
        startedAt: null,
      },
      timer: null,
    };
  },
  methods: {
    onHandleBeforeStart(config) {
      this.settings.totalOfBlocks = config.totalOfBlocks;

      // render array
      const firstCards = Array.from(
        { length: this.settings.totalOfBlocks / 2 },
        (_, i) => i + 1
      );

      // copy array
      const secondsCards = firstCards.slice();

      // merge array
      const cards = firstCards.concat(secondsCards);

      // assign with array random 4 times
      this.settings.cardsContext = shuffled(shuffled(shuffled(cards)));

      // assign startedAt = date now
      this.settings.startedAt = new Date().getTime();

      // data ready
      this.statusMatch = "match";
    },

    onGetResult() {
      // get timer
      this.timer = new Date().getTime() - this.settings.startedAt;

      // switch to result component
      this.statusMatch = "result";
    },

    onHandleStartAgain() {
      this.statusMatch = "default";
    },
  },
};
</script>

<style scoped>
.screen {
  background-color: var(--dark);
  height: 100vh;
  color: var(--light);
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
}
</style>
