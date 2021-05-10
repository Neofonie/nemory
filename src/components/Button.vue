<template>
  <button class="button neo-btn neo-btn--red uppercase" @click="onClick">
    {{ label }}
  </button>
</template>

<script>
export default {
  name: "Button",

  props: ["label", "onClick"],

  mounted() {},
};
</script>

<style scoped lang="scss">
.button {
  box-shadow: 5px 5px 15px #a09f9f, -5px -5px 15px #fff;
}
</style>
