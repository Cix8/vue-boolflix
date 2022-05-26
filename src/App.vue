<template>
  <div id="app">
    <AppHeader
      @getKeyword="searchThis($event)"
      @getSelectedGenre="filterContents($event)"
      :theseGenres="genresArray"
    />
    <main>
      <section class="movies" v-if="moviesReady">
        <div class="section-title">
          <h2>Boolflix Film</h2>
        </div>
        <AppContents :contentsArray="displayMovies" />
      </section>
      <section class="tv-shows" v-if="showsReady">
        <div class="section-title">
          <h2>Boolflix Serie TV</h2>
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
      mainMovies: [],
      displayMovies: [],
      mainTvShows: [],
      displayTvShows: [],
      moviesReady: false,
      showsReady: false,
    };
  },
  created() {
    const req1 = axios.get(
      "https://api.themoviedb.org/3/genre/movie/list?api_key=5281cccae9a725e7baaa26749f7bb197"
    );
    const req2 = axios.get(
      "https://api.themoviedb.org/3/genre/tv/list?api_key=5281cccae9a725e7baaa26749f7bb197"
    );
    axios.all([req1, req2]).then((resp) => {
      const movieGenresArray = resp[0].data.genres;
      const movieGenresId = function () {
        const result = [];
        movieGenresArray.forEach((element) => {
          result.push(element.id);
        });
        return result;
      };
      const showGenresArray = resp[1].data.genres;
      const filteredShowGenres = showGenresArray.filter((element) => {
        const thisArray = movieGenresId();
        if (!thisArray.includes(element.id)) {
          return true;
        }
      });
      this.genresArray = movieGenresArray.concat(filteredShowGenres);
    });
  },
  methods: {
    searchThis(keyWord) {
      this.showsReady = false;
      this.moviesReady = false;
      const req1 = axios.get(
        `https://api.themoviedb.org/3/search/movie?api_key=5281cccae9a725e7baaa26749f7bb197&query=${keyWord}`
      );
      const req2 = axios.get(
        `https://api.themoviedb.org/3/search/tv?api_key=5281cccae9a725e7baaa26749f7bb197&query=${keyWord}`
      );

      axios.all([req1, req2]).then((resp) => {
        console.log(resp[0].data.results);
        this.mainMovies = resp[0].data.results;
        this.displayMovies = this.mainMovies;

        console.log(resp[1].data.results);
        this.mainTvShows = resp[1].data.results;
        this.displayTvShows = this.mainTvShows;

        this.showsReady = true;
        this.moviesReady = true;
      });
    },

    filterContents(keyId) {
      if (keyId !== "") {
        this.displayMovies = this.mainMovies.filter((element) => {
          if (element.genre_ids.length > 0) {
            for (let i = 0; i < element.genre_ids.length; i++) {
              if (element.genre_ids[i] == keyId) {
                return true;
              }
            }
          }
        });
      } else {
        this.displayMovies = this.mainMovies;
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
  min-width: 1150px;
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
