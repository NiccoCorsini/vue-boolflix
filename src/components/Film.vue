<template>
  <div class="content">
    <ul>
      <li><img :src="imageUrl + film.poster_path" :alt="film.name" /></li>
      <li>{{ film.title == undefined ? film.name : film.title }}</li>
      <li>
        {{
          film.original_title == undefined
            ? film.original_name
            : film.original_title
        }}
      </li>
      <li class="img">
        <span v-if="languageFlag(film.original_language)">
          <img
            :src="require(`../assets/img/${film.original_language}.png`)"
            :alt="film.original_language"
          />
        </span>
        <span v-else>{{ film.original_language }}</span>
      </li>
      <li :stars="Math.round(film.vote_average / 2)">
        <span
          class="yellow"
          v-for="(star, index) in Math.round(film.vote_average / 2)"
          :key="index"
          ><i class="fas fa-star"></i
        ></span>

        <span
          v-for="(star, index) in 5 - Math.round(film.vote_average / 2)"
          :key="index + 'B'"
          ><i class="far fa-star"></i
        ></span>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  name: "Film",
  props: ["film", "stars"],
  data() {
    return {
      vote: this.stars,
      imageUrl: "https://image.tmdb.org/t/p/w342/",
    };
  },
  methods: {
    languageFlag(lang) {
      return lang == "en" || lang == "it" ? true : false;
    },
  },
};
</script>

<style scoped lang="scss">
ul {
  li {
    &:last-child {
      span.yellow {
        color: yellow;
      }
    }
  }
  li.img {
    img {
      width: 24px;
    }
  }
}
</style>
