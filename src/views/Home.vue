<template>
  <div class="home">
    <image-input v-on:update="handleUpdate"></image-input>
    <transition-group name="fade" tag="ul" class="images-list">
      <li class="image-item" v-for="image in images" v-bind:key="image.key">
        <insta-image v-bind:image="image"></insta-image>
      </li>
    </transition-group>
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
      max_images: 0
    };
  },
  methods: {
    handleUpdate(d) {
      this.search_tag = d.search_tag;
      this.max_images = d.num_images;
      this.removeItems();
      this.getImages();
    },
    removeItems() {
      while (this.images.length > 0) {
        // Loop and splice out images
        this.images.splice(0, 1);
      }
    },
    loadImages(res) {
      let d = res.data.graphql.hashtag.edge_hashtag_to_media.edges;
      let t_images = d.length >= this.max_images ? this.max_images : d.length;
      for (let i = 0; i < t_images; i++) {
        let img = new Image();
        img.onload = () => {
          this.images.push({
            key: i,
            thumb: img,
            image: d[i].node.display_url
          });
          return true;
        };
        img.src = d[i].node.thumbnail_src;
      }
    },
    getImages() {
      axios
        .get(`https://www.instagram.com/explore/tags/${this.search_tag}/?__a=1`)
        .then(res => {
          this.loadImages(res);
        })
        .catch(err => {
          this.handleNoImages(err);
        });
    },
    handleNoImages(err) {
      // eslint-disable-next-line
      console.log(`No images found for that...${err}`);
    }
  },
  computed: {
    loaded_images: function() {
      return this.images.filter(i => {
        return i.loaded;
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
} // Transitions

.fade-enter-active,
.fade-leave-active {
  transition: opacity 1s;
}
.fade-enter,
.fade-leave-to {
  opacity: 0;
}
</style>
