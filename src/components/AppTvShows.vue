<template>
  <ul>
    <TvShowCard
      v-for="(item, index) in showsArray"
      :key="index"
      :thisShow="item"
    />
  </ul>
</template>

<script>
import TvShowCard from "./TvShowCard.vue";

export default {
  name: "AppTvShows",
  components: {
    TvShowCard,
  },
  props: {
    showsArray: Array,
  },
};
</script>

<style lang="scss" scoped>
ul {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
}
</style>
