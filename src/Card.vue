<template>
  <div
    class="card"
    v-on:click="onClick(label)"
    v-bind:class="{ active: !showFace }"
  >
    <span class="text">{{ label }}</span>
  </div>
</template>

<script>
export default {
  name: "card",

  props: ["label", "showFace", "onClick"],

  methods: {},

  mounted() {}
};
</script>

<style lang="scss">
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
  background-image: url(./assets/logo.png);
  background-size: contain;
  transform: rotateY(180deg);

  .text {
    display: none;
  }
}
</style>
