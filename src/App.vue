<template>
  <div id="app">
    <AppHeader @getKeyword="searchThis($event)" />
    <main>
      <section class="films" v-if="filmsReady">
        <div class="section-title">
          <h2>FILMS</h2>
        </div>
        <AppFilms :filmsArray="mainFilms" />
      </section>
      <section class="tv-shows" v-if="showsReady">
        <div class="section-title">
          <h2>TV SHOWS</h2>
        </div>
        <AppTvShows :showsArray="mainTvShows" />
      </section>
    </main>
  </div>
</template>

<script>
import AppHeader from "./components/AppHeader.vue";
import AppFilms from "./components/AppFilms.vue";
import AppTvShows from "./components/AppTvShows.vue";
import axios from "axios";

export default {
  name: "App",
  components: {
    AppHeader,
    AppFilms,
    AppTvShows,
  },
  data() {
    return {
      mainFilms: [],
      mainTvShows: [],
      filmsReady: false,
      showsReady: false,
    };
  },
  methods: {
    searchThis(keyWord) {
      axios
        .get(
          `https://api.themoviedb.org/3/search/movie?api_key=5281cccae9a725e7baaa26749f7bb197&query=${keyWord}`
        )
        .then((resp) => {
          console.log(resp.data.results);
          this.mainFilms = resp.data.results;
          this.filmsReady = true;
        });
      axios
        .get(
          `https://api.themoviedb.org/3/search/tv?api_key=5281cccae9a725e7baaa26749f7bb197&query=${keyWord}`
        )
        .then((res) => {
          console.log(res.data.results);
          this.mainTvShows = res.data.results;
          this.showsReady = true;
        });
    },
  },
};
</script>

<style lang="scss">
@import "./style/common.scss";

.section-title {
  padding-top: 3rem;
  font-size: 3rem;
  font-weight: bold;
  color: red;
  text-align: center;
}
</style>
