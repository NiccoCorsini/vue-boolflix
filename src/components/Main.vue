<template>
  <main>
    <div class="container">
      <Filmdetails
        v-if="showDetails"
        :filmPage="filmPage"
        @detailsOff="detailsOff"
        @favourite="updateList"
        @remove="removeList"
      />
      <div v-else class="discovery">
        <br />
        <h1 v-show="showStart">Serie TV</h1>
        <br />
        <div v-if="showStart" class="preview">
          <div
            class="film"
            v-for="(film, index) in objTv"
            :key="objTv.id + '-' + index"
            @click="details(film)"
          >
            <Film :film="film" />
          </div>
        </div>
        <br />
        <h1 v-show="showStart">Film</h1>
        <div v-if="showStart" class="pellicola">
          <div
            class="movie"
            v-for="(film, index) in objMovie"
            :key="objMovie.id + '-' + index"
            @click="details(film)"
          >
            <Film :film="film"> </Film>
          </div>
        </div>
        <div v-else class="searched">
          <div
            class="film"
            v-for="(film, index) in objRes.results"
            :key="film.id + '-' + index"
            @click="details(film)"
          >
            <Film :film="film"> </Film>
          </div>
        </div>
      </div>
    </div>
  </main>
</template>

<script>
import axios from "axios";
import Film from "../components/Film";
import Filmdetails from "../components/Filmdetails";

export default {
  name: "Main",
  components: {
    Film,
    Filmdetails,
  },
  props: {
    objRes: Object,
  },
  data() {
    return {
      objStart: {},
      showStart: false,
      objTv: [],
      objMovie: {},
      filmPage: {},
      showDetails: false,
      imageUrl: "https://image.tmdb.org/t/p/original",
    };
  },
  updated() {
    this.getApiStart();
  },
  created() {
    this.getApiStart();
  },
  methods: {
    getApiStart() {
      axios
        .get(
          "https://api.themoviedb.org/3/trending/all/week?api_key=44a7a4e50c9163f38b3c927adf4699c8"
        )
        .then((res) => {
          if (this.objRes.results === undefined) {
            this.showStart = true;
            this.objStart = res.data;

            this.objTv = res.data.results.filter(
              (element) => element.media_type == "tv"
            );

            this.objMovie = res.data.results.filter(
              (element) => element.media_type == "movie"
            );
          } else if (this.objRes.results !== undefined) {
            this.showStart = false;
          }
        });
    },
    details(film) {
      this.filmPage = film;
      this.showDetails = true;
    },
    detailsOff(boolean) {
      this.showDetails = boolean;
    },
    updateList(page) {
      this.$emit("favourite", page);
    },
    removeList(page) {
      this.$emit("remove", page);
    },
  },
};
</script>

<style scoped lang="scss">
@import "../style/main";
</style>
