<template>
  <button
    class="bg-neo-red hover:bg-red-700 text-white uppercase p-2 m-4 rounded"
    v-on:click="onClick"
  >
    {{ label }}
  </button>
</template>

<script>
export default {
  name: "Button",

  props: ["label", "onClick"],

  mounted() {}
};
</script>
