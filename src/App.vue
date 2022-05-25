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
import axios from "axios";

export default {
  name: "App",
  components: {
    AppHeader,
    AppContents,
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
      this.showsReady = false;
      this.filmsReady = false;
      const req1 = axios.get(
        `https://api.themoviedb.org/3/search/movie?api_key=5281cccae9a725e7baaa26749f7bb197&query=${keyWord}`
      );
      const req2 = axios.get(
        `https://api.themoviedb.org/3/search/tv?api_key=5281cccae9a725e7baaa26749f7bb197&query=${keyWord}`
      );
      const req3 = axios.get(
        "https://api.themoviedb.org/3/genre/movie/list?api_key=5281cccae9a725e7baaa26749f7bb197"
      );
      const req4 = axios.get(
        "https://api.themoviedb.org/3/genre/tv/list?api_key=5281cccae9a725e7baaa26749f7bb197"
      );
      axios.all([req1, req2, req3, req4]).then((resp) => {
        console.log(resp[0].data.results);
        this.mainFilms = resp[0].data.results;
        this.displayFilms = this.mainFilms;

        console.log(resp[1].data.results);
        this.mainTvShows = resp[1].data.results;
        this.displayTvShows = this.mainTvShows;

        const movieGenresArray = resp[2].data.genres;
        const movieGenresId = function () {
          const result = [];
          movieGenresArray.forEach((element) => {
            result.push(element.id);
          });
          return result;
        };
        const showGenresArray = resp[3].data.genres;
        const filteredShowGenres = showGenresArray.filter((element) => {
          const thisArray = movieGenresId();
          if (!thisArray.includes(element.id)) {
            return true;
          }
        });
        this.genresArray = movieGenresArray.concat(filteredShowGenres);

        this.showsReady = true;
        this.filmsReady = true;
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
  padding-bottom: 3rem;
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
