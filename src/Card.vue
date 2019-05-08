<template>
  <div
    class="card"
    v-on:click="handleClick"
    v-bind:class="{ active: !model.showFace }"
  >
    <span v-if="model.label" class="text">{{ model.label }}</span>
    <img v-if="model.image" class="image" :src="model.image" />
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
  background-color: #e60024;
  background-size: contain;
  background-position: 50%;
  background-repeat: no-repeat;
  border-radius: 5px;
  color: #fff;
  cursor: pointer;
  //padding: 20px;
  font-size: 150%;
  height: 100px;
  transition: all 0.4s ease;
  display: flex;
  justify-content: center;
  align-items: center;
  user-select: none;
  overflow: hidden;

  .image {
    max-width: 100%;
    max-height: 100%;
  }
}

.active {
  background-image: url(./assets/logo.png);
  transform: rotateY(180deg);

  .image,
  .text {
    display: none;
  }
}
</style>
