<template>
  <div class="home">
    <div class="header">
      <image-input v-on:update="handleUpdate" v-on:remove="handleRemove"></image-input>
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
    handleRemove(d) {
      // Find number of items to remvoe
      let num_items = this.search_tags.find(e => {
        return e.search_tag == d.search_tag;
      }).num_items;
      if (num_items) {
        for (let i = 0; i < num_items; i++) {
          this.images.splice(
            this.images.indexOf(
              this.images.find(e => {
                e.id == i && e.search_tag == d.search_tag;
              })
            ),
            1
          );
        }
        this.search_tags.splice(
          this.search_tags.indexOf(
            this.search_tags.find(e => {
              return e.search_tag == d.search_tag;
            })
          ),
          1
        );
      }
    },
    removeItems() {
      while (this.images.length > 0) {
        // Loop and splice out images
        this.images.splice(0, 1);
      }
    },
    loadImages(res, e) {
      // Image post loader
      function loadPosts(arr, num_posts, $t) {
        for (let i = 0; i < num_posts; i++) {
          let img = new Image();
          img.onload = () => {
            $t.images.push({
              key: `${i}_${e.search_tag}`,
              thumb: img,
              image: arr[i].node.display_url,
              search_tag: e.search_tag,
              id: i
            });
            return true;
          };
          // Temporarily disabled to save data
          img.src = arr[i].node.thumbnail_src;
        }
      }

      if (e.top_posts) {
        let d = res.data.graphql.hashtag.edge_hashtag_to_top_posts.edges;
        let num_posts = d.length >= e.num_items ? e.num_items : d.length;
        loadPosts(d, num_posts, this);
        if (num_posts < e.num_items) {
          d = res.data.graphql.hashtag.edge_hashtag_to_media.edges;
          num_posts =
            d.length >= e.num_items - num_posts
              ? e.num_items - num_posts
              : d.length;
          loadPosts(d, num_posts, this);
          // num_posts = d.length >= e.num_items ? e.num_items : d.length;
        }
      } else {
        let d = res.data.graphql.hashtag.edge_hashtag_to_media.edges;
        let num_posts = d.length >= e.num_items ? e.num_items : d.length;
        loadPosts(d, num_posts, this);
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
      return this.images.slice().reverse();
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
