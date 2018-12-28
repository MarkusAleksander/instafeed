<template>
  <div class="instafeed-image" v-on:click="updateSelectedImage">
    <img :src="image.thumb.src">
  </div>
</template>

<script>
export default {
  name: "InstaImage",
  props: ["image"],
  methods: {
    updateSelectedImage: function() {
      this.$emit("select", { url: this.image.image });
    }
  }
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