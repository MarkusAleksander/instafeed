<template>
  <form v-on:submit="handleSubmit">
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
      num_images: 12
    };
  },
  mounted: function() {
    this.emitData();
  },
  methods: {
    emitData: function() {
      this.$emit("submit", {
        search_tag: this.search_tag,
        num_images: this.num_images
      });
    },
    handleSubmit: function(e) {
      e.preventDefault();
      this.emitData();
    }
  }
};
</script>

<style lang="scss" scoped>
</style>

