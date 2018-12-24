<template>
  <form @submit.enter.prevent="emitData">
    <div class="col">
      <label>Enter tag:</label>
      <input type="text" v-model="search_tag">
    </div>
    <div class="col">
      <label>Number of images to display:</label>
      <input text="number" min="1" max="25" step="1" v-model="num_images">
    </div>
    <button v-on:click="emitData">Update</button>
  </form>
</template>

<script>
export default {
  name: "ImageInput",
  data: function() {
    return {
      search_tag: "christmas",
      num_images: 4
    };
  },
  mounted: function() {
    this.emitData();
  },
  methods: {
    emitData: function(e) {
      console.log("emitting data...");
      this.$emit("update", {
        search_tag: this.search_tag,
        num_images: this.num_images
      });
    }
  }
};
</script>

<style lang="scss" scoped>
</style>

