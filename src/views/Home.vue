<template>
  <div class="home">
    <div class="header">
      <image-input v-on:update="handleUpdate"></image-input>
    </div>
    <div class="col">
      <transition-group name="fade" tag="ul" class="images-list">
        <li class="image-item" v-for="image in loaded_images" v-bind:key="image.key">
          <insta-image v-on:select="updateSelectedImage" v-bind:image="image"></insta-image>
        </li>
      </transition-group>
    </div>
    <div class="col">
      <image-viewer v-if="selectedImage != ''" v-bind:image="selectedImage"></image-viewer>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import InstaImage from "@/components/InstaImage.vue";
import ImageInput from "@/components/ImageInput.vue";
import ImageViewer from "@/components/ImageViewer.vue";

import axios from "axios";

export default {
  name: "home",
  components: {
    InstaImage,
    ImageInput,
    ImageViewer
  },
  data: function() {
    return {
      search_tags: [],
      images: [],
      selectedImage: "",
      latest_search_tags: []
    };
  },
  methods: {
    handleUpdate(d) {
      // Find difference between old and new array
      this.latest_search_tags = d.search_tags.filter(i => {
        return this.search_tags.indexOf(i) < 0;
      });
      this.latest_search_tags.forEach(e => {
        this.search_tags.push(e);
      });
      this.getImages();
    },
    removeItems() {
      while (this.images.length > 0) {
        // Loop and splice out images
        this.images.splice(0, 1);
      }
    },
    loadImages(res, e) {
      let d = res.data.graphql.hashtag.edge_hashtag_to_media.edges;
      // let t_images = d.length >= this.max_images ? this.max_images : d.length;
      let t_images = d.length >= e.num_items ? e.num_items : d.length;
      for (let i = 0; i < t_images; i++) {
        let img = new Image();
        img.onload = () => {
          this.images.push({
            key: `${i}_${e.search_tag}`,
            thumb: img,
            image: d[i].node.display_url,
            search_tag: e.search_tag,
            id: i
          });
          return true;
        };
        // Temporarily disabled to save data
        img.src = d[i].node.thumbnail_src;
      }
    },
    getImages() {
      this.latest_search_tags.forEach(e => {
        axios
          .get(`https://www.instagram.com/explore/tags/${e.search_tag}/?__a=1`)
          .then(res => {
            this.loadImages(res, e);
          })
          .catch(err => {
            this.handleNoImages(err);
          });
      });
      // Clear latest tags after usage
      this.latest_search_tags = [];
    },
    handleNoImages(err) {
      // eslint-disable-next-line
      console.log(`No images found for that...${err}`);
    },
    updateSelectedImage(i) {
      this.selectedImage = i.url;
    }
  },
  computed: {
    loaded_images: function() {
      return this.images.reverse();
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
    flex-basis: 22.5%;
    margin-bottom: 1rem;
  }
}

.home {
  display: flex;
  justify-content: center;
  flex-wrap: wrap;

  .header {
    flex-basis: 100%;
    flex-grow: 1;
  }

  .col {
    flex-basis: 50%;
    padding: 1.5rem;
  }
}

// Transitions

.fade-enter-active,
.fade-leave-active {
  transition: opacity 1s;
}
.fade-enter,
.fade-leave-to {
  opacity: 0;
}
</style>
