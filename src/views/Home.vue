<template>
  <div class="home">
    <p>Images:</p>
    <ul>
      <li v-for="image in images" :key="image.id">
        <insta-image v-bind:image="image.image"></insta-image>
      </li>
    </ul>
  </div>
</template>

<script>
// @ is an alias to /src
import InstaImage from "@/components/InstaImage.vue";

export default {
  name: "home",
  components: {
    InstaImage
  },
  data: function() {
    return {
      images: [
        { key: 1, image: "image-1,jpg" },
        { key: 2, image: "image-2.jpg" }
      ]
    };
  }
};
</script>
