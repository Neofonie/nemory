<template>
  <div
    class="card h-20 bg-center bg-contain bg-no-repeat bg-neo-red cursor-pointer flex flex-col justify-center items-center text-white text-xl overflow-hidden rounded select-none"
    v-on:click="handleClick"
    v-bind:class="{ active: !model.showFace }"
  >
    <span v-if="model.label" class="text">{{ model.label }}</span>
    <img
      v-if="model.image"
      class="image max-w-full max-h-full"
      :src="model.image"
    />
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
  transition: all 0.4s ease;
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
