<template>
  <div class="about">
    <h1>Login</h1>
    <div class="error">{{ error }}</div>
    <div>
      <form action="#" @submit.prevent="login">
        <div>
          <label for="email">Email</label>
          <input type="text" v-model="email" name="email" id="email" />
        </div>
        <div>
          <label for="password">password</label>
          <input type="text" v-model="password" name="password" id="password" />
        </div>
        <div>
          <button>Login</button>
          <div v-if="loading">loading....</div>
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
      email: "",
      password: "",
      error: null,
      loading: false,
    };
  },
  methods: {
    login() {
      this.loading = true;
      this.$apollo
        .mutate({
          mutation: gql`
            mutation login($input: LoginInput!) {
              login(input: $input) {
                token
              }
            }
          `,
          variables: {
            input: {
              email: this.email,
              password: this.password,
            },
          },
        })
        .then((data) => {
          this.loading = false;
          localStorage.setItem("apollo-token", data.data.login.token);
          this.$router.push({ name: "me" });
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
.error {
  color: red;
  margin-bottom: 12px;
}
</style>
