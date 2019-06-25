<template lang="pug">
  h1.tomato {{ greeting }} from {{ name }}
</template>

<script>
export default {
  data: () => {
    return {
      greeting: "Hello",
      name: "Vue.js"
    };
  }
};
</script>

<style lang="stylus">
.tomato
  color: tomato
</style>
