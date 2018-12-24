<template>
  <div class="home">
    <ul class="images-list">
      <li class="image-item" v-for="image in images" :key="image.id">
        <insta-image v-bind:image="image.image"></insta-image>
      </li>
    </ul>
  </div>
</template>

<script>
// @ is an alias to /src
import InstaImage from "@/components/InstaImage.vue";

import axios from "axios";

export default {
  name: "home",
  components: {
    InstaImage
  },
  data: function() {
    return {
      images: [],
      max_images: 10
    };
  },
  mounted: function() {
    axios
      .get("https://www.instagram.com/explore/tags/christmas/?__a=1")
      .then(res => {
        let d = res.data.graphql.hashtag.edge_hashtag_to_media.edges;
        for (let i = 0; i <= this.max_images; i++) {
          this.images.push({
            key: i,
            image: d[i].node.display_url
          });
        }
      });
  }

  // https://www.instagram.com/explore/tags/hawaii/?__a=1
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
    flex-basis: 33.33333%;
    padding: 10px;
  }
}
</style>
