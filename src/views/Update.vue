<template>
  <div>
    <h1>Update Post for id: {{ $route.params.id }}</h1>
    <div class="error">{{ error }}</div>
    <div>
      <form action="#" @submit.prevent="updatePost">
        <div>
          <label for="title">Title</label>
          <input v-model="title" type="text" name="title" id="title" />
        </div>
        <div>
          <label for="body">body</label>
          <textarea
            cols="30"
            rows="10"
            v-model="body"
            name="body"
            id="body"
          ></textarea>
        </div>
        <div>
          <button>Update Post</button>
          <div v-if="loading">loading....</div>
        </div>
      </form>
    </div>
  </div>
</template>
<script>
import gql from "graphql-tag";

export default {
  name: "UpdatePost",
  data() {
    return {
      title: "",
      body: "",
      error: null,
      loading: false,
    };
  },
  watch: {
    post(newPost) {
      this.title = newPost.title;
      this.body = newPost.body;
    },
  },
  apollo: {
    // Simple query that will update the 'hello' vue property
    post: {
      query: gql`
        query getPost($id: ID!) {
          post(id: $id) {
            title
            body
          }
        }
      `,
      variables() {
        return {
          id: this.$route.params.id,
        };
      },
    },
  },
  methods: {
    updatePost() {
      this.loading = true;
      this.$apollo
        .mutate({
          mutation: gql`
            mutation updatePost($id: ID!, $title: String!, $body: String!) {
              updatePost(id: $id, title: $title, body: $body) {
                id
                title
                body
                created_at
              }
            }
          `,
          variables: {
            id: this.$route.params.id,
            title: this.title,
            body: this.body,
          },
        })
        .then((data) => {
          this.loading = false;
          this.$router.push({ name: "home" });
        })
        .catch((error) => {
          this.loading = false;
          const key = Object.keys(
            error.graphQLErrors[0].extensions.validation
          )[0];
          this.error = error.graphQLErrors[0].extensions.validation[key][0];
          console.log(key);
        });
    },
  },
};
</script>
<style>
.error {
  color: red;
  margin-bottom: 12px;
}
</style>
