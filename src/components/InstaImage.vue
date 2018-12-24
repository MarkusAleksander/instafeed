<template>
  <div class="instafeed-image">
    <a :href="image">
      <img :src="thumb">
    </a>
  </div>
</template>

<script>
export default {
  name: "InstaImage",
  props: ["thumb", "image"]
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