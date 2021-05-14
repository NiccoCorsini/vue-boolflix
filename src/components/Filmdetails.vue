<template>
  <div class="general">
    <div class="details">
      <div class="megaposter">
        <div class="details">
          <div class="dati">
            <i
              @click="$emit('detailsOff', false)"
              class="fas fa-arrow-left"
            ></i>
            <h1>{{ filmPage.title }}{{ filmPage.name }}</h1>
            <br />
            <h3>
              Vote: <span>{{ filmPage.vote_average }}</span>
            </h3>
            <p>{{ filmPage.overview }}</p>
            <h4>
              Release date:
              <span
                >{{ filmPage.release_date }}{{ filmPage.first_air_date }}</span
              >
            </h4>

            <div class="actions">
              <button
                class="btn"
                :class="{ activeadd: active == 'add' }"
                @click.prevent="
                  $emit('favourite', filmPage);
                  act('add');
                "
              >
                Add
              </button>

              <button
                style="margin-left: 20px"
                class="btn"
                :class="{ activerem: active == 'remove' }"
                @click.prevent="
                  $emit('remove', filmPage);
                  act('remove');
                "
              >
                remove
              </button>

              <button
                style="margin-left: 20px"
                class="btn"
                @click.prevent="showTrailer = true"
              >
                Trailer
              </button>
            </div>
          </div>
          <img
            class="posterdata"
            :src="imageUrl + filmPage.poster_path"
            :alt="filmPage.title"
          />
          <div v-if="showTrailer" class="trailer" @click="showTrailer = false">
            <Trailer
              :keyword="
                filmPage.media_type == 'movie' ? trailerMovie : trailerTv
              "
            />
          </div>
        </div>
        <img :src="imageUrl + filmPage.poster_path" :alt="filmPage.title" />
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import Trailer from "../components/Trailer";

export default {
  name: "Filmdetails",
  props: {
    filmPage: Object,
  },
  components: {
    Trailer,
  },
  data() {
    return {
      imageUrl: "https://image.tmdb.org/t/p/original",
      active: "",
      trailerMovie: "",
      trailerTv: "",
      showTrailer: false,
    };
  },
  created() {
    this.getTrailer();
    // console.log(this.filmPage.id);
    // console.log(this.trailerTv);
    setTimeout(() => {
      this.cazzi();
    }, 100);
  },
  updated() {
    // this.getTrailer();
  },
  methods: {
    act(param) {
      this.active = param;
    },
    getTrailer() {
      if (this.filmPage.media_type == "movie") {
        axios
          .get(
            `https://api.themoviedb.org/3/movie/${this.filmPage.id}/videos?api_key=44a7a4e50c9163f38b3c927adf4699c8`
          )

          .then((res) => {
            this.trailerMovie = res.data.results[0].key;
          });
      } else {
        axios
          .get(`https://api.themoviedb.org/3/tv/${this.filmPage.id}/videos`, {
            params: {
              api_key: "44a7a4e50c9163f38b3c927adf4699c8",
            },
          })
          .then((res) => {
            this.trailerTv = res.data.results[0].key;
          });
      }
    },
    cazzi() {
      console.log(this.trailerMovie);
    },
  },
};
</script>

<style scoped lang="scss">
@import "../style/filmdetails";
</style>
