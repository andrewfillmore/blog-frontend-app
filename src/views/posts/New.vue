<template>
  <div class="posts-new">
    <form v-on:submit.prevent="createPost()">
      <h1>New Post</h1>
      <ul>
        <li class="text-danger" v-for="error in errors" v-bind:key="error">
          {{ error }}
        </li>
      </ul>
      <div class="form-group">
        <label>Title:</label>
        <input type="text" class="form-control" v-model="newPostParams.title" />
      </div>

      <div class="form-group">
        <label>Post:</label>
        <input type="text" class="form-control" v-model="newPostParams.body" />
      </div>
      <div class="form-group">
        <label>Image</label>
        <input
          type="text"
          class="form-control"
          v-model="newPostParams.image_url"
        />
      </div>

      <input type="submit" class="btn btn-primary" value="Submit" />
    </form>
    newPostParams: {{ newPostParams }}
  </div>
</template>

<script>
import axios from "axios";
export default {
  data: function () {
    return {
      newPostParams: {},
      errors: [],
    };
  },

  methods: {
    createPost: function () {
      axios
        .post("/posts", this.newPostParams)
        .then((response) => {
          console.log(response.data);
          this.$router.push("/posts");
        })
        .catch((error) => {
          this.errors = error.response.data.errors;
        });
    },
  },
};
</script>
