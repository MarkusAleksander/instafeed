<template>
  <transition name="fade" mode="out-in">
    <div id="video-viewer">
      <video autoplay controls>
        <source :src="video">
      </video>
    </div>
  </transition>
</template>

<script>
export default {
  name: "VideoViewer",
  props: ["video"]
};
</script>

<style lang="scss" scoped>
#video-viewer {
  video {
    width: 100%;
    max-width: 100%;
    display: block;
    height: auto;
  }
}
</style>
