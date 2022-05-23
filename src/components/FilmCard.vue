<template>
  <li @mouseover="showInfo = true" @mouseleave="showInfo = false">
    <div class="img-container">
      <img
        :src="'https://image.tmdb.org/t/p/w342' + thisFilm.backdrop_path"
        alt=""
      />
    </div>
    <div class="info" :class="showInfo ? '' : 'd-none'">
      <div class="title">
        <h2>Titolo: {{ thisFilm.title }}</h2>
      </div>
      <div class="original-title">
        <h2>Titolo Originale: {{ thisFilm.original_title }}</h2>
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
          <span>Lingua: {{ thisFilm.original_language.toUpperCase() }}</span>
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
li {
  position: relative;
  width: 342px;
  height: 450px;
  margin: 2rem;
  cursor: pointer;

  .info {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 98%;
    display: flex;
    flex-direction: column;
    justify-content: flex-end;
    background-color: rgba(0, 0, 0, 0.4);
    color: white;

    .lang {
      .flag-container {
        width: 30px;
        height: 30px;
        margin: 1rem 0;
      }
    }

    .rating {
      i {
        margin: 0 2px;
        color: yellow;
      }
    }
  }
}
</style>
