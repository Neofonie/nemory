<template>
  <div id="app">
    <div class="wrapper">
      <div
        class="card"
        v-for="card in cards"
        v-bind:key="card.id"
        v-bind:class="{ active: !card.showFace }"
        v-on:click="handleClick(card)"
      >
        <span class="text">{{ card.id }}</span>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "app",

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

    handleClick(card) {
      card.showFace = true;

      setTimeout(function() {
        card.showFace = false;
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
  justify-content: center;
  align-items: center;
}

.wrapper {
  width: 600px;
  display: grid;
  grid-template-columns: repeat(5, 100px);
  grid-gap: 10px;

  .card {
    background-color: #444;
    color: #fff;
    border-radius: 5px;
    padding: 20px;
    font-size: 150%;
    order: 1;
    cursor: pointer;
    height: 60px;
    transition: all 0.4s ease;

    display: flex;
    justify-content: center;
    align-items: center;
  }

  .active {
    color: #e51826;
    background: #e51826;
    transform: rotateY(180deg);

    .text {
      display: none;
    }
  }
}
</style>
