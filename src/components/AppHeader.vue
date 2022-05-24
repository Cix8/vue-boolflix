<template>
  <header>
    <div class="logo-container">
      <h1>BOOLFLIX</h1>
    </div>
    <div class="input-container">
      <div class="select-genre">
        <select
          name="genres"
          id="genres"
          v-model="selectedGenre"
          @change="$emit('getSelectedGenre', selectedGenre)"
        >
          <option value="">Tutti</option>
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
          v-model="thisFilm"
          @keyup.enter="$emit('getKeyword', thisFilm)"
        />
        <button id="search-btn" @click="$emit('getKeyword', thisFilm)">
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
      thisFilm: "",
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

  .logo-container {
    color: red;
  }

  .input-container {
    display: flex;

    .select-genre {
      margin-right: 10px;

      select {
        min-width: 100px;
      }
    }
  }
}
</style>
