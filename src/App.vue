<template>
  <div id="app">
    <Header @reSearch="research" @list="activateList" @home="activateHome" />

    <Main
      v-show="activateHomeBool"
      :objRes="objRes"
      @favourite="addToFavourite"
      @remove="removeFromFavourite"
    />

    <Mylist
      v-show="activateListBool"
      :lista="favouriteList"
      @remove="removeFromFavourite"
    />
  </div>
</template>

<script>
import Header from "./components/Header";
import Main from "./components/Main";
import Mylist from "./components/Mylist";

export default {
  name: "App",
  components: {
    Header,
    Main,
    Mylist,
  },
  data() {
    return {
      objRes: {},
      activateHomeBool: true,
      activateListBool: false,
      favouriteList: [],
      closeDetails: undefined,
    };
  },
  updated() {},
  methods: {
    research(text) {
      this.objRes = text;
    },
    activateList(boolean) {
      this.activateListBool = boolean;
      if (this.activateListBool) {
        this.activateHomeBool = false;
      }
    },
    activateHome(boolean) {
      this.activateHomeBool = boolean;
      if (this.activateHomeBool) {
        this.activateListBool = false;
      }
    },
    addToFavourite(film) {
      if (!this.favouriteList.includes(film)) {
        this.favouriteList.push(film);
      }
    },
    removeFromFavourite(film) {
      this.favouriteList.forEach((element, index) => {
        if (element.id === film.id) {
          this.favouriteList.splice(index, 1);
        }
      });
    },
  },
};
</script>

<style lang="scss">
@import "https://cdnjs.cloudflare.com/ajax/libs/font-awesome/5.15.3/css/all.min.css";

@import "./style/general";
</style>
