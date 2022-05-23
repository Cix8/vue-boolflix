<template>
  <li @mouseover="showInfo = true" @mouseleave="showInfo = false">
    <template v-if="thisShow.backdrop_path !== null">
      <div class="img-container">
        <img
          :src="'https://image.tmdb.org/t/p/original' + thisShow.backdrop_path"
          alt=""
        />
      </div>
    </template>
    <template v-else>
      <div class="img-placeholder">
        <h2>{{ thisShow.name }}</h2>
      </div>
    </template>
    <div class="info" :class="showInfo ? '' : 'd-none'">
      <div class="title">
        <h2>
          Titolo: <span>{{ thisShow.name }}</span>
        </h2>
      </div>
      <div class="original-title">
        <h2>
          Titolo Originale: <span>{{ thisShow.original_name }}</span>
        </h2>
      </div>
      <div class="lang">
        <template v-if="langFlag(thisShow.original_language).length > 2">
          <div class="flag-container">
            <img
              :src="
                require('../assets/img/' +
                  langFlag(thisShow.original_language) +
                  '-flag.png')
              "
              alt=""
            />
          </div>
        </template>
        <template v-else>
          <div class="text">
            <span>Lingua: {{ thisShow.original_language.toUpperCase() }}</span>
          </div>
        </template>
      </div>
      <div class="rating" v-if="thisShow.vote_average">
        <strong
          >Voto:
          <i
            class="fas fa-star"
            v-for="(star, index) in starRating(parseInt(thisShow.vote_average))"
            :key="index"
          ></i>
        </strong>
      </div>
      <div class="overview" v-if="thisShow.overview">
        <p class="bold">
          Overview: <small>{{ thisShow.overview }}</small>
        </p>
      </div>
    </div>
  </li>
</template>

<script>
export default {
  name: "TvShowCard",
  props: {
    thisShow: Object,
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
