<template>
  <div class="posts-index">
    <input
      v-model="searchTerm"
      type="text"
      list="titles"
      placeholder="search"
    />
    <button v-on:click="setSortAttribute('title')">Sort by Post Titles</button>
    <button v-on:click="setSortAttribute('user_id')">Sort by User</button>
    <datalist id="titles" placeholder="Search">
      <option v-for="post in posts" v-bind:key="post.title">
        {{ post.title }}
      </option>
    </datalist>
    <!-- <div>
      <h3>Search All Posts</h3>
      <input type="text" v-model="searchTerm" placeholder="Search Posts" />
    </div> -->
    <div
      is="transition-group"
      appear
      enter-active-class="animate__fadeIn"
      leave-active-class="animate__fadeOut"
    >
      >
      <div
        v-for="post in orderBy(
          filterBy(posts, searchTerm),
          sortAttribute,
          sortOrder
        )"
        v-bind:key="post.id"
      >
        <h2>{{ post.title }}</h2>

        <img :src="post.image" alt="" />
        <p>Body: {{ post.body }}</p>
        <p>User: {{ post.user_id }}</p>
        <p>Post Created: {{ relativeDate(post.created_at) }}</p>
        <router-link :to="`/posts/${post.id}`">Go to Post</router-link>
      </div>
    </div>
  </div>
</template>

<style scoped></style>

<script>
import axios from "axios";
import moment from "moment";
import Vue2Filters from "vue2-filters";

export default {
  mixins: [Vue2Filters.mixin],
  data: function () {
    return {
      posts: [],
      searchTerm: "",
      sortAttribute: "",
      sortOrder: "",
    };
  },
  created: function () {
    axios.get("/posts").then((response) => {
      console.log("Posts Array", response.data);
      this.posts = response.data;
    });
  },
  methods: {
    relativeDate: function (date) {
      return moment(date).fromNow();
    },
    setSortAttribute: function (attribute) {
      this.sortAttribute = attribute;
    },
  },
};
</script>
