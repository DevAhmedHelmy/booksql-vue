<template>
  <div class="home">
    <!-- Apollo Query -->
    <ApolloQuery :query="categoriesQuery">
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
    <div v-if="selectedCategory === 'all'">
      <ApolloQuery :query="query">
        <template slot-scope="{ result: { data, loading }, isLoading }">
          <div v-if="isLoading">Loading...</div>
          <div v-else>
            <div v-for="book of data.books.data" :key="book.id">
              <router-link :to="`/books/${book.id}`">
                {{ book.id }} . {{ book.title }}
              </router-link>
              <div>{{book.author}}</div>
              <img :src="book.image" alt="image">
            </div>
          </div>
        </template>
      </ApolloQuery>
    </div>
    <div v-else-if="selectedCategory === 'featured'">
      <ApolloQuery :query="query" :variables="{ featured: true }">
        <template slot-scope="{ result: { data, loading }, isLoading }">
          <div v-if="isLoading">Loading...</div>
          <div v-else>
            <div v-for="book of data.booksByFeatured" :key="book.id">
              <router-link :to="`/books/${book.id}`">
                {{ book.id }} . {{ book.title }}
              </router-link>
            </div>
          </div>
        </template>
      </ApolloQuery>
    </div>
    <div v-else>
      <ApolloQuery :query="categoryQuery" :variables="{ id: selectedCategory }">
        <template slot-scope="{ result: { data, loading }, isLoading }">
          <div v-if="isLoading">Loading...</div>
          <div v-else>
            <div v-for="book of data.category.books" :key="book.id">
              <router-link :to="`/books/${book.id}`">
                {{ book.id }} . {{ book.title }}
              </router-link>
            </div>
          </div>
        </template>
      </ApolloQuery>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import categoriesQuery from "@/graphql/queries/Categories.gql";
import categoryQuery from "@/graphql/queries/Category.gql";
import BooksQuery from "@/graphql/queries/Books.gql";
import BookFeaturedQuery from "@/graphql/queries/BookFeaturedQuery.gql";
export default {
  name: "Home",
  components: {},
  data() {
    return {
      selectedCategory: "all",
      categoriesQuery,
      categoryQuery,
      BookFeaturedQuery,
      query: BooksQuery,
    };
  },

  methods: {
    selectedCategoryID(category) {
      if (category === "all") {
        this.query = BooksQuery;
      } else if (category === "featured") {
        this.query = BookFeaturedQuery;
      } else {
        this.query = categoryQuery;
      }
      this.selectedCategory = category;
    },
  },
};
</script>
<style>
.link-margin {
  margin-right: 24px;
}
</style>
