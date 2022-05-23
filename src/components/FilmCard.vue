<template>
  <li>
    <div class="title">
      <h2>Titolo: {{ thisElement.title }}</h2>
    </div>
    <div class="original-title">
      <h2>Titolo Originale: {{ thisElement.original_title }}</h2>
    </div>
    <div class="lang">
      <template v-if="langFlag(thisElement.original_language).length > 2">
        <div class="flag-container">
          <img
            :src="
              require('../assets/img/' +
                langFlag(thisElement.original_language) +
                '-flag.png')
            "
            alt=""
          />
        </div>
      </template>
      <template v-else>
        <span>Lingua: {{ thisElement.original_language.toUpperCase() }}</span>
      </template>
    </div>
    <div class="rating">
      <strong>Voto: {{ thisElement.popularity }}</strong>
    </div>
  </li>
</template>

<script>
export default {
  name: "FilmCard",
  props: {
    thisElement: Object,
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
