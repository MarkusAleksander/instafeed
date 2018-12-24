<template>
  <form id="image-search" @submit.enter.prevent="emitData">
    <div class="col">
      <label>Enter tag:</label>
      <input type="text" v-model="search_tag">
    </div>
    <div class="col">
      <label>Number of images to display:</label>
      <input text="number" min="1" max="25" step="1" v-model="num_images">
    </div>
    <div class="col">
      <button v-on:click="emitData">Update</button>
    </div>
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
    emitData: function() {
      this.$emit("update", {
        search_tag: encodeURI(this.search_tag),
        num_images: this.num_images
      });
    }
  }
};
</script>

<style lang="scss" scoped>
#image-search {
  width: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
  flex-wrap: wrap;
  padding: 15px;

  .col {
    flex-grow: 1;
    flex-basis: 50%;
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: column;
  }

  label {
    text-align: center;
    margin-bottom: 1rem;
  }

  input {
    background-color: #fff;
    padding: 0.5rem;
  }

  button {
    background-color: #000;
    color: #fff;
    border: 0;
    border-radius: 1.5rem;
    padding: 0.8rem 1.2rem;
    cursor: pointer;
  }
}
</style>

