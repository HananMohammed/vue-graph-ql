<template>
  <div>
    <h1>My Info</h1>
    {{ error }}
    <div v-if="me">
      <div>{{ me.name }}</div>
      <div>{{ me.email }}</div>
    </div>
  </div>
</template>
<script>
import gql from "graphql-tag";

export default {
  data() {
    return {
      error: null,
    };
  },
  apollo: {
    // Simple query that will update the 'hello' vue property
    me: {
      query: gql`
        query {
          me {
            name
            email
          }
        }
      `,
      error(error) {
        this.error = error.message;
      },
      // variables() {
      //   return {
      //     page: this.$route.query.page ? parseInt(this.$route.query.page) : 1,
      //   };
      // },
    },
  },
};
</script>
