<template>
  <div class="posts-edit">
    <form v-on:submit.prevent="updatePost()">
      <h1>Edit Post</h1>
      <ul>
        <li class="text-danger" v-for="error in errors" v-bind:key="error">
          {{ error }}
        </li>
      </ul>
      <div class="form-group">
        <label>Title:</label>
        <input
          type="text"
          class="form-control"
          v-model="editPostParams.title"
        />
      </div>

      <div class="form-group">
        <label>Post:</label>
        <input type="text" class="form-control" v-model="editPostParams.body" />
      </div>
      <div class="form-group">
        <label>Image Url:</label>
        <input
          type="text"
          class="form-control"
          v-model="editPostParams.image"
        />
      </div>
      <br />

      <input type="submit" class="btn btn-primary" value="Submit" /><br />
      <button v-on:click="destroyPost()">Delete Post</button>
    </form>
    editPostParams: {{ editPostParams }}
  </div>
</template>

<script>
import axios from "axios";
export default {
  data: function () {
    return {
      editPostParams: {},
      errors: [],
    };
  },
  created: function () {
    axios.get(`/posts/${this.$route.params.id}`).then((response) => {
      console.log("Post object", response.data);
      this.editPostParams = response.data;
    });
  },
  methods: {
    updatePost: function () {
      axios
        .patch(`/posts/${this.editPostParams.id}`, this.editPostParams)
        .then((response) => {
          console.log(response.data);
          this.$router.push(`/posts/${response.data.id}`);
        })
        .catch((error) => {
          this.errors = error.response.data.errors;
        });
    },
    destroyPost: function () {
      if (confirm("Would you like to delete this post?")) {
        axios.delete(`/posts/${this.recipe.id}`).then((response) => {
          console.log(response.data);
          this.$router.push("/posts");
        });
      }
    },
  },
};
</script>
