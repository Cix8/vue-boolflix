<template>
  <ul>
    <ContentCard
      v-for="(element, index) in contentsArray"
      :key="index"
      :thisContent="element"
    />
  </ul>
</template>

<script>
import ContentCard from "./ContentCard.vue";

export default {
  name: "AppContents",
  props: {
    contentsArray: Array,
  },
  components: {
    ContentCard,
  },
};
</script>

<style lang="scss" scoped>
ul {
  display: flex;
  flex-wrap: wrap;
}
</style>
