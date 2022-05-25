<template>
  <div id="app">
    <AppHeader
      @getKeyword="searchThis($event)"
      @getSelectedGenre="filterContents($event)"
      :theseGenres="genresArray"
    />
    <main>
      <section class="films" v-if="filmsReady">
        <div class="section-title">
          <h2>Boolflix Films</h2>
        </div>
        <AppContents :contentsArray="displayFilms" />
      </section>
      <section class="tv-shows" v-if="showsReady">
        <div class="section-title">
          <h2>Boolflix Tv shows</h2>
        </div>
        <AppContents :contentsArray="displayTvShows" />
      </section>
    </main>
  </div>
</template>

<script>
import AppHeader from "./components/AppHeader.vue";
import AppContents from "./components/AppContents.vue";
// import AppTvShows from "./components/AppTvShows.vue";
import axios from "axios";

export default {
  name: "App",
  components: {
    AppHeader,
    AppContents,
    // AppTvShows,
  },
  data() {
    return {
      genresArray: [],
      mainFilms: [],
      displayFilms: [],
      mainTvShows: [],
      displayTvShows: [],
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
          this.displayFilms = this.mainFilms;
          this.filmsReady = true;
        });
      axios
        .get(
          `https://api.themoviedb.org/3/search/tv?api_key=5281cccae9a725e7baaa26749f7bb197&query=${keyWord}`
        )
        .then((res) => {
          console.log(res.data.results);
          this.mainTvShows = res.data.results;
          this.displayTvShows = this.mainTvShows;
          this.showsReady = true;
        });
      axios
        .get(
          "https://api.themoviedb.org/3/genre/movie/list?api_key=5281cccae9a725e7baaa26749f7bb197"
        )
        .then((resp) => {
          this.genresArray = resp.data.genres;
        });
    },

    filterContents(keyId) {
      if (keyId !== "") {
        this.displayFilms = this.mainFilms.filter((element) => {
          if (element.genre_ids.length > 0) {
            for (let i = 0; i < element.genre_ids.length; i++) {
              if (element.genre_ids[i] == keyId) {
                return true;
              }
            }
          }
        });
      } else {
        this.displayFilms = this.mainFilms;
      }

      if (keyId !== "") {
        this.displayTvShows = this.mainTvShows.filter((element) => {
          if (element.genre_ids.length > 0) {
            for (let i = 0; i < element.genre_ids.length; i++) {
              if (element.genre_ids[i] == keyId) {
                return true;
              }
            }
          }
        });
      } else {
        this.displayTvShows = this.mainTvShows;
      }
    },
  },
};
</script>

<style lang="scss">
@import "./style/common.scss";

#app {
  min-height: 100vh;
  background-image: linear-gradient(rgb(0, 0, 0), rgba(0, 0, 0, 0.8));

  section {
    width: 90%;
    margin: 0 auto;

    .section-title {
      padding-top: 3rem;
      font-size: 2rem;
      font-weight: bold;
      color: white;
    }
  }
}
</style>
