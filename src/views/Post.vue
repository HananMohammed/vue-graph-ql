<template>
  <div class="about">
    <h1>This is an Post page for id {{ $route.params.id }}</h1>
    <div>
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
    <div>
      <h1>Apollo Query Components</h1>
      <ApolloQuery
        :query="
          (gql) => gql`
            query getPost($id: ID!) {
              post(id: $id) {
                title
                body
              }
            }
          `
        "
        :variables="{ id: $route.params.id }"
      >
        <template v-slot="{ result: { loading, error, data } }">
          <!-- Loading -->
          <div v-if="loading" class="loading apollo">Loading...</div>

          <!-- Error -->
          <div v-else-if="error" class="error apollo">An error occurred</div>

          <!-- Result -->
          <div v-else-if="data" class="result apollo">
            <h2>{{ post.title }}</h2>
            <div>{{ post.body }}</div>
          </div>

          <!-- No result -->
          <div v-else class="no-result apollo">No result :(</div>
        </template>
      </ApolloQuery>
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
