<template>
  <div class="home">
<!--    <img alt="Vue logo" src="../assets/logo.png" />-->
<!--    <HelloWorld msg="Welcome to Your Vue.js App" />-->
    <h1>List of Posts</h1>
    <div v-if="$apollo.queries.posts.loading">loading.....</div>
    <ul v-else>
      <li v-for="post in posts.data" :key="post.id">
        <router-link :to="{name:'post', params:{id:post.id}}" >{{post.title}}</router-link>
      </li>
    </ul>
  </div>
</template>

<script>
// @ is an alias to /src
// import HelloWorld from "@/components/HelloWorld.vue";
import gql from 'graphql-tag'

export default {
  name: "HomeView",
  components: {
    // HelloWorld,
  },
  apollo: {
    // Simple query that will update the 'hello' vue property
    posts: gql`
    query{
        posts
        {
          paginatorInfo{
            count
            currentPage
            perPage
            total
          }
          data{
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
ul{
  padding-left: 0;
}
ul > li{
  list-style-type: none;
}
</style>