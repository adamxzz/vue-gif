<template>
  <div>
    <div class="search-box">
      <input type="text" v-model="term" v-on:keyup.enter="searchGiphy()" />
      <b-button class="float-end" variant="primary" @click="searchGiphy()"
        >Search</b-button
      >
    </div>
    <div>
      <!--Activity 1: Trending gifs button -->
      <button class="float-end" variant="secondary" @click="trendingGiphy()">
        Trending
      </button>
    </div>
          <!-- Activity 2: Random gifs button -->
      <button class="float-end" variant="secondary" @click="randomGiphy()">
        Random Gif
      </button>

    <b-card-group columns>
      <b-card
        v-for="gif in gifs"
        :key="gif.id"
        :img-src="gif.images.fixed_width.url"
        :img-alt="gif.title"
      >
        <b-card-text>
          <a :href="gif.url" target="_blank">{{ gif.title }}></a>
        </b-card-text>
      </b-card>
    </b-card-group>
    <img :src="gifs[0].images.fixed_width.url" alt="" />
    <p>{{ gifs[0].title }}</p>
  </div>
</template>

<script>
import axios from "axios";
const GIPHY_URL = "https://api.giphy.com/v1/gifs";
const API_KEY = "OLBjofZ2BGbKoS5dcUJWkBa53PWByED7";
export default {
  name: "GiphyViewer",
  data() {
    return {
      gifs: [],
    };
  },
  mounted() {
this.trendingGiphy();
  },
  methods: {
    searchGiphy() {
      if (!this.term) {
        alert("Please enter a search term");
        this.$byToast.toast("Please enter a search term", {
          title: "Warning",
          variant: "danger",
          toaster: "b-toaster-top-center",
          autoHideDelay: 5000,
          solid: true,
        });
        return;
      }
      axios
        .get(`${GIPHY_URL}/search?api_key=${API_KEY}&q=${this.term}&limit=20`)
        .then((response) => {
          console.log(response.data.data);
          this.gifs = response.data.data;
        })
        .catch((error) => console.log(error));
    },
    trendingGiphy() {
      axios
        .get(`${GIPHY_URL}/trending?api_key=${API_KEY}`)
        .then((response) => {
          console.log(response.data.data);
          this.gifs = response.data.data;
        })
        .catch((error) => console.log(error));
    },
    randomGiphy() {
        axios
        .get(`${GIPHY_URL}/random?api_key=${API_KEY}`)
        .then((response) => {
          console.log(response.data.data);
this.gifs = [response.data.data ]        })
        .catch((error) => console.log(error));
    },
  },
};
</script>
<style>
.card {
  margin-bottom: 20px;
}
@media (min-width: 34em) {
  .card-columns {
    -webkit-column-count: 2;
    -moz-column-count: 2;
    column-count: 2;
  }
}
@media (min-width: 48em) {
  .card-columns {
    -webkit-column-count: 3;
    -moz-column-count: 3;
    column-count: 3;
  }
}
@media (min-width: 62em) {
  .card-columns {
    -webkit-column-count: 4;
    -moz-column-count: 4;
    column-count: 4;
  }
}
@media (min-width: 75em) {
  .card-columns {
    -webkit-column-count: 5;
    -moz-column-count: 5;
    column-count: 5;
  }
}
</style>