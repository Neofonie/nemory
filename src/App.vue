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
          label: config.matchingOptions[i].name
        });
      }

      this.cards = util.shuffle(arr);
    },

    handleClick(id) {
      if (!this.first) {
        this.first = this.cards[id];
        this.first.showFace = true;
      } else if (!this.second) {
        this.second = this.cards[id];
        this.second.showFace = true;

        if (this.first.id == this.second.id) {
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
