<template>
  <li
    @mouseover="showInfo = true"
    @mouseleave="(showInfo = false), (showDetails = false)"
  >
    <template v-if="thisContent.backdrop_path !== null">
      <div class="img-container">
        <img
          :src="
            'https://image.tmdb.org/t/p/original' + thisContent.backdrop_path
          "
          alt=""
        />
      </div>
    </template>
    <template v-else>
      <div class="img-placeholder">
        <h2>{{ thisContent.title ? thisContent.title : thisContent.name }}</h2>
      </div>
    </template>
    <div
      class="info"
      :class="showInfo ? '' : 'd-none'"
      @click="findCastAndGenres(thisContent)"
    >
      <div class="details" v-if="showDetails">
        <div class="cast">
          <div class="cast-title">Cast:</div>
          <div v-for="(people, index) in cast" :key="index">
            {{ people.original_name }}
          </div>
        </div>
        <div class="genre">
          <div class="genre-title">Genere:</div>
          <div v-for="(genre, index) in genres" :key="index">
            {{ genre.name }}
          </div>
        </div>
      </div>
      <div class="title">
        <h2>
          Titolo:
          <span>{{
            thisContent.title ? thisContent.title : thisContent.name
          }}</span>
        </h2>
      </div>
      <div class="original-title">
        <h2>
          Titolo Originale:
          <span>{{
            thisContent.original_title
              ? thisContent.original_title
              : thisContent.original_name
          }}</span>
        </h2>
      </div>
      <div class="lang">
        <template v-if="langFlag(thisContent.original_language).length > 2">
          <div class="flag-container">
            <img
              :src="
                require('../assets/img/' +
                  langFlag(thisContent.original_language) +
                  '-flag.png')
              "
              alt=""
            />
          </div>
        </template>
        <template v-else>
          <div class="text">
            <span
              >Lingua: {{ thisContent.original_language.toUpperCase() }}</span
            >
          </div>
        </template>
      </div>
      <div class="rating" v-if="thisContent.vote_average">
        <strong
          >Voto:
          <i
            v-for="(star, index) in starRating(thisContent.vote_average)"
            :key="index"
            class="fas fa-star"
            :class="star ? 'full-star' : ''"
          ></i>
        </strong>
      </div>
      <div class="overview" v-if="thisContent.overview">
        <p class="bold">
          Overview: <small>{{ thisContent.overview }}</small>
        </p>
      </div>
    </div>
  </li>
</template>

<script>
import axios from "axios";

export default {
  name: "ContentCard",
  props: {
    thisContent: Object,
  },
  data() {
    return {
      cast: [],
      genres: [],
      showDetails: false,
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
      } else if (checkLang === "fr") {
        result = "french";
      } else if (checkLang === "hi") {
        result = "hawaiian";
      } else if (checkLang === "de") {
        result = "german";
      } else if (checkLang === "ja") {
        result = "japanese";
      } else if (checkLang === "no") {
        result = "norwegian";
      } else {
        result = lang.toUpperCase();
      }
      return result;
    },

    starRating(vote) {
      let thisArray = [];
      const formatVote = parseInt(vote) / 2;
      for (let i = 0; i < formatVote; i++) {
        thisArray.push("star");
      }
      thisArray.length = 5;
      return thisArray;
    },

    findCastAndGenres(obj) {
      if (obj.title) {
        const req1 = axios.get(
          `https://api.themoviedb.org/3/movie/${obj.id}/credits?api_key=5281cccae9a725e7baaa26749f7bb197`
        );
        const req2 = axios.get(
          `https://api.themoviedb.org/3/movie/${obj.id}?api_key=5281cccae9a725e7baaa26749f7bb197`
        );

        axios.all([req1, req2]).then((resp) => {
          this.cast = resp[0].data.cast.splice(0, 5);
          this.genres = resp[1].data.genres;
          this.showDetails = !this.showDetails;
        });
      } else {
        const req1 = axios.get(
          `https://api.themoviedb.org/3/tv/${obj.id}/credits?api_key=5281cccae9a725e7baaa26749f7bb197`
        );
        const req2 = axios.get(
          `https://api.themoviedb.org/3/tv/${obj.id}?api_key=5281cccae9a725e7baaa26749f7bb197`
        );

        axios.all([req1, req2]).then((resp) => {
          this.cast = resp[0].data.cast.splice(0, 5);
          this.genres = resp[1].data.genres;
          this.showDetails = !this.showDetails;
        });
      }
    },
  },
};
</script>

<style lang="scss" scoped>
@import "../style/card_style.scss";
</style>
