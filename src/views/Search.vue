<template>
  <div class="search-result">
    <div class="header text-center mt-3 p-4">
      <h4>Showing Results for "{{ $route.params.id }}"</h4>
    </div>
    <div class="results-cards container">
      <div
        class="card mx-auto my-3"
        style="width: 18rem;"
        v-for="res in searchResults"
        :key="res.key"
      >
        <img class="card-img-top" :src="res.urlToImage" alt="Card image cap" v-if="res.urlToImage" />
        <img
              src="https://cdn3.iconfinder.com/data/icons/ballicons-reloaded-free/512/icon-70-512.png"
              width="50%"
              alt=""
              style="display: block; margin-left: auto; margin-right: auto;"
              v-else
            />
        <div class="card-body">
          <h5 class="card-title">{{ res.title }}</h5>

          <a :href="res.url" target="_blank" class="btn btn-primary text-white">Read full article</a>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  mounted() {
    console.log(this.searchResults);
  },
  computed: {
    searchResults() {
      return this.$store.state.searchResults;
    },
  },
};
</script>

<style>
.search-result {
  background-color: white;
}

.search-result .results-cards {
  display: flex;
  justify-content: space-between;
  flex-wrap: wrap;
  align-items: flex-start;
}
</style>
