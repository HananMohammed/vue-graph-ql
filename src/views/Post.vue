<template>
  <div class="about">
    <h1>This is an Post page for id {{ $route.params.id }}</h1>
    <div v-if="$apollo.queries.post.loading">loading.....</div>
    <div v-else>
      <div v-if="post">
        <h2>{{ post.title }}</h2>
        <div>{{ post.body }}</div>
      </div>
      <div v-else>
        <h2>No post Found</h2>
      </div>
    </div>
  </div>
</template>
<script>
// @ is an alias to /src
// import HelloWorld from "@/components/HelloWorld.vue";
import gql from "graphql-tag";

export default {
  name: "Post",
  components: {
    // HelloWorld,
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
};
</script>
