<template>
  <div class="container">
    <Filmdetails
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
.container {
  display: flex;
  flex-wrap: wrap;
  max-width: 100%;
  .noitems {
    width: 100%;
    height: calc(100vh - 200px);
    display: flex;
    justify-content: center;
    align-items: center;
  }
  .list {
    display: flex;
  }
  .film {
    width: 342px;
    cursor: pointer;
    padding: 2rem 1rem;
    transition: transform 400ms;
    position: relative;
    &:hover {
      transform: scale(1.1);
    }
    button {
      position: absolute;
      bottom: 36px;
      right: 16px;
      z-index: 6;
    }
  }
}
</style>
