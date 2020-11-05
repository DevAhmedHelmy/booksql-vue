<template>
  <div class="home">
    <!-- Apollo Query -->
    <ApolloQuery :query="require('@/graphql/queries/Categories.gql')">
      <template slot-scope="{ result: { data, loading }, isLoading }">
        <div v-if="isLoading">Loading...</div>
        <div v-else>
          <a href="#" @click.prevent="selectedCategoryID('all')"
            >All Categories</a
          >
          <a href="#" @click.prevent="selectedCategoryID('featured')"
            >Featured</a
          >
          <a
            href="#"
            v-for="category of data.categories.data"
            :key="category.id"
            class="link-margin"
            @click.prevent="selectedCategoryID(category.id)"
          >
            {{ category.id }} . {{ category.name }}
          </a>
        </div>
      </template>
    </ApolloQuery>

    <!-- <ApolloQuery :query="require('@/graphql/queries/Books.gql')">
      <template slot-scope="{ result: { data, loading }, isLoading }">
        <div v-if="isLoading">Loading...</div>
        <div v-else>
          <div v-for="book of data.books.data" :key="book.id">
            {{ book.id }} . {{ book.title }} {{ book.author }} {{ book.image }}
          </div>
        </div>
      </template>
    </ApolloQuery> -->
    <ApolloQuery
      :query="require('@/graphql/queries/Category.gql')"
      :variables="{ id: selectedCategory }"
    >
      <template slot-scope="{ result: { data, loading }, isLoading }">
        <div v-if="isLoading">Loading...</div>
        <div v-else>
          <div v-for="book of data.category.books" :key="book.id">
            {{ book.id }} . {{ book.title }} {{ book.author }} {{ book.image }}
          </div>
        </div>
      </template>
    </ApolloQuery>
  </div>
</template>

<script>
// @ is an alias to /src

import gql from "graphql-tag";
import categoryQuery from "@/graphql/queries/Category.gql";
import BookQuery from "@/graphql/queries/Category.gql";
import BookFeaturedQuery from "@/graphql/queries/Category.gql";
export default {
  name: "Home",
  components: {},
  data() {
    return {
      selectedCategory: 1,
      categories: [],
      books: [],
      query: null
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
    `,
    books: gql`
      {
        books {
          data {
            id
            title
            author
            image
          }
        }
      }
    `
  },
  methods: {
    selectedCategoryID(category) {
      if (category === "all") {
        this.query = BookQuery;
      } else if (category === "featured") {
        this.query = BookFeaturedQuery;
      } else {
        this.query = categoryQuery;
        this.selectedCategory = category;
      }
    }
  }
};
</script>
<style>
.link-margin {
  margin-right: 24px;
}
</style>
