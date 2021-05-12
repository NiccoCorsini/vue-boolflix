<template>
  <main class="container">
    <div v-if="showDetails" class="details">
      <div class="megaposter">
        <div class="details">
          <div class="dati">
            <i @click="detailsOff" class="fas fa-arrow-left"></i>
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
          </div>
          <img
            class="posterdata"
            :src="imageUrl + filmPage.poster_path"
            :alt="filmPage.title"
          />
        </div>
        <img :src="imageUrl + filmPage.poster_path" :alt="filmPage.title" />
      </div>
    </div>
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
      <div v-if="showStart" class="preview film">
        <div
          class="film movie"
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
          } else if (this.objRes.results.length > 0) {
            this.showStart = false;
          }
        });
    },
    details(film) {
      this.filmPage = film;
      this.showDetails = true;
    },
    detailsOff() {
      this.showDetails = false;
    },
  },
};
</script>

<style scoped lang="scss">
@import "../style/vars";
main.container {
  .megaposter {
    height: calc(100vh - 70px);
    width: 109.2%;
    transform: translateX(-5%);
    margin: -30px auto 0;
    overflow: hidden;
    position: relative;

    img {
      filter: blur(10px);
      width: 100%;
      opacity: 0.2;
      position: relative;
    }
    .details {
      position: absolute;
      width: 100%;
      height: 100%;
      padding: 4rem 6rem;
      display: flex;
      justify-content: center;
      .dati {
        width: 40%;
        padding-right: 4rem;
        i {
          cursor: pointer;
          z-index: 3;
          position: absolute;
          top: 20px;
          left: 70px;
          font-size: 2rem;
        }
        h1 {
          margin-bottom: 2.5rem;
        }
        h3 {
          margin-bottom: 2rem;
          span {
            background-color: rgba(255, 255, 255, 0.2);
            padding: 5px 10px;
            margin-left: 10px;
          }
        }
        h4 {
          margin-top: 2rem;
          span {
            background-color: rgba(255, 255, 255, 0.2);
            padding: 5px 10px;
            margin-left: 10px;
          }
        }
      }
      .posterdata {
        width: 500px;
        height: 700px;
        filter: blur(0);
        opacity: 1;
      }
    }
  }
  .searched,
  .preview {
    display: flex;
    flex-wrap: wrap;
    justify-content: space-between;
  }
  .preview.film {
    max-width: 100%;
    overflow-x: auto;
    flex-wrap: nowrap;

    .movie {
      width: 250px;
    }
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
