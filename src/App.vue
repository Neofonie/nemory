<template>
  <div class="app font-display">
    <h1 class="app__title mb-8">Nemory</h1>

    <div class="app__settings">
      <h2 class="mb-4">Settings</h2>

      <label class="mb-4 flex">
        <span class="pr-4 w-2/3"> Number of pairs:</span>
        <input v-model="maxPairs" class="neo-input w-1/3" type="number" />
      </label>

      <Button :on-click="resetBoard" :label="'reset'" />
    </div>

    <div class="app__board">
      <Card
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
    </div>

    <div class="app__footer text-center">
      <hr class="m-4" />
      <a href="https://www.neofonie.de/english/#jobs_anchor" target="blank">We're hiring!</a>
    </div>
  </div>
</template>

<script>
import Button from "./components/Button.vue";
import Card from "./components/Card.vue";
import config from "../nemory.config";
import util from "./util";

export default {
  name: "App",

  components: { Button, Card },

  data() {
    return {
      cards: [],
      first: null,
      second: null,
      maxPairs: 10,
      score: 0,
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
    },

    resetBoard() {
      this.populateBoard();
    },

    handleClick(id) {
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
