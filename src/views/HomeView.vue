<!-- src/views/HomeView.vue -->
<template>
  <v-container>
    <movie-search @searchMovieEmit="setMovieSearch"></movie-search>
    <movie-list @showMovieDetailEmit="showMovieDetail"></movie-list>
    <movie-detail v-if="selectedMovie" v-bind:movie-id="selectedMovie.id" @close-detail="closeMovieDetail"></movie-detail>
    <movie-pagination></movie-pagination>
  </v-container>
</template>

<script>
import MovieList from '../components/MovieList.vue';
import MovieDetail from '../components/MovieDetail.vue';
import MovieSearch from '../components/MovieSearch.vue';
import MoviePagination from '../components/MoviePagination.vue';
import config from '@/config.json';
import axios from 'axios';
import _ from 'lodash';

export default {
  name: 'HomeView',
  components: {
    MovieList,
    MovieDetail,
    MovieSearch,
    MoviePagination
  },
  data() {
    return {
      selectedMovie: null,
      movieSearch: '',
      movies: []
    };
  },
  computed: {
    movieFiltered() {
      if (!this.movieSearch) {
        return this.movies;
      } else {
        return _.filter(this.movies, movie =>
          movie.title.toLowerCase().includes(this.movieSearch.toLowerCase())
        );
      }
    }
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
    showMovieDetail(movie) {
      this.selectedMovie = movie;
    },
    closeMovieDetail() {
      this.selectedMovie = null;
    },
    setMovieSearch(searchQuery) {
      this.movieSearch = searchQuery;
    }
  }
}
</script>

<style scoped>
/* Styles spécifiques au composant HomeView */
</style>
