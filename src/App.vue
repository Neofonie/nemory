<template>
  <div class="flex flex-col justify-center items-center m-4">
    <h1 class="m-8">Nemory</h1>
    <p class="m-4">Score: {{ score }}</p>
    <div class="board m-4 w-full max-w-3/4">
      <Card
        v-for="(card, index) in cards"
        v-bind:key="index"
        v-bind:model="card"
        v-bind:onClick="handleClick"
      />
    </div>
    <Button class="m-4" v-bind:onClick="resetBoard" v-bind:label="'reset'" />
    <hr class="w-full m-4" />
    <a href="https://www.neofonie.de/english/#jobs_anchor" target="blank"
      >We're hiring!</a
    >
  </div>
</template>

<script>
import Button from "./components/Button";
import Card from "./components/Card";
import config from "../nemory.config";
import util from "./util";

export default {
  name: "app",

  components: { Button, Card },

  data() {
    return {
      cards: [],
      first: null,
      second: null,
      score: 0
    };
  },

  methods: {
    populateBoard() {
      let arr = [];

      for (let i = 0; i < config.matchingOptions.length; i++) {
        arr.push({
          id: i,
          showFace: false,
          label: config.matchingOptions[i].name
        });
        arr.push({
          id: i,
          showFace: false,
          image: config.matchingOptions[i].image
        });
      }

      this.cards = util.shuffle(arr);
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
    }
  },

  mounted() {
    this.populateBoard();
  }
};
</script>

<style scoped lang="scss">
.board {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(5rem, 1fr));
  grid-template-rows: repeat(auto-fill, minmax(5rem, 1fr));
  grid-gap: 10px;
}
</style>
