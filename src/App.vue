<template>
  <nav>
    <router-link to="/">Home</router-link> |
    <router-link to="/about">About</router-link> |
    <router-link to="/create">Create</router-link> |
    <router-link to="/register">Register</router-link> |
    <router-link to="/login">login</router-link> |
    <router-link to="/me">Me</router-link> |
    <a href="javascript:void(0)" @click.prevent="logout">Logout</a>
  </nav>
  <router-view />
</template>
<script>
import gql from "graphql-tag";

export default {
  methods: {
    logout() {
      this.loading = true;
      this.$apollo
        .mutate({
          mutation: gql`
            mutation {
              logout {
                status
                message
              }
            }
          `,
        })
        .then((data) => {
          this.loading = false;
          localStorage.removeItem("apollo-token");
          this.$router.push({ name: "login" });
        })
        .catch((error) => {
          // this.loading = false;
          // const key = Object.keys(
          //   error.graphQLErrors[0].extensions.validation
          // )[0];
          // this.error = error.graphQLErrors[0].extensions.validation[key][0];
          console.log(error.graphQLErrors);
          this.error = error.graphQLErrors[0].message;
        });
    },
  },
};
</script>
<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

nav {
  padding: 30px;
}

nav a {
  font-weight: bold;
  color: #2c3e50;
}

nav a.router-link-exact-active {
  color: #42b983;
}
</style>
