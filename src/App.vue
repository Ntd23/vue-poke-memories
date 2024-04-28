<template>
  <div>
    <main-screen
      v-if="statusMatch === 'default'"
      @onStart="onHandleBeforeStart($event)"
    ></main-screen>
    <interactive-screen
      v-if="statusMatch === 'match'"
      :cardsContext="settings.cardsContext"
      @onFinish="onGetResult"
    ></interactive-screen>
    <result-screen
      v-if="statusMatch === 'result'"
      :timer="timer"
      @onStartAgain="statusMatch = 'default'"
    ></result-screen>
    <hr />
    <copy-right></copy-right>
  </div>
</template>

<script>
import MainScreen from "./components/MainScreen.vue";
import InteractiveScreen from "./components/InteractiveScreen.vue";
import ResultScreenVue from "./components/ResultScreen.vue";
import { shuffled } from "./utils/array.js";
import CopyrightScreenVue from "./components/CopyrightScreen.vue";

export default {
  name: "App",
  data() {
    return {
      settings: {
        totalOfBlock: 0,
        cardsContext: [],
        startAt: null,
        timer: 0,
      },
      statusMatch: "default",
    };
  },
  components: {
    MainScreen,
    InteractiveScreen,
    ResultScreen: ResultScreenVue,
    CopyRight: CopyrightScreenVue,
  },
  methods: {
    onHandleBeforeStart(config) {
      this.settings.totalOfBlock = config.totalOfBlock;
      const firstCard = Array.from(
        { length: this.settings.totalOfBlock / 2 },
        (_, i) => i + 1
      );
      const secondCard = [...firstCard];
      const cards = [...firstCard, ...secondCard];
      this.settings.cardsContext = shuffled(
        shuffled(shuffled(shuffled(cards)))
      );
      this.settings.startAt = new Date().getTime();
      this.statusMatch = "match";
    },
    onGetResult() {
      this.timer = new Date().getTime() - this.settings.startAt;
      this.statusMatch = "result";
    },
  },
};
</script>
