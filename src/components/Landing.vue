<template>
  <div class="landing">
    <div class="jumbotron w-100">
      <h2 class="shadow-text text-white mt-5">Welcome to True Story News!</h2>
      <p class="lead shadow-text text-white" style="font-size: 22px;">
        View the latest news today for world, weather, entertainment, politics
        and health
      </p>
      <div class="text-center mb-3">
        <h5 class="text-white shadow-text">Follow us!</h5>
      </div>
      <div class="follow-us">
        <a href="http://www.twitter.com" target="_blank">
          <span class="fa fa-twitter-square"></span
        ></a>
        <a href="http://www.facebook.com" target="_blank"
          ><span class="fa fa-facebook-square mx-4"></span
        ></a>
        <a href="http://www.instagram.com" target="_blank"
          ><span class="fa fa-instagram"></span
        ></a>
      </div>
      <hr class="my-4" />
      <form class="text-white shadow-text font-weight-bold">
        <div class="input-group container" style="max-width: 400px">
          <input
            type="text"
            class="form-control"
            :placeholder="'Search by ' + selected_filter"
            v-model="keyword_value"
            v-if="selected_filter === 'Keyword'"
          />
          <select v-else class="form-control" v-model="selected_source">
            <option value="bbc-news">BBC NEWS</option>
            <option value="google-news">Google News</option>
            <option value="ESPN">ESPN</option>
            <option value="cnn">CNN</option>
          </select>
          <div class="input-group-btn">
            <button
              class="btn custom-button"
              type="submit"
              @click.prevent="searchBy(selected_filter)"
            >
              <span class="fa fa-search"></span>
            </button>
          </div>
        </div>
        <div class="text-center mt-2" @click.prevent="showFilters">
          Show more filters
          <span
            class="fa"
            v-bind:class="{ 'fa-arrow-up': filters, 'fa-arrow-down': !filters }"
          ></span>
        </div>
        <div class="filters" v-if="filters">
          <select
            class="form-control"
            style="max-width: 300px"
            v-model="selected_filter"
          >
            <option>Keyword</option>
            <option>Source</option>
          </select>
        </div>
      </form>
    </div>
    <div class="row w-100 pb-5">
      <div class="col-12 col-lg-9">
        <div class="container-md py-3" v-if="news">
          <h3 class="text-left news-header">{{ news[5].title }}</h3>
          <div class="card">
            <img
              :src="news[5].urlToImage"
              alt="Avatar"
              style="width:100%"
              v-if="news.urlToImage"
            />
            <img
              src="https://cdn3.iconfinder.com/data/icons/ballicons-reloaded-free/512/icon-70-512.png"
              width="50%"
              alt=""
              style="display: block; margin-left: auto; margin-right: auto;"
              v-else
            />
            <div class="container text-left">
            <a :href="news[5].url" class="btn custom-button text-white btn-sm mb-2" target="_blank">Read full article</a> 
            <br/>
              <small>{{ news[5].publishedAt }}</small>
            </div>
          </div>
        </div>
        <div class="container-md py-3" v-if="news">
          <h3 class="text-left news-header">{{ news[0].title }}</h3>
          <div class="card">
            <img
              :src="news[0].urlToImage"
              alt="Avatar"
              style="width:100%"
              v-if="news.urlToImage"
            />
            <img
              src="https://cdn3.iconfinder.com/data/icons/ballicons-reloaded-free/512/icon-70-512.png"
              width="50%"
              alt=""
              style="display: block; margin-left: auto; margin-right: auto;"
              v-else
            />
            <div class="container text-left">
            <a :href="news[0].url" class="btn custom-button text-white btn-sm mb-2" target="_blank"> Read full article</a> 
            <br/>
              <small>{{ news[0].publishedAt }}</small>
            </div>
          </div>
        </div>
      </div>
      <div class="col-lg-3 col-12 justify-content-center">
        <h1 class="mt-lg-0 mt-5">Breaking News</h1>
        <div class="f-flex container-md">
          <!-- eslint-disable vue/no-use-v-if-with-v-for,vue/no-confusing-v-for-v-if -->

          <div
            class="card my-4 pr-0"
            v-for="(onenews, index) in news"
            :key="onenews.key"
            v-if="index > 0 && index < 4"
          >
            <img
              :src="onenews.urlToImage"
              alt="Avatar"
              style="width:100%"
              v-if="onenews.urlToImage"
            />
            <img
              src="https://cdn3.iconfinder.com/data/icons/ballicons-reloaded-free/512/icon-70-512.png"
              width="50%"
              alt=""
              style="display: block; margin-left: auto; margin-right: auto;"
              v-else
            />
            <div class="container text-left">
              <p>
                {{ onenews.title }}
              </p>
              <p>
                <a
                  class="color: black; text-decoration-none"
                  :href="onenews.url"
                  target="_blank"
                  >Read more...</a
                >
              </p>
              <small>{{ onenews.publishedAt }}</small>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "Landing",
  data() {
    return {
      news: null,
      filters: false,
      selected_filter: "Keyword",
      search_news: null,
      keyword_value: "",
      selected_source: "bbc-news",
    };
  },
  mounted() {
    axios
      .get(
        "https://newsapi.org/v2/top-headlines?" +
          "country=us&" +
          "apiKey=25ec27623cd24e69a0cb47b0364e8a97"
      )
      .then((response) => {
        //this.news.push(response.data.articles);
        let arts = response.data.articles;
        this.news = [];
        for (let art in arts) {
          this.news.push(arts[art]);
        }
      })
      .catch((err) => console.log(err.message));
  },
  methods: {
    showFilters() {
      this.filters = !this.filters;
    },
    searchBy(filter) {
      if (filter === "Source") {
        axios
          .get(
            "https://newsapi.org/v2/top-headlines?" +
              `sources=${this.selected_source}&` +
              "apiKey=25ec27623cd24e69a0cb47b0364e8a97"
          )
          .then((response) => {
            //this.news.push(response.data.articles);
            let search_value = response.data.articles;
            this.search_news = [];
            for (let value in search_value) {
              this.search_news.push(search_value[value]);
            }
          })
          .catch((err) => console.log(err.message))
          .finally(() => {
            if (this.search_news !== null) {
              this.$store.state.searchResults = this.search_news;
              this.$router.push(`/search-news/${this.selected_source}`);
            } else {
              alert("No results found");
            }
          });
      }
      if (filter === "Keyword") {
        if (this.keyword_value !== "") {
          axios
            .get(
              "https://newsapi.org/v2/top-headlines?" +
                `q=${this.keyword_value}&` +
                "from=2020-07-30&" +
                "sortBy=popularity&" +
                "apiKey=25ec27623cd24e69a0cb47b0364e8a97"
            )
            .then((response) => {
              //this.news.push(response.data.articles);
              let search_value = response.data.articles;
              this.search_news = [];
              for (let value in search_value) {
                this.search_news.push(search_value[value]);
              }
            })
            .catch((err) => console.log(err.message))
            .finally(() => {
              if (this.search_news !== null) {
                this.$store.state.searchResults = this.search_news;
                this.$router.push(`/search-news/${this.keyword_value}`);
              } else {
                alert("No results found");
              }
            });
        } else {
          alert("Please enter a keyword");
        }
      }
    },
  },
  computed: {
    searchResults() {
      return this.$store.state.searchResults;
    },
  },
};
</script>

