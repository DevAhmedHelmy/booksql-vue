<template>
  <div class="home"></div>
</template>

<script>
// @ is an alias to /src

import gql from "graphql-tag"; 
export default {
  name: "Home",
  components: {},
  data() {
    return {
      books: []
    };
  },
  apollo: {
    books: gql`
      {
        books {
          id
          name
        }
      }
    `
  }
};
</script>
