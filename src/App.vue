<template>
  <div class="app font-display">
    <h1 class="app__title mb-8">Nemory</h1>

    <div class="app__settings">
      <h2 class="mb-4">Settings</h2>

      <label class="mb-4 flex">
        <span class="pr-4 w-2/3"> Number of pairs:</span>
        <input v-model="maxPairs" class="neo-input w-1/3" type="number" min="1" />
      </label>

      <NeoButton :on-click="resetBoard" :label="'reset'" />
    </div>

    <div class="app__board">
      <NemoryCard
        v-for="(card, index) in cards"
        v-bind="{
          index: index,
          model: card,
          onClick: handleClick,
        }"
        :key="index"
      />
    </div>

    <div class="app__control">
      <h2>Score</h2>
      <p class="m-4">{{ score }}</p>

      <h2>Time</h2>
      <p class="m-4">{{ getTime() }}</p>
    </div>

    <div class="app__footer text-center">
      <hr class="m-4" />
      <a href="https://www.neofonie.de/english/#jobs_anchor" target="blank">We're hiring!</a>
    </div>
  </div>
</template>

<script>
import NeoButton from "./components/NeoButton.vue";
import NemoryCard from "./components/NemoryCard.vue";
import config from "../nemory.config";
import util from "./util";

export default {
  name: "App",

  components: { NeoButton, NemoryCard },

  data() {
    return {
      cards: [],
      first: null,
      second: null,
      maxPairs: 10,
      score: 0,
      time: 0,
    };
  },

  mounted() {
    this.populateBoard();
  },

  methods: {
    populateBoard() {
      // use the max number of items
      let numPairs = Math.min(config.matchingOptions.length, this.maxPairs);

      // shuffle our config and only use the first x items
      let shuffledConfig = util.shuffle([...config.matchingOptions]).splice(0, numPairs);

      // populate cards and shuffle them again
      this.cards = util.shuffle(
        shuffledConfig.reduce((result, item, index) => {
          result.push({
            id: index,
            showFace: false,
            label: item.name,
          });
          result.push({
            id: index,
            showFace: false,
            image: item.image,
          });
          return result;
        }, [])
      );

      this.first = null;
      this.second = null;
      this.score = 0;
      this.time = 0;
    },

    createCounter() {
      this.counter = setInterval(() => {
        this.time++;
      }, 100);
    },

    destroyCounter() {
      clearInterval(this.counter);
    },

    getTime() {
      let seconds = this.time / 10;
      let minutes = 0;
      if (seconds >= 60) {
        minutes = Math.floor(seconds / 60);
        seconds = seconds % 60;
        return minutes.toFixed(0) + " minute(s) " + seconds.toFixed(1) + " seconds";
      }
      return seconds.toFixed(1) + " seconds";
    },

    resetBoard() {
      this.time = 0;
      this.destroyCounter();
      this.populateBoard();
    },

    won() {
      this.destroyCounter();
    },

    handleClick(id) {
      if (this.time === 0) {
        this.createCounter();
      }
      if (!this.first) {
        this.first = this.cards[id];
        this.first.showFace = true;
      } else if (!this.second) {
        this.second = this.cards[id];
        this.second.showFace = true;

        if (this.first.id === this.second.id) {
          this.first = null;
          this.second = null;
          this.score++;
        } else {
          setTimeout(() => {
            this.first.showFace = false;
            this.second.showFace = false;
            this.first = null;
            this.second = null;
          }, 500);
        }
      }
      if (this.score >= this.cards.length / 2) {
        this.won();
      }
    },
  },
};
</script>

<style scoped lang="scss">
.app {
  display: grid;
  grid-template-columns: 10px 200px 1fr 200px 10px;
  grid-template-areas:
    ". title title title ."
    ". control board settings ."
    ". footer footer footer .";
  grid-gap: 15px;

  &__title {
    grid-area: title;

    @apply text-center;
  }

  &__settings {
    grid-area: settings;

    @apply text-right;
  }

  &__control {
    grid-area: control;
  }

  &__board {
    grid-area: board;
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(5rem, 1fr));
    grid-template-rows: repeat(auto-fill, minmax(5rem, 1fr));
    grid-gap: 15px;
  }

  &__footer {
    grid-area: footer;
  }
}
</style>
