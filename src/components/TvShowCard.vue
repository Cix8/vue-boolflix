<template>
  <li>
    <div class="title">
      <h2>Titolo: {{ thisShow.name }}</h2>
    </div>
    <div class="original-title">
      <h2>Titolo Originale: {{ thisShow.original_name }}</h2>
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
        <span>Lingua: {{ thisShow.original_language.toUpperCase() }}</span>
      </template>
    </div>
    <div class="rating">
      <strong>Voto: {{ thisShow.vote_average }}</strong>
    </div>
  </li>
</template>

<script>
export default {
  name: "TvShowCard",
  props: {
    thisShow: Object,
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
  },
};
</script>

<style lang="scss" scoped>
li {
  margin: 2rem 0;

  .lang {
    .flag-container {
      width: 30px;
      height: 30px;
      border-radius: 50%;
      overflow: hidden;
      margin: 1rem 0;
    }
  }
}
</style>
