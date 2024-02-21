<!-- src/components/MoviePagination.vue -->
<template>
    <v-pagination v-model="page" :length="totalPages" @input="changePage"></v-pagination>
  </template>
  
  <script>
  export default {
    name: 'MoviePagination',
    data() {
      return {
        page: 1,
        totalPages: 5 // Remplacez par la valeur réelle
      };
    },
    methods: {
      changePage(newPage) {
        // Mettez à jour la liste des films avec la nouvelle page
      }
    }
    // Autres options du composant
  }
  </script>
  
  <style scoped>
  /* Styles spécifiques au composant MoviePagination */
  </style>
  