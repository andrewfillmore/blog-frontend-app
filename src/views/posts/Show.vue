<template>
  <div class="posts-show">
    <h2>{{ post.title }}</h2>
    <img :src="post.image" alt="" />
    <p>{{ post.body }}</p>
    <p>{{ post.user_id }}</p>
    <router-link :to="`/posts/${post.id}/edit`">Edit</router-link>
  </div>
</template>

<script>
import axios from "axios";
export default {
  data: function () {
    return {
      post: {},
    };
  },
  created: function () {
    axios.get(`/posts/${this.$route.params.id}`).then((response) => {
      console.log("Post object", response.data);
      this.post = response.data;
    });
  },
  methods: {},
};
</script>
