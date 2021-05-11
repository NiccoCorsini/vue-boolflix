<template>
  <div id="app">
    <Header @reSearch="research" />
    <Main :objRes="objRes" />
  </div>
</template>

<script>
import Header from "./components/Header";
import Main from "./components/Main";

export default {
  name: "App",
  components: {
    Header,
    Main,
  },
  data() {
    return {
      objRes: {},
    };
  },
  updated() {},
  methods: {
    research(text) {
      this.objRes = text;
    },
  },
};
</script>

<style lang="scss">
@import "https://cdnjs.cloudflare.com/ajax/libs/font-awesome/5.15.3/css/all.min.css";

@import "./style/general";
</style>
