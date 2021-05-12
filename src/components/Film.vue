<template>
  <div class="content">
    <div class="poster">
      <div class="layover">
        <ul>
          <li>
            <img :src="imageUrlPreview + film.poster_path" :alt="film.name" />
          </li>
          <li>
            Title:
            <strong>{{
              film.title == undefined ? film.name : film.title
            }}</strong>
          </li>
          <li>
            Original title:
            <strong>
              {{
                film.original_title == undefined
                  ? film.original_name
                  : film.original_title
              }}
            </strong>
          </li>
          <li class="img">
            Original language:
            <span v-if="languageFlag(film.original_language)">
              <img
                :src="require(`../assets/img/${film.original_language}.png`)"
                :alt="film.original_language"
              />
            </span>
            <span v-else>{{ film.original_language }}</span>
          </li>
          <li>
            Vote:
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
      <img
        :src="
          film.media_type == 'movie'
            ? imageUrlPreview + film.poster_path
            : imageUrl + film.poster_path
        "
        :alt="film.name"
      />
    </div>
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
      imageUrlPreview: "https://image.tmdb.org/t/p/w185/",
      imageUrlPp: "https://image.tmdb.org/t/p/w92/",
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
@import "../style/film";
</style>
