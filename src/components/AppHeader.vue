<template>
  <header>
    <nav>
      <ul>
        <li class="logo-container">
          <h1>BOOLFLIX</h1>
        </li>
        <li
          v-for="(link, index) in navContents"
          :key="index"
          :class="{ active: activeLinkIndex === index }"
          @click="activeLinkIndex = index"
        >
          <a>{{ link }}</a>
        </li>
      </ul>
    </nav>
    <div class="input-container">
      <div class="select-genre">
        <select
          name="genres"
          id="genres"
          v-model="selectedGenre"
          @change="$emit('getSelectedGenre', selectedGenre)"
        >
          <option value="">Scegli un Genere</option>
          <option
            v-for="element in theseGenres"
            :key="element.id"
            :value="element.id"
          >
            {{ element.name }}
          </option>
        </select>
      </div>
      <div class="input-search">
        <input
          type="text"
          v-model="searchTheseCotents"
          @keyup.enter="
            searchTheseCotents.trim() !== ''
              ? ($emit('getKeyword', searchTheseCotents), (selectedGenre = ''))
              : (searchTheseCotents = '')
          "
          placeholder="Cerca film/serieTV"
        />
        <button
          id="search-btn"
          @click="
            searchTheseCotents.trim() !== ''
              ? ($emit('getKeyword', searchTheseCotents), (selectedGenre = ''))
              : (searchTheseCotents = '')
          "
        >
          <i class="fas fa-search"></i>
        </button>
      </div>
    </div>
  </header>
</template>

<script>
export default {
  name: "AppHeader",
  props: {
    theseGenres: Array,
  },
  data() {
    return {
      navContents: [
        "Home",
        "Serie TV",
        "Film",
        "Originali",
        "Aggiunti di recente",
        "La mia lista",
      ],
      activeLinkIndex: 0,
      searchTheseCotents: "",
      selectedGenre: "",
    };
  },
};
</script>

<style lang="scss" scoped>
header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 40px;

  nav {
    color: gray;

    ul {
      display: flex;
      align-items: center;

      li {
        margin: 0 6px;

        &.active {
          color: white;
        }

        a {
          cursor: pointer;
        }
      }

      .logo-container {
        font-size: 25px;
        color: red;
      }

      .logo-container + li {
        margin-left: 30px;
      }
    }
  }

  .input-container {
    display: flex;

    .select-genre {
      margin-right: 10px;

      select {
        min-width: 130px;
        height: 25px;
        padding: 0 2px;
      }
    }

    .input-search {
      input,
      button {
        height: 25px;
      }

      input {
        padding: 0 2px;
      }

      button {
        padding: 0 4px;
        cursor: pointer;
      }
    }
  }
}
</style>
