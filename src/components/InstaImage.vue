<template>
  <div class="instafeed-image">
    <a :href="image.image">
      <img :src="image.thumb.src">
    </a>
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