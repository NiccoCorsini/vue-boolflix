<template>
  <main>
    <div
      class="film"
      v-for="(film, index) in objRes.results"
      :key="objRes.total_results + '-' + index"
    >
      <Film :film="film" />
    </div>
  </main>
</template>

<script>
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
    return {};
  },
  updated() {
    console.log(this.objRes);
  },
};
</script>

<style></style>
