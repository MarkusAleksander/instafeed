<template>
  <transition name="fade" mode="out-in">
    <div id="image-viewer">
      <img :src="image">
    </div>
  </transition>
</template>

<script>
export default {
  name: "ImageViewer",
  props: ["image"]
};
</script>

<style lang="scss" scoped>
#image-viewer {
  img {
    width: 100%;
    max-width: 100%;
    display: block;
    height: auto;
  }
}
</style>
