<template>
  <div class="posts-index">
    <div v-for="post in posts" v-bind:key="post.id">
      <h2>{{ post.title }}</h2>
      <img :src="post.image_url" alt="" />
      <p>Body: {{ post.body }}</p>
      <p>User{{ post.user_id }}</p>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  data: function () {
    return {
      posts: [],
    };
  },
  created: function () {
    axios.get("/posts").then((response) => {
      console.log("Posts Array", response.data);
      this.posts = response.data;
    });
  },
};
</script>
