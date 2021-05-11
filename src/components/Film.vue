<template>
  <div class="content">
    <div class="poster">
      <div class="layover">
        <ul>
          <li>
            <img :src="imageUrlPreview + film.poster_path" :alt="film.name" />
          </li>
          <li>Title: {{ film.title == undefined ? film.name : film.title }}</li>
          <li>
            Original title:
            {{
              film.original_title == undefined
                ? film.original_name
                : film.original_title
            }}
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
      <img :src="imageUrl + film.poster_path" :alt="film.name" />
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
      width: 30px;
      margin-left: 1rem;
      vertical-align: middle;
    }
  }
}

.poster {
  position: relative;
  .layover {
    background: rgba(0, 0, 0, 0.85);
    display: flex;
    flex-direction: column;
    padding: 2rem;

    justify-content: flex-end;
    align-items: center;
    position: absolute;
    width: 100%;
    height: 100%;
    opacity: 0;
    transition: opacity 400ms;
    &:hover {
      opacity: 1;
    }
    ul {
      li {
        text-align: center;
        font-size: 12px;
        margin-top: 1.2rem;
        &:first-child {
          img {
            display: block;
            margin: 0 auto;
          }
        }
      }
    }
  }
}
</style>
