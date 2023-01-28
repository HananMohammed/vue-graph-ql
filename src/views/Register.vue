<template>
  <div class="about">
    <h1>Login</h1>
    <div class="error">{{ error }}</div>
    <div>
      <form action="#" @submit.prevent="login">
        <div>
          <label for="name">Name</label>
          <input v-model="name" type="text" name="name" id="name" />
        </div>
        <div>
          <label for="email">Email</label>
          <input type="text" v-model="email" name="email" id="email" />
        </div>
        <div>
          <label for="password">password</label>
          <input
            type="password"
            v-model="password"
            name="password"
            id="password"
          />
        </div>
        <div>
          <label for="password_confirmation">password confirmation</label>
          <input
            type="password"
            v-model="password_confirmation"
            name="password_confirmation"
            id="password_confirmation"
          />
        </div>
        <div>
          <button>register</button>
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
      name: "",
      email: "",
      password: "",
      password_confirmation: "",
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
            mutation register($input: RegisterInput!) {
              register(input: $input) {
                token
                status
              }
            }
          `,
          variables: {
            input: {
              name: this.name,
              email: this.email,
              password: this.password,
              password_confirmation: this.password_confirmation,
            },
          },
        })
        .then((data) => {
          this.loading = false;
          localStorage.setItem("apollo-token", data.data.register.token);
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
