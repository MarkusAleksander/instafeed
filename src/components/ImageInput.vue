<template>
  <form id="image-search" @submit.enter.prevent="updateForm">
    <div class="col">
      <label>Enter tag:</label>
      <input type="text" v-model="search_tag">
      <ul>
        <li
          v-for="tag in search_tags"
          v-on:click="removeTag"
          :key="tag.search_tag"
        >{{tag.search_tag}} : {{tag.num_items}}</li>
      </ul>
    </div>
    <div class="col">
      <label>Number of images to display:</label>
      <input type="number" min="1" max="25" step="1" v-model.number="num_images">
    </div>
    <div class="col">
      <button v-on:click="updateForm">Update</button>
    </div>
  </form>
</template>

<script>
export default {
  name: "ImageInput",
  data: function() {
    return {
      search_tags: [
        {
          num_items: 4,
          search_tag: "christmas"
        }
      ],
      search_tag: "",
      num_images: 0
    };
  },
  mounted: function() {
    this.emitData();
  },
  methods: {
    addNewTag: function() {
      this.search_tags.push({
        num_items: this.num_images,
        search_tag: this.search_tag
      });
      this.search_tag = "";
      this.num_images = 0;
    },
    removeTag: function(e) {},
    updateForm: function() {
      if (this.search_tag.length && this.num_images) {
        this.addNewTag();
        this.emitData();
      }
    },
    emitData: function() {
      this.$emit("update", { search_tags: this.search_tags });
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

