<template>
  <div class="home">
    <!--    <img alt="Vue logo" src="../assets/logo.png" />-->
    <!--    <HelloWorld msg="Welcome to Your Vue.js App" />-->
    <div>
      <h1>List of Posts</h1>
      <div v-if="$apollo.queries.posts.loading">loading.....</div>
      <ul v-else>
        <li v-for="post in posts.data" :key="post.id">
          <router-link :to="{ name: 'post', params: { id: post.id } }">{{
            post.title
          }}</router-link>
        </li>
      </ul>
    </div>

    <div>
      <h1>Apollo Query Components</h1>
      <ApolloQuery
        :query="
          (gql) => gql`
            query {
              posts {
                paginatorInfo {
                  count
                  currentPage
                  perPage
                  total
                }
                data {
                  id
                  title
                  body
                }
              }
            }
          `
        "
      >
        <template v-slot="{ result: { loading, error, data } }">
          <!-- Loading -->
          <div v-if="loading" class="loading apollo">Loading...</div>

          <!-- Error -->
          <div v-else-if="error" class="error apollo">An error occurred</div>

          <!-- Result -->
          <div v-else-if="data" class="result apollo">
            <ul>
              <li v-for="post in posts.data" :key="post.id">
                <router-link :to="{ name: 'post', params: { id: post.id } }">{{
                  post.title
                }}</router-link>
              </li>
            </ul>

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
  name: "HomeView",
  components: {
    // HelloWorld,
  },
  apollo: {
    // Simple query that will update the 'hello' vue property
    posts: gql`
      query {
        posts {
          paginatorInfo {
            count
            currentPage
            perPage
            total
          }
          data {
            id
            title
            body
          }
        }
      }
    `,
  },
};
</script>
<style>
ul {
  padding-left: 0;
}
ul > li {
  list-style-type: none;
}
</style>
