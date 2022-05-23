<template>
  <li @mouseover="showInfo = true" @mouseleave="showInfo = false">
    <template v-if="thisFilm.backdrop_path !== null">
      <div class="img-container">
        <img
          :src="'https://image.tmdb.org/t/p/original' + thisFilm.backdrop_path"
          alt=""
        />
      </div>
    </template>
    <template v-else>
      <div class="img-placeholder">
        <h2>{{ thisFilm.title }}</h2>
      </div>
    </template>
    <div class="info" :class="showInfo ? '' : 'd-none'">
      <div class="title">
        <h2>
          Titolo: <span>{{ thisFilm.title }}</span>
        </h2>
      </div>
      <div class="original-title">
        <h2>
          Titolo Originale: <span>{{ thisFilm.original_title }}</span>
        </h2>
      </div>
      <div class="lang">
        <template v-if="langFlag(thisFilm.original_language).length > 2">
          <div class="flag-container">
            <img
              :src="
                require('../assets/img/' +
                  langFlag(thisFilm.original_language) +
                  '-flag.png')
              "
              alt=""
            />
          </div>
        </template>
        <template v-else>
          <div class="text">
            <span>Lingua: {{ thisFilm.original_language.toUpperCase() }}</span>
          </div>
        </template>
      </div>
      <div class="rating">
        <strong
          >Voto:
          <i
            class="fas fa-star"
            v-for="(star, index) in starRating(parseInt(thisFilm.vote_average))"
            :key="index"
          ></i>
        </strong>
      </div>
      <div class="overview">
        <p class="bold">
          Overview: <small>{{ thisFilm.overview }}</small>
        </p>
      </div>
    </div>
  </li>
</template>

<script>
export default {
  name: "FilmCard",
  props: {
    thisFilm: Object,
  },
  data() {
    return {
      showInfo: false,
    };
  },
  methods: {
    langFlag(lang) {
      let result;
      let checkLang = lang.toLowerCase();
      if (checkLang === "it") {
        result = "italian";
      } else if (checkLang === "en") {
        result = "english";
      } else if (checkLang === "pt") {
        result = "portugues";
      } else if (checkLang === "es") {
        result = "espanol";
      } else {
        result = lang.toUpperCase();
      }
      return result;
    },

    starRating(vote) {
      let thisArray = [];
      const formatVote = vote / 2;
      for (let i = 0; i < formatVote; i++) {
        thisArray.push("star");
      }
      return thisArray;
    },
  },
};
</script>

<style lang="scss" scoped>
@import "../style/card_style.scss";
</style>
