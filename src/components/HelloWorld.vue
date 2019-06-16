<template>
  <div class="hello">
    <ul class="list-group list-group-flush">
      <li
        class="list-group-item flex container"
        v-for="post in posts"
        :key="post.data.id"
        @click="openImage(post.data.preview.images[0].source.url)"
      >
        <img :src="post.data.thumbnail" alt="thumb" class="thumbnail">
        <div>{{ post.data.title }}</div>
      </li>
    </ul>
  </div>
</template>

<script>
const axios = require("axios");
const { ipcRenderer } = require("electron");
export default {
  name: "HelloWorld",
  props: {
    msg: String
  },
  data() {
    return {
      posts: []
    };
  },
  methods: {
    openImage(image) {
      ipcRenderer.send("toggle-image", image);
    }
  },
  created() {
    axios
      .get("https://reddit.com/r/pics.json")
      .then(response => {
        this.posts = response.data.data.children;
      })
      .catch(error => {
        console.log(error);
      });
  }
};
</script>
<style scoped>
.thumbnail {
  width: 60px;
  height: 60px;
  border-radius: 30px;
  margin-right: 16px;
}
.list-group-item {
  cursor: pointer;
}
.list-group-item:hover {
  background-color: #eee;
}
</style>
