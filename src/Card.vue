<template>
  <div
    class="card"
    v-on:click="handleClick"
    v-bind:class="{ active: !model.showFace }"
  >
    <span v-if="model.label" class="text">{{ model.label }}</span>
    <img v-if="model.image" class="text" :src="model.image" />
  </div>
</template>

<script>
export default {
  name: "card",

  props: ["model", "onClick"],

  methods: {
    handleClick() {
      if (!this.model.showFace) {
        this.onClick(this.$vnode.key);
      }
    }
  },

  mounted() {}
};
</script>

<style lang="scss">
.card {
  background-color: #444;
  border-radius: 5px;
  color: #fff;
  cursor: pointer;
  padding: 20px;
  font-size: 150%;
  height: 60px;
  transition: all 0.4s ease;
  display: flex;
  justify-content: center;
  align-items: center;
  user-select: none;
}

.active {
  background-image: url(./assets/logo.png);
  background-size: contain;
  transform: rotateY(180deg);

  .text {
    display: none;
  }
}
</style>
