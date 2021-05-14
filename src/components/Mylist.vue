<template>
  <div class="container">
    <Filmdetails
      :lista="lista"
      v-if="showDetails"
      :filmPage="filmPage"
      @detailsOff="detailsOff"
      @favourite="updateList"
      @remove="removeList"
    />
    <div v-else class="container">
      <div v-if="lista.length == 0" class="noitems">
        <h1>La tua lista non contiene nessun titolo al momento...</h1>
      </div>
      <div v-else class="list">
        <div
          v-for="(film, index) in lista"
          :key="film.id + index"
          class="film"
          @click="details(film)"
        >
          <button
            class="btn"
            @mousedown.prevent="
              $emit('remove', film);
              detailsOff(false);
            "
          >
            remove
          </button>
          <Film :film="film" />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Film from "../components/Film";
import Filmdetails from "../components/Filmdetails";

export default {
  name: "Mylist",
  props: {
    lista: Array,
  },
  components: {
    Film,
    Filmdetails,
  },
  data() {
    return {
      showDetails: false,
      showStart: false,
    };
  },
  methods: {
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
@import "../style/mylist";
</style>
