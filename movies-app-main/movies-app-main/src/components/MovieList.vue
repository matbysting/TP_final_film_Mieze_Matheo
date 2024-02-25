<template>
    <div>
        <h1>Liste des Films</h1>
        <div v-if="moviesData" style="display: flex; flex-wrap: wrap;">
            <li v-for="movie in moviesData" :key="movie.id"
                style="width: 300px; height: auto; margin: 35px; list-style: none;">
                <strong>{{ movie.title }}</strong> - {{ movie.release_date }}
                <img :src="getImageUrl(movie.poster_path)" alt="Movie Poster" style="width: 300px; height: 400px;">
                <button @click="showMovieDetails(movie)">Voir les détails</button>
                <div v-if="selectedMovie && selectedMovie.id === movie.id">
                    <p>{{ movie.overview }}</p>
                    <a id="plus" :href="'https://www.themoviedb.org/movie/' + movie.id + '?language=fr-FR'">PLUS D'INFORMATION ?</a>
                </div>
            </li>
        </div>
        <div v-if="error">
            <p style="color: rgb(225, 0, 255);">Une erreur s'est produite lors de la récupération des données.</p>
            <p>{{ error.message }}</p>
        </div>
    </div>
</template>

<script>
import axios from 'axios';

export default {
    name: 'MoviesList',
    data() {
        return {
            moviesData: null,
            error: null,
            baseImageUrl: 'https://image.tmdb.org/t/p/w500',
            selectedMovie: null,
        };
    },
    created() {
        const api_key = '7185432bb4f3cd6339be3e7f9d3cd370';

        axios
            .get(`https://api.themoviedb.org/3/movie/popular?api_key=${api_key}&language=fr-FR`)
            .then((res) => {
                this.moviesData = res.data.results;
            })
            .catch((error) => {
                this.error = error;
            });
    },
    methods: {
        getImageUrl(posterPath) {
            return this.baseImageUrl + posterPath;
        },
        showMovieDetails(movie) {
            this.selectedMovie = this.selectedMovie === movie ? null : movie;
        },
    },
};
</script>

<style>
/* Ajouter une marge entre chaque film */
li {
    margin-right: 15px;
    margin-bottom: 15px;
    /* Ajout de marge en bas pour l'espace entre les lignes */
}

/* Styliser le bouton "Voir les détails" */
button {
    background-color: transparent; /* Fond transparent */
    color: blue; /* Couleur du texte bleue */
    border: 1px solid blue; /* Bordure bleue */
    padding: 5px 10px; /* Padding autour du texte */
    cursor: pointer;
}

/* Styliser le contour du bouton "Voir les détails" */
button:hover {
    background-color: blue; /* Fond bleu au survol */
    color: white; /* Texte blanc au survol */
}

/* Ajouter une marge autour de l'image */
img {
    margin-bottom: 15px; /* Ajout de marge en bas de l'image */
    height: 400px; /* Définir une hauteur fixe pour toutes les images */
}
</style>
