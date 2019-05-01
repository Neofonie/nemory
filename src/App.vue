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
import util from "./util";

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

      this.cards = util.shuffle(arr);
    },

    handleClick(id) {
      console.log(id);
      this.cards[id].showFace = true;

      /*
      setTimeout(() => {
        this.cards[id].showFace = false;
      }, 2000);

      */
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
