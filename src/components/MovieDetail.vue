<!-- src/components/MovieDetail.vue -->

<template>
  <v-dialog v-model="dialog" max-width="800px" persistent>
    <v-card>
      <v-card-title>
        {{ movieDetails.title }}
        <v-spacer></v-spacer>
        <v-btn icon @click="closeDetail">
          <v-icon>mdi-close</v-icon>
        </v-btn>
      </v-card-title>
      <v-card-text>
        <v-container>
          <v-row>
            <v-col cols="12" md="4">
              <v-img :src="getPosterUrl(movieDetails.poster_path)" :alt="movieDetails.title" />
            </v-col>
            <v-col cols="12" md="8">
              <v-row>
                <v-col cols="12">
                  <v-rating v-model="movieDetails.vote_average" precision="0.5" readonly></v-rating>
                </v-col>
                <v-col cols="12">
                  <v-chip v-if="movieDetails.release_date">{{ movieDetails.release_date }}</v-chip>
                </v-col>
                <v-col cols="12" v-if="movieDetails.genres && movieDetails.genres.length > 0">
                  <v-chip>{{ movieDetails.genres.join(', ') }}</v-chip>
                </v-col>
                <v-col cols="12" v-if="movieDetails.production_companies && movieDetails.production_companies.length > 0">
                  <v-chip>{{ movieDetails.production_companies.join(', ') }}</v-chip>
                </v-col>
              </v-row>
            </v-col>
          </v-row>
          <v-row>
            <v-col cols="12">
              <v-divider></v-divider>
            </v-col>
            <v-col cols="12">
              <v-subheader>Description</v-subheader>
              <p>{{ movieDetails.overview }}</p>
            </v-col>
          </v-row>
        </v-container>
      </v-card-text>
      <v-card-actions>
        <v-spacer></v-spacer>
        <v-btn @click="closeDetail">Close</v-btn>
      </v-card-actions>
    </v-card>
  </v-dialog>
</template>

<script>
import axios from 'axios';
import config from '@/config.json';

export default {
  name: 'MovieDetail',
  props: {
    movieId: {
      type: Number,
      required: true
    }
  },
  data() {
    return {
      dialog: true,
      movieDetails: {}
    };
  },
  created() {
    this.fetchMovieDetails();
  },
  methods: {
    fetchMovieDetails() {
      const apiKey = config.api_key;
      const movieDetailURL = `${config.api_urls.movie_detail}/${this.movieId}?api_key=${apiKey}`;

      axios.get(movieDetailURL)
        .then(response => {
          this.movieDetails = response.data;
        })
        .catch(error => {
          console.error('Error fetching movie details:', error);
        });
    },
    closeDetail() {
      this.$emit('close-detail');
    },
    getPosterUrl(posterPath) {
      if (posterPath) {
        return `${config.api_urls.photo_path}${posterPath}`;
      } else {
        return 'https://via.placeholder.com/400x600?text=No+Poster';
      }
    }
  }
};
</script>

<style scoped>
/* Styles spécifiques au composant MovieDetail */
</style>