<style>
.landing {
  width: 100%;
  height: 100vh;
  position: absolute;
}

.landing .follow-us {
  text-align: center;
  font-size: 28px;
}

.follow-us a {
  color: white;
  text-decoration: none;
  text-shadow: 2px 2px 2px rgba(0, 0, 0, 0.9);
}
.filters {
  display: flex;
  justify-content: center;
  margin-top: 25px;
}
.landing .card {
  /* Add shadows to create the "card" effect */
  box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2);
  transition: 0.3s;
}

.landing .jumbotron {
  height: 100vh !important;
  background: linear-gradient(to right, transparent 0%, rgba(0, 0, 0, 0.3) 100%),
    url("https://cdn.pixabay.com/photo/2014/05/03/01/04/manhattan-336708_960_720.jpg")
      no-repeat center center;
  -webkit-background-size: cover;
  -moz-background-size: cover;
  -o-background-size: cover;
  background-size: cover;
}

.landing .card:hover {
  box-shadow: 0 8px 16px 0 rgba(0, 0, 0, 0.2);
}

.landing .container {
  padding: 2px 16px;
}
.landing .card img:before {
  content: "";
  background: white;
}
.landing .custom-button {
  background-color: rgb(228, 80, 80);
  font-weight: 600;
  border-radius: 0 5px 5px 0;
}
.card a {
  color: black;
}

.color-one {
  color: rgb(228, 80, 80);
}
.landing .custom-button:hover {
  cursor: pointer;
}
.shadow-text {
  text-shadow: 3px 3px 3px rgba(0, 0, 0, 0.9);
}

.text-white {
  color: #ccc;
}
.shadow-content {
  box-shadow: 0px 0px 20px 0 rgba(0, 0, 0, 0.3);
}
@media screen and (max-width: 900px) {
  .landing .news-header {
    font-size: 20px;
  }
  .col-12 {
    padding-right: 0 !important;
    padding-left: 30px;
  }
}
</style>
