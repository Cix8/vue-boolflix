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
    <div
      class="info"
      :class="showInfo ? '' : 'd-none'"
      @click="findCastAndGenres(thisFilm.id)"
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
      <div class="rating" v-if="thisFilm.vote_average">
        <strong
          >Voto:
          <i
            class="fas fa-star"
            v-for="(star, index) in starRating(thisFilm.vote_average)"
            :key="index"
          ></i>
        </strong>
      </div>
      <div class="overview" v-if="thisFilm.overview">
        <p class="bold">
          Overview: <small>{{ thisFilm.overview }}</small>
        </p>
      </div>
    </div>
  </li>
</template>

<script>
import axios from "axios";

export default {
  name: "FilmCard",
  props: {
    thisFilm: Object,
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
      return thisArray;
    },

    findCastAndGenres(objId) {
      axios
        .get(
          `https://api.themoviedb.org/3/movie/${objId}/credits?api_key=5281cccae9a725e7baaa26749f7bb197`
        )
        .then((resp) => {
          this.cast = resp.data.cast.splice(0, 5);
        });
      axios
        .get(
          `https://api.themoviedb.org/3/movie/${objId}?api_key=5281cccae9a725e7baaa26749f7bb197`
        )
        .then((res) => {
          this.genres = res.data.genres;
          this.showDetails = !this.showDetails;
        });
    },
  },
};
</script>

<style lang="scss" scoped>
@import "../style/card_style.scss";
</style>
