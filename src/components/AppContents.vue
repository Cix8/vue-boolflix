<template>
  <div class="list-container" v-if="contentsArray.length !== 0">
    <div class="title">
      <h2>
        {{ contentsArray[0].title ? "Boolflix Film" : "Boolflix Serie TV" }}
      </h2>
    </div>
    <ul>
      <ContentCard
        v-for="(element, index) in contentsArray"
        :key="index"
        :thisContent="element"
      />
    </ul>
  </div>
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
.list-container {
  .title {
    padding-top: 3rem;
    font-size: 2rem;
    font-weight: bold;
    color: white;
  }

  ul {
    display: flex;
    flex-wrap: wrap;
  }
}
</style>
