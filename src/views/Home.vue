<template>
  <div class="home">
    <image-input v-on:submit="handleSubmit"></image-input>
    <ul class="images-list">
      <li class="image-item" v-for="image in images" :key="image.id">
        <insta-image v-bind:thumb="image.thumb" v-bind:image="image.image"></insta-image>
      </li>
    </ul>
  </div>
</template>

<script>
// @ is an alias to /src
import InstaImage from "@/components/InstaImage.vue";
import ImageInput from "@/components/ImageInput.vue";

import axios from "axios";

export default {
  name: "home",
  components: {
    InstaImage,
    ImageInput
  },
  data: function() {
    return {
      search_tag: "",
      images: [],
      max_images: 12
    };
  },
  methods: {
    handleSubmit(d) {
      this.search_tag = d.search_tag;
      this.max_images = d.num_images;
      this.getImages();
    },
    getImages() {
      console.log("getting images...");
      axios
        .get(`https://www.instagram.com/explore/tags/${this.search_tag}/?__a=1`)
        .then(res => {
          this.images = [];
          let d = res.data.graphql.hashtag.edge_hashtag_to_media.edges;
          for (let i = 0; i < this.max_images; i++) {
            this.images.push({
              key: i,
              thumb: d[i].node.thumbnail_src,
              image: d[i].node.display_url
            });
          }
        });
    }
  }
};
</script>

<style lang="scss" scoped>
.images-list {
  display: flex;
  justify-content: space-between;
  align-items: center;
  flex-wrap: wrap;
  list-style-type: none;
  .image-item {
    flex-grow: 1;
    flex-basis: 25%;
    padding: 10px;
  }
}
</style>
