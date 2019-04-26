<template>
  <div id="app">
    <speak-to-me v-bind:onDetect="handleClick" />
    <div class="wrapper">
      <card
        v-for="card in cards"
        v-bind:key="card.id"
        v-bind:label="card.id"
        v-bind:showFace="card.showFace"
        v-bind:onClick="handleClick"
      />
    </div>
  </div>
</template>

<script>
import SpeakToMe from "./SpeakToMe";
import Card from "./Card";

export default {
  name: "app",

  components: { Card, SpeakToMe },

  data() {
    return {
      cards: [],

      matchingOptions: [
        { name: "Bier", pairs: 2 },
        { name: "Cola", pairs: 2 },
        { name: "Fanta", pairs: 2 },
        { name: "Wasser", pairs: 2 },
        { name: "Sprite", pairs: 2 },
        { name: "Gin", pairs: 2 },
        { name: "Tonic", pairs: 2 },
        { name: "Wein", pairs: 2 },
        { name: "Mate", pairs: 2 },
        { name: "Wodka", pairs: 2 },
        { name: "Eistee", pairs: 2 },
        { name: "Saft", pairs: 2 }
      ]
    };
  },

  methods: {
    populateBoard() {
      for (let i = 0; i <= 24; i++) {
        this.cards.push({ id: i, showFace: false });
      }
    },

    handleClick(id) {
      this.cards[id].showFace = true;
      setTimeout(() => {
        this.cards[id].showFace = false;
      }, 2000);
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
