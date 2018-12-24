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
.instafeed-image {
  width: 100%;
  height: 100%;

  img {
    display: block;
    width: 100%;
    max-width: 100%;
    height: auto;
  }
}
</style>