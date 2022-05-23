<template>
  <li>
    <div class="title">
      <h2>Titolo: {{ thisElement.title }}</h2>
    </div>
    <div class="original-title">
      <h2>Titolo Originale: {{ thisElement.original_title }}</h2>
    </div>
    <div class="lang">
      <p>Lingua: {{ thisElement.original_language.toUpperCase() }}</p>
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
};
</script>

<style lang="scss" scoped>
li {
  margin: 2rem 0;
}
</style>
