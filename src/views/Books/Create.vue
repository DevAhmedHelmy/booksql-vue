<template>
  <div class="addBook">
    <h1>This is an addBooke</h1>

    <form @submit.prevent="addBook">
      <div class="form-group">
        <label for="exampleInputTitle">Title</label>
        <input
          type="text"
          id="exampleInputTitle"
          aria-describedby="titleHelp"
          placeholder="Enter title"
          v-model="title"
        />
      </div>
      <div class="form-group">
        <label for="exampleInputAuthor">Author</label>
        <input
          type="text"
          id="exampleInputAuthor"
          aria-describedby="authorHelp"
          placeholder="Enter Author"
          v-model="author"
        />
      </div>
      <div class="form-group">
        <label for="exampleInputDescription">Description</label>
        <input
          type="text"
          id="exampleInputDescription"
          aria-describedby="DescriptionHelp"
          placeholder="Enter Description"
          v-model="description"
        />
      </div>
      <div class="form-group">
        <label for="exampleInputImage">Image</label>
        <input type="text" name="image" id="" v-model="image" />
      </div>
      <div class="form-group">
        <label for="link">Link</label>
        <input type="text" name="link" id="link" v-model="link" />
      </div>
      <div class="form-group">
        <ApolloQuery :query="require('@/graphql/queries/Categories.gql')">
          <template slot-scope="{ result: { data, loading }, isLoading }">
            <div v-if="isLoading">Loading...</div>
            <select v-model="category" v-else>
              <option
                v-for="category of data.categories.data"
                :key="category.id"
                :value="category.id"
                class="link-margin"
              >
                {{ category.name }}
              </option>
            </select>
          </template>
        </ApolloQuery>
      </div>
      <div class="form-check">
        <input
          type="checkbox"
          class="form-check-input"
          id="featured"
          v-model="featured"
          name="featured"
        />
        <label class="form-check-label" for="featured">Featured</label>
      </div>
      <button type="submit" class="btn btn-primary">Submit</button>
    </form>
  </div>
</template>
<script>
import addBook from "@/graphql/mutations/AddBook.gql";
export default {
  data() {
    return {
      title: "",
      author: "",
      image: "",
      link: "",
      description: "",
      featured: false,
      category: 1
    };
  },
  methods: {
    addBook() {
      this.$apollo
        .mutate({
          // Query
          mutation: addBook,
          // Parameters
          variables: {
            title:this.title, 
            author:this.author,
            link:this.link,
            image:this.image,
            description:this.description,
            featured:this.featured, 
            category:this.category
          }
        })
        .then(data => {
          console.log(data);
        })
        .catch(error => {
          console.error(error);
        });
    }
  }
};
</script>
<style scoped>
.form-group {
  margin-bottom: 32px;
}
input[type="text"] {
  padding: 10px 14px;
}
button {
  padding: 16px;
  background: #027bff;
  color: #fff;
  border-radius: 5px;
  font-size: 16px;
}
</style>
