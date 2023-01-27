<template>
  <div class="about">
    <h1>Create Post</h1>
    <div class="error">{{ error }}</div>
    <div>
      <form action="#" @submit.prevent="createPost">
        <div>
          <label for="title">Title</label>
          <input v-model="title" type="text" name="title" id="title" />
        </div>
        <div>
          <label for="body">body</label>
          <textarea v-model="body" name="body" id="body"></textarea>
        </div>
        <div>
          <button>Create Post</button>
          <div v-if="loading"> loading....</div>
        </div>
      </form>
    </div>
  </div>
</template>
<script>
import gql from "graphql-tag";

export default {
  name: "CreatePost",
  data() {
    return {
      title: "",
      body: "",
      error: null,
      loading: false,
    };
  },
  methods: {
    createPost() {
      this.loading = true;
      this.$apollo
        .mutate({
          mutation: gql`
            mutation createPost(
              $user_id: ID!
              $title: String!
              $body: String!
            ) {
              createPost(user_id: $user_id, title: $title, body: $body) {
                id
                title
                body
                created_at
              }
            }
          `,
          variables: {
            user_id: 1,
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
