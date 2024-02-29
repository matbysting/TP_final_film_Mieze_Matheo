<template>
    <div>
        <h1>Liste des Films</h1>
        <div v-if="filteredMovies" class="movies-list-container">
            <div v-for="movie in filteredMovies" :key="movie.id" class="movie-container">
                <strong class="movie-title">{{ movie.title }}</strong>
                <span class="release-date">{{ movie.release_date }}</span> <!-- Placer la date ici -->
                <img :src="getImageUrl(movie.poster_path)" alt="Movie Poster" class="movie-poster">
                <button @click="showMovieDetails(movie)" class="details-button">Voir les détails</button>
                <div v-if="selectedMovie && selectedMovie.id === movie.id" class="movie-details">
                    <p class="movie-overview">{{ movie.overview }}</p>
                    <a :href="'https://www.themoviedb.org/movie/' + movie.id + '?language=fr-FR'"
                        class="more-info-link">PLUS D'INFORMATION ?</a>
                </div>
            </div>
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
        MoviePagination,
    },
    props: ['movieSearch'],
    data() {
        return {
            moviesData: [],
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
            const api_key = 'Votre_API_KEY';
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

            this.$emit('selected-movie', movie.id);
        },
        handlePageChange(pageNumber) {
            this.currentPage = pageNumber;
            this.fetchMovies();
        },
    },
    computed: {
        filteredMovies() {

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
li {
    margin-right: 15px;
    margin-bottom: 15px;

}


button {
    background-color: transparent;

    color: blue;

    border: 1px solid blue;

    padding: 5px 10px;

    cursor: pointer;
}


button:hover {
    background-color: blue;

    color: white;

}

img {
    margin-bottom: 15px;

    height: 400px;

}


.movies-list-container {
    display: flex;
    flex-wrap: wrap;
    justify-content: space-between;
}

.movie-container {
    width: calc(33.33% - 30px);
    margin-bottom: 30px;
    background-color: gainsboro;
    border-radius: 10px;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
    padding: 15px;
    text-align: center;
}

.movie-title {
    display: block;
    font-weight: bold;
}

.release-date {
    display: block;
    margin-top: 5px;
}

.movie-poster {
    margin-bottom: 15px;
    height: 400px;
}

.details-button {
    background-color: transparent;
    color: blue;
    border: 1px solid blue;
    padding: 5px 10px;
    cursor: pointer;
    margin-top: 10px;
}

.details-button:hover {
    background-color: blue;
    color: white;
}

.movie-details {
    margin-top: 15px;
}

.more-info-link {
    color: blue;
    text-decoration: none;
}

.more-info-link:hover {
    text-decoration: underline;
}
</style>
