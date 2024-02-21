<!-- src/components/MovieSearch.vue -->
<template>
  <v-container>
    <v-text-field
      v-model="searchQuery"
      label="Recherche"
      placeholder="Rechercher un film"
    ></v-text-field>
  </v-container>
</template>

<script>
export default {
  name: 'MovieSearch',
  data() {
    return {
      searchQuery: ''
    };
  },
  watch: {
    searchQuery(newVal) {
      this.$emit('searchMovieEmit', newVal);
    }
  }
}
</script>

<style scoped>
/* Styles spécifiques au composant MovieSearch */
</style>
