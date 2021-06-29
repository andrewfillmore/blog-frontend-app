<template>
  <div class="posts-new">
    <form v-on:submit.prevent="createPost()">
      <h1>New Post</h1>
      <ul>
        <li class="text-danger" v-for="error in errors" v-bind:key="error">
          {{ error }}
        </li>
      </ul>
      <img v-if="status" :src="`https://http.cat/${status}`" alt="" />
      <div class="form-group">
        <label>Title:</label>
        <input type="text" class="form-control" v-model="newPostParams.title" />
      </div>

      <div class="form-group">
        <label>Post:</label>
        <input type="text" class="form-control" v-model="newPostParams.body" />
        <br />
        <small>
          {{ 200 - newPostParams.body.length }} characters remaining
        </small>
        <small
          v-if="
            newPostParams.body.length > 0 && newPostParams.body.length > 200
          "
          type="text-danger"
          >Characters exceding limit</small
        >
      </div>
      <div class="form-group">
        <label>Image Url:</label>
        <input type="text" class="form-control" v-model="newPostParams.image" />
      </div>

      <input type="submit" class="btn btn-primary" value="Submit" />
    </form>
    newPostParams: {{ newPostParams }}
  </div>
</template>

<style scoped>
.text-danger {
  color: red;
}
</style>
<script>
import axios from "axios";
export default {
  data: function () {
    return {
      newPostParams: { body: "" },
      errors: [],
      status: "",
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
          this.status = error.response.status;
        });
    },
  },
};
</script>
