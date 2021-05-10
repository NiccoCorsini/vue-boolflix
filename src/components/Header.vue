<template>
  <header>
    <form action="">
      <input type="text" name="" id="" v-model.trim="search" />
      <button @click.prevent="$emit('reSearch', result)">
        Search
      </button>
    </form>
  </header>
</template>

<script>
import axios from "axios";

export default {
  name: "Header",
  data() {
    return {
      search: "",
      result: {},
    };
  },
  updated() {
    this.getApi();
    this.getApiTv();
  },
  methods: {
    getApi() {
      axios
        .get("https://api.themoviedb.org/3/search/movie", {
          params: {
            api_key: "44a7a4e50c9163f38b3c927adf4699c8",
            query: this.search,
          },
        })
        .then((res) => {
          //   console.log(res.data);
          this.result = res.data;
        });
    },
    getApiTv() {
      axios
        .get("https://api.themoviedb.org/3/search/tv", {
          params: {
            api_key: "44a7a4e50c9163f38b3c927adf4699c8",
            // language: "it-IT",
            query: this.search,
          },
        })
        .then((res) => {
          //   console.log(res.data);
          this.result = res.data;
        });
    },
  },
};
</script>

<style></style>
