<template>
  <div>
    <h1>Menu d'accueil</h1>
    <MovieSearch @searchMovieEmit="setMovieSearch" />
    <MoviesList :selectedMovieId="selectedMovieId" :movieSearch="movieSearch" @selected-movie="handleSelectedMovie" />
    <MovieDetail v-if="selectedMovieId" :movieId="selectedMovieId" @close-detail="selectedMovieId = null" />
    <MoviePagination :totalPages="totalPages" :currentPage="currentPage" @page-changed="handlePageChange" />
  </div>
</template>

<script>
import MoviesList from '../components/MovieList.vue';
import MovieDetail from '../components/MovieDetail.vue';
import MovieSearch from '../components/movieSearch.vue';
import MoviePagination from '../components/MoviePagination.vue'; // Assurez-vous d'importer le composant MoviePagination

export default {
  name: 'HomeView',
  components: {
    MoviesList,
    MovieDetail,
    MovieSearch,
    MoviePagination, // Assurez-vous que le composant MoviePagination est enregistré
  },
  data() {
    return {
      selectedMovieId: null,
      movieSearch: '',
      currentPage: 1,
      totalPages: 0,
    };
  },
  methods: {
    handleSelectedMovie(movieId) {
      this.selectedMovieId = movieId;
    },
    setMovieSearch(searchQuery) {
      this.movieSearch = searchQuery;
      this.currentPage = 1; // Réinitialiser la page actuelle lors de la recherche
    },
    handlePageChange(page) {
      this.currentPage = page;
    },
  },
};
</script>
