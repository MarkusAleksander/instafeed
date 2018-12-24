<template>
  <div class="instafeed-image">
    <img :src="image">
  </div>
</template>

<script>
export default {
  name: "InstaImage",
  props: ["image"]
};
</script>
<style lang="scss" scoped>
</style>