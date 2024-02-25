<template>
  <div>
    <h1>Menu d'accueil</h1>
    <MoviesList />
  </div>
</template>

<script>
import MoviesList from '../components/MovieList.vue';
import MoviesDetail from '../components/MovieList.vue'; 
import MoviePagination from '../components/MovieList.vue'; 


export default {
  name: 'HomeView',
  components: {
    MoviesList,
    MoviesDetail,
    MoviePagination,
  },
};


</script>