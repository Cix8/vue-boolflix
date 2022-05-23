<template>
  <ul>
    <FilmCard
      v-for="(element, index) in filmsArray"
      :key="index"
      :thisFilm="element"
    />
  </ul>
</template>

<script>
import FilmCard from "./FilmCard.vue";

export default {
  name: "AppFilms",
  props: {
    filmsArray: Array,
  },
  components: {
    FilmCard,
  },
};
</script>

<style lang="scss" scoped></style>
