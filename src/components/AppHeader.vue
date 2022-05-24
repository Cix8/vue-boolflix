<template>
  <header>
    <div class="logo-container">
      <h1>BOOLFLIX</h1>
    </div>
    <div class="input-container">
      <input
        type="text"
        v-model="thisFilm"
        @keyup.enter="$emit('getKeyword', thisFilm)"
      />
      <button id="search-btn" @click="$emit('getKeyword', thisFilm)">
        <i class="fas fa-search"></i>
      </button>
    </div>
  </header>
</template>

<script>
export default {
  name: "AppHeader",
  data() {
    return {
      thisFilm: "",
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
}
</style>
