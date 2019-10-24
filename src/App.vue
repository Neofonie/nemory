<template>
  <div class="flex flex-col justify-center items-center m-4">
    <h1 class="font-sans text-2xl text-black m-8">Nemory</h1>
    <h2 class="font-sans text-1xl text-black m-4">Score: {{ score }}</h2>
    <h2 class="font-sans text-1xl text-black m-4">
      Time tracking: {{ getTime() }}
    </h2>
    <div class="board m-4">
      <card
        v-for="(card, index) in cards"
        v-bind:key="index"
        v-bind:model="card"
        v-bind:onClick="handleClick"
      />
    </div>
    <button
      v-on:click="resetBoard"
      class="bg-red-600 hover:bg-red-700 text-white uppercase p-2 m-4 rounded"
    >
      reset
    </button>
  </div>
</template>

<script>
import Card from "./Card";
import config from "../nemory.config";
import util from "./util";

export default {
  name: "app",

  components: { Card },

  data() {
    return {
      cards: [],
      first: null,
      second: null,
      score: 0,
      time: 0
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

      this.createCounter();
    },

    createCounter() {
      this.time = 0;
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
        return (
          minutes.toFixed(0) + " minute(s) " + seconds.toFixed(1) + " seconds"
        );
      }
      return seconds.toFixed(1) + " seconds";
    },

    resetBoard() {
      this.destroyCounter();
      this.populateBoard();
    },

    won() {
      this.destroyCounter();
    },

    handleClick(id) {
      if (!this.first) {
        this.first = this.cards[id];
        this.first.showFace = true;
      } else if (!this.second) {
        this.second = this.cards[id];
        this.second.showFace = true;

        if (this.first.id === this.second.id) {
          console.log("matched!");
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
  width: 100%;
  max-width: 766px;
}
</style>
