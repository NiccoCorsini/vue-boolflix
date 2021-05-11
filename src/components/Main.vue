<template>
  <main class="container">
    <!-- <h1>tv</h1> -->
    <div v-if="showStart" class="preview">
      <!-- <div
        class="film"
        v-for="(film, index) in objTv"
        :key="objTv.id + '-' + index"
      >
        <Film :film="film" />
      </div> -->

      <div
        class="film"
        v-for="(film, index) in objStart.results"
        :key="objStart.total_results + '-' + index"
      >
        <Film :film="film" />
      </div>
    </div>

    <div v-else class="searched">
      <div
        class="film"
        v-for="(film, index) in objRes.results"
        :key="film.id + '-' + index"
      >
        <Film :film="film" />
      </div>
    </div>
  </main>
</template>

<script>
import axios from "axios";
import Film from "../components/Film";

export default {
  name: "Main",
  components: {
    Film,
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
    };
  },
  updated() {
    this.getApiStart();
    // console.log(this.objTv);
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
            // res.data.results.forEach((element) => {
            //   if (element.media_type == "tv") {
            //     this.objTv.push(element);
            //   }
            // });

            // this.objTv = res.data.results.filter(
            //   (element) => element.media_type == "tv"
            // );
          } else if (this.objRes.results.length > 0) {
            this.showStart = false;
          }
        });
    },
  },
};
</script>

<style scoped lang="scss">
@import "../style/vars";
main.container {
  .searched,
  .preview {
    display: flex;
    flex-wrap: wrap;
    justify-content: space-between;
  }
  .film {
    cursor: pointer;
    padding: 2rem 1rem;
    transition: transform 400ms;
    &:hover {
      transform: scale(1.1);
    }
  }
}
</style>
