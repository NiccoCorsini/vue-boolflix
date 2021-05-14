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
.container {
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
      z-index: 5;
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
        p {
          line-height: 1.4rem;
        }
        h4 {
          margin-top: 2rem;
          span {
            background-color: rgba(255, 255, 255, 0.2);
            padding: 5px 10px;
            margin-left: 10px;
          }
        }
        .actions {
          margin-top: 30px;

          .btn.activeadd {
            position: relative;

            &::after {
              animation: ghost 600ms linear forwards;
              content: "Added";

              font-size: 14px;
              color: lightgreen;
              font-weight: 700;
              position: absolute;
              top: 0;
              left: 50%;
              transform: translateX(-50%);
            }

            @keyframes ghost {
              from {
                opacity: 0;
              }
              50% {
                opacity: 1;
                top: -10px;
              }
              to {
                opacity: 0;
                top: -20px;
              }
            }
          }

          .btn.activerem {
            position: relative;

            &::after {
              animation: ghost 600ms linear forwards;
              content: "Removed";

              font-size: 14px;
              color: lightgreen;
              font-weight: 700;
              position: absolute;
              top: 0;
              left: 50%;
              transform: translateX(-50%);
            }

            @keyframes ghost {
              from {
                opacity: 0;
              }
              50% {
                opacity: 1;
                top: -10px;
              }
              to {
                opacity: 0;
                top: -20px;
              }
            }
          }
        }
      }
      .posterdata {
        width: 500px;
        height: 700px;
        filter: blur(0);
        opacity: 1;
      }
      .trailer {
        animation: appear 500ms linear forwards;

        @keyframes appear {
          from {
            opacity: 0;
          }

          to {
            opacity: 1;
          }
        }
      }
    }
  }
}
</style>
