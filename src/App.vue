<template>
  <div id="app">
    <AppHeader @getKeyword="searchThis($event)" />
    <main>
      <ul>
        <FilmCard
          v-for="(element, index) in mainData"
          :key="index"
          :thisElement="element"
        />
      </ul>
    </main>
  </div>
</template>

<script>
import AppHeader from "./components/AppHeader.vue";
import FilmCard from "./components/FilmCard.vue";
import axios from "axios";

export default {
  name: "App",
  components: {
    AppHeader,
    FilmCard,
  },
  data() {
    return {
      mainData: [],
    };
  },
  methods: {
    searchThis(keyWord) {
      axios
        .get(
          `https://api.themoviedb.org/3/search/movie?api_key=e99307154c6dfb0b4750f6603256716d&query=${keyWord}`
        )
        .then((resp) => {
          console.log(resp.data.results);
          this.mainData = resp.data.results;
        });
    },
  },
};
</script>

<style lang="scss">
@import "./style/common.scss";
</style>
