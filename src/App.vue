<template>
  <div class="flex flex-col justify-center items-center">
    <h1 class="font-sans text-2xl text-black m-8">Nemory</h1>
    <div class="board">
      <card
        v-for="(card, index) in cards"
        v-bind:key="index"
        v-bind:model="card"
        v-bind:onClick="handleClick"
      />
    </div>
    <button
      v-on:click="resetBoard"
      class="bg-red-600 hover:bg-red-700 text-white py-2 px-4 mx-2 my-3 rounded"
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
      second: null
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
          console.log("matched!");
          this.first = null;
          this.second = null;
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
  width: 100%;
  max-width: 766px;
}
</style>
