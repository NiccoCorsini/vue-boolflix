<template>
  <header>
    <div class="container">
      <div class="left">
        <nav>
          <ul>
            <li>
              <a href="#"
                ><img
                  src="https://fontmeme.com/permalink/210511/69dfe28dc8c9bdcdb8ca6878bedab0b5.png"
                  alt="Netflix"
              /></a>
            </li>
            <li><a href="#">Home</a></li>
            <li><a href="#">Serie TV</a></li>
            <li><a href="#">Film</a></li>
            <li><a href="#">Nuovi e popolari</a></li>
            <li><a href="#">La mia lista</a></li>
          </ul>
        </nav>
      </div>
      <div class="right">
        <ul>
          <li @click="onfocus">
            <i class="fas fa-search"></i>
            <input
              type="text"
              v-model.trim="search"
              placeholder="Titoli, Persone, Generi"
              ref="search"
              @keyup="$emit('reSearch', result)"
              @keyup.esc="resetValue"
            />

            <!-- <form :class="{ active: focusOn }" action="">
              <input type="text" name="" id="" v-model.trim="search" />
              <button @click.prevent="$emit('reSearch', result)">
                Search
              </button>
            </form> -->
          </li>

          <li><a href="#">BAMBINI</a></li>
          <li>
            <a href="#"><i class="fas fa-gift"></i></a>
          </li>
          <li>
            <a href="#"><i class="fas fa-bell"></i></a>
          </li>
          <li>
            <a href="#"
              ><img :src="require('../assets/img/avatar.png')" alt="User"/></a
            ><i class="fas fa-caret-down"></i>
          </li>
        </ul>
      </div>
    </div>
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
      focusOn: false,
    };
  },
  updated() {
    this.getApi();
    this.apiRecent();
  },
  methods: {
    getApi() {
      if (this.search.length > 0) {
        axios
          .get("http://api.themoviedb.org/3/search/movie/", {
            params: {
              api_key: "44a7a4e50c9163f38b3c927adf4699c8",
              query: this.search,
            },
          })
          .then((res) => {
            this.result = res.data;
            if (res.data.results.length === 0) {
              axios
                .get("http://api.themoviedb.org/3/search/tv/", {
                  params: {
                    api_key: "44a7a4e50c9163f38b3c927adf4699c8",
                    // language: "it-IT",
                    query: this.search,
                  },
                })
                .then((res) => {
                  this.result = res.data;
                });
            }
          });
      }
    },
    apiRecent() {
      axios
        .get(
          "https://api.themoviedb.org/3/trending/all/week?api_key=44a7a4e50c9163f38b3c927adf4699c8"
        )
        .then((res) => {
          // this.result = res;
          if (this.search === "") {
            this.result = res.data;
            console.log(res);
          }
        });
    },
    onfocus() {
      this.$refs.search.focus();
    },
    resetValue() {
      this.search = "";
    },
  },
};
</script>

<style scoped lang="scss">
@import "../style/vars";

header {
  z-index: 3;
  width: 100%;
  height: 70px;
  position: fixed;
  top: 0;
  background-color: $primary-bg-color;
  font-size: 14px;
  font-weight: 100;
  .container {
    display: flex;
    align-items: center;
    height: 100%;
    ul {
      display: flex;
      align-items: center;
      li {
        &:not(:last-child) {
          margin-right: 1.5rem;
        }
      }
    }
    .left {
      width: 60%;
      ul {
        li {
          &:first-child {
            margin-right: 2.5rem;
            img {
              width: 100px;
              position: relative;
              top: 2px;
            }
          }
          &:nth-child(2) {
            font-weight: 700;
          }
        }
      }
    }
    .right {
      width: 40%;
      display: flex;
      justify-content: flex-end;
      ul {
        li {
          i {
            font-size: 1.2rem;
          }
          &:last-child {
            img {
              width: 36px;
              border-radius: 5px;
              margin-right: 0.6rem;
            }
            i {
              position: relative;
              bottom: 10px;
              transition: transform 300ms;
            }
            &:hover {
              i {
                transform: rotate(180deg);
              }
            }
          }
          &:first-child {
            cursor: pointer;
            display: flex;
            align-items: center;
            position: relative;

            // form {
            //   width: 0;
            //   overflow: hidden;
            //   transition: width 300ms;
            // }

            i {
              position: absolute;
              left: 7px;
              cursor: pointer;
              z-index: 3;
            }

            input {
              transition: all 300ms;
              width: 0;
              outline: none;
              background: transparent;
              border: 1px solid white;
              padding: 7px 0px 7px 30px;
              color: white;
              opacity: 0;
              &::placeholder {
                color: rgb(170, 170, 170);
                padding-left: 30px;
              }

              &:focus {
                width: 240px;
                opacity: 1;
              }
            }

            // form.active {
            //   width: 250px;
            // }
          }
        }
      }
    }
  }
}
</style>
