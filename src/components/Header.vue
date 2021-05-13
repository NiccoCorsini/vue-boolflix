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
            <li>
              <a
                @click.prevent="
                  $emit('home', true);
                  act('home');
                "
                :class="{ active: active == 'home' }"
                href="#Home"
                >Home</a
              >
            </li>
            <li><a href="#">Serie TV</a></li>
            <li><a href="#">Film</a></li>
            <li><a href="#">Nuovi e popolari</a></li>
            <li>
              <a
                @click.prevent="
                  $emit('list', true);
                  act('list');
                "
                :class="{ active: active == 'list' }"
                href="#Mylist"
                >La mia lista</a
              >
            </li>
          </ul>
        </nav>
      </div>
      <div class="right">
        <ul>
          <li @click.prevent="onfocus($event)">
            <i class="fas fa-search"></i>

            <input
              type="text"
              v-model.trim="search"
              placeholder="Titoli, Persone, Generi"
              @keyup="$emit('reSearch', result)"
              @keyup.esc="resetValue"
              ref="search"
            />
            <!-- @click.prevent="resetValue($event)" -->
            <i
              v-show="focusOn"
              @mousedown="resetValue($event)"
              class="fas fa-times"
            ></i>
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
      active: "home",
    };
  },
  updated() {
    this.resetLibrary();
    this.getApi();
    this.apiRecent();
    this.overX();
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
          if (this.search === "") {
            this.result = res.data;
          }
        });
    },
    onfocus(event) {
      event.stopPropagation();
      this.$refs.search.focus(event);
    },
    resetValue(event) {
      event.preventDefault();
      event.stopPropagation();
      this.search = "";
      this.$refs.search.focus(event);
    },
    resetLibrary() {
      if (this.search == "") {
        this.result = undefined;
        this.$emit("reSearch", {});
      }
    },
    overX() {
      if (this.search !== "") {
        this.focusOn = true;
      } else {
        this.focusOn = false;
      }
    },
    act(param) {
      this.active = param;
    },
  },
};
</script>

<style scoped lang="scss">
@import "../style/header";
</style>
