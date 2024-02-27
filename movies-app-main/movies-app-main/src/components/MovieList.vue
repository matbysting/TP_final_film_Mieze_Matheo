<template>
    <div>
        <h1>Liste des Films</h1>
        <div v-if="filteredMovies" style="display: flex; flex-wrap: wrap;">
            <li v-for="movie in filteredMovies" :key="movie.id"
                style="width: 300px; height: auto; margin: 35px; list-style: none;">
                <strong>{{ movie.title }}</strong> - {{ movie.release_date }}
                <img :src="getImageUrl(movie.poster_path)" alt="Movie Poster" style="width: 300px; height: 400px;">
                <button @click="showMovieDetails(movie)">Voir les détails</button>
                <div v-if="selectedMovie && selectedMovie.id === movie.id">
                    <p>{{ movie.overview }}</p>
                    <a id="plus" :href="'https://www.themoviedb.org/movie/' + movie.id + '?language=fr-FR'">PLUS
                        D'INFORMATION ?</a>
                </div>
            </li>
        </div>
        <div v-if="error">
            <p style="color: rgb(225, 0, 255);">Une erreur s'est produite lors de la récupération des données.</p>
            <p>{{ error.message }}</p>
        </div>
        <MoviePagination :totalPages="totalPages" @pageChange="handlePageChange" />
    </div>
</template>
  
<script>
import axios from 'axios';
import MoviePagination from './MoviePagination.vue'; 
export default {
    name: 'MoviesList',
    components: {
        MoviePagination, // Déclarer le composant MoviePagination
    },
    props: ['movieSearch'], // Ajoutez une prop pour la valeur de recherche
    data() {
        return {
            moviesData: [], // Initialisez à un tableau vide
            error: null,
            baseImageUrl: 'https://image.tmdb.org/t/p/w500',
            selectedMovie: null,
            currentPage: 1,
            moviesPerPage: 10,
        };
    },

    created() {
        this.fetchMovies();
    },
    methods: {
        fetchMovies() {
            const api_key = '7185432bb4f3cd6339be3e7f9d3cd370';
            axios
                .get(`https://api.themoviedb.org/3/movie/popular?api_key=${api_key}&language=fr-FR&page=${this.currentPage}`)
                .then((res) => {
                    this.moviesData = res.data.results;
                })
                .catch((error) => {
                    this.error = error;
                });
        },
        getImageUrl(posterPath) {
            return this.baseImageUrl + posterPath;
        },
        showMovieDetails(movie) {
            // Émettre un événement avec l'ID du film sélectionné
            this.$emit('selected-movie', movie.id);
        },
        handlePageChange(pageNumber) {
            this.currentPage = pageNumber;
            this.fetchMovies();
        },
    },
    computed: {
        filteredMovies() {
            // Filtrer la liste des films en fonction de la valeur de recherche
            if (!this.moviesData || !this.movieSearch) {
                return this.moviesData;
            }
            const searchQuery = this.movieSearch.toLowerCase();
            return this.moviesData.filter(movie => {
                return movie.title.toLowerCase().includes(searchQuery);
            });
        },
        totalPages() {
            return Math.ceil(this.filteredMovies.length / this.moviesPerPage);
        },
    },
};
</script>
  
<style>

</style>
