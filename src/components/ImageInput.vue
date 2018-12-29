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
          :data-tag="tag.search_tag"
        >{{tag.search_tag}} : {{tag.num_items}} : {{tag.top_posts}}</li>
      </ul>
    </div>
    <div class="col">
      <label>Number of images to display:</label>
      <input type="number" min="1" :max="max_images" step="1" v-model.number="num_images">
      <label>Search Top Posts First</label>
      <input type="checkbox" v-model="top_posts">
    </div>
    <div class="col">
      <button v-on:click="updateForm">Update</button>
    </div>
  </form>
</template>

<script>
const default_num_images = 8;

export default {
  name: "ImageInput",
  data: function() {
    return {
      search_tags: [
        {
          num_items: default_num_images,
          search_tag: "christmas",
          top_posts: false
        }
      ],
      search_tag: "",
      num_images: default_num_images,
      top_posts: false,
      max_images: 20
    };
  },
  mounted: function() {
    this.emitData();
  },
  methods: {
    addNewTag: function() {
      this.search_tags.push({
        num_items: this.num_images,
        search_tag: this.search_tag,
        top_posts: this.top_posts
      });
      this.search_tag = "";
      this.num_images = 0;
      this.top_posts = false;
    },
    removeTag: function(e) {
      let t = e.target.getAttribute("data-tag");
      if (t) {
        this.search_tags.splice(
          this.search_tags.indexOf(
            this.search_tags.find(e => {
              e.search_tag == t;
            })
          ),
          1
        );
        this.$emit("remove", { search_tag: t });
      }
    },
    updateForm: function() {
      if (this.search_tag.length && this.num_images <= this.max_images) {
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

