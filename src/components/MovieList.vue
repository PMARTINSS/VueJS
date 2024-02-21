<!-- src/components/MovieList.vue -->
<template>
  <v-container>
    <v-row>
      <v-col v-for="movie in movies" :key="movie.id" cols="12" sm="6" md="4" lg="3">
        <v-card>
          <v-img :src="getPosterUrl(movie.poster_path)" :alt="movie.title"></v-img>
          <v-card-title>{{ movie.title }}</v-card-title>
          <!-- Bouton pour afficher les détails -->
          <v-btn @click="showMovieDetails(movie)">Afficher les détails</v-btn>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import axios from 'axios';
import config from '@/config.json';

export default {
  name: 'MovieList',
  data() {
    return {
      movies: []
    };
  },
  created() {
    this.fetchMovies();
  },
  methods: {
    fetchMovies() {
      const apiKey = config.api_key;
      const language = config.language;
      const movieListURL = `${config.api_urls.movie_list}?api_key=${apiKey}&language=${language}`;

      axios.get(movieListURL)
        .then(response => {
          this.movies = response.data.results;
        })
        .catch(error => {
          console.error('Error fetching movie list:', error);
        });
    },
    showMovieDetails(movie) {
      // Émettre un événement pour afficher les détails du film
      this.$emit('showMovieDetailEmit', movie);
    },
    getPosterUrl(posterPath) {
      if (posterPath) {
        return `${config.api_urls.photo_path}${posterPath}`;
      } else {
        return 'https://via.placeholder.com/185x278?text=No+Poster';
      }
    }
  }
}
</script>

<style scoped>
/* Styles spécifiques au composant MovieList */
</style>
