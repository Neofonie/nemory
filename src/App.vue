<template>
  <div id="app">
    <div class="wrapper">
      <card
        v-for="(card, index) in cards"
        v-bind:key="index"
        v-bind:model="card"
        v-bind:onClick="handleClick"
      />
    </div>
  </div>
</template>

<script>
import Card from "./Card";
import config from "./config";

export default {
  name: "app",

  components: { Card },

  data() {
    return {
      cards: []
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

      this.cards = this.shuffle(arr);
    },

    handleClick(id) {
      this.cards[id].showFace = true;
      setTimeout(() => {
        this.cards[id].showFace = false;
      }, 2000);
    },

    /**
     * Randomly shuffle an array
     * https://stackoverflow.com/a/2450976/1293256
     * @param  {Array} array The array to shuffle
     * @return {String}      The first item in the shuffled array
     */
    shuffle(array) {
      let currentIndex = array.length;
      let temporaryValue, randomIndex;

      // While there remain elements to shuffle...
      while (0 !== currentIndex) {
        // Pick a remaining element...
        randomIndex = Math.floor(Math.random() * currentIndex);
        currentIndex -= 1;

        // And swap it with the current element.
        temporaryValue = array[currentIndex];
        array[currentIndex] = array[randomIndex];
        array[randomIndex] = temporaryValue;
      }

      return array;
    }
  },

  mounted() {
    this.populateBoard();
  }
};
</script>

<style lang="scss">
#app {
  display: flex;
  flex-flow: column;
  justify-content: center;
  align-items: center;
}

.wrapper {
  width: 600px;
  display: grid;
  grid-template-columns: repeat(5, 100px);
  grid-gap: 10px;
}
</style>
