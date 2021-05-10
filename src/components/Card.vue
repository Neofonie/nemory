<template>
  <div
    class="
      card
      h-20
      bg-center bg-contain bg-no-repeat bg-neo-red
      cursor-pointer
      flex flex-col
      justify-center
      items-center
      text-white text-xl
      overflow-hidden
      rounded-lg
      select-none
    "
    :class="{ active: !model.showFace }"
    @click="handleClick"
  >
    <span v-if="model.label" class="text">{{ model.label }}</span>
    <img v-if="model.image" class="image max-w-full max-h-full" :src="model.image" />
  </div>
</template>

<script>
export default {
  name: "Card",

  props: ["model", "index", "onClick"],

  mounted() {},

  methods: {
    handleClick() {
      if (!this.model.showFace) {
        this.onClick(this.index);
      }
    },
  },
};
</script>

<style scoped lang="scss">
.card {
  box-shadow: 5px 5px 15px #a09f9f, -5px -5px 15px #fff;
  transition: all 0.4s ease;
}

.active {
  background-image: url(../assets/logo.png);
  transform: rotateY(180deg);

  .image,
  .text {
    display: none;
  }
}
</style>
