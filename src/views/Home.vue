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
      categories: []
    };
  },
  apollo: {
    categories: gql`
      {
        categories {
          data {
            id
            name
          }
        }
      }
    `
  }
};
</script>
