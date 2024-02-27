<template>
    <div v-if="detailsFilm" class="movie-details">
        <v-dialog v-model="dialog" max-width="500">
            <v-card>
                <v-img :src="getImageUrl(detailsFilm.poster_path)" aspect-ratio="2.5"></v-img>
                <v-card-text>
                    <div class="title">{{ detailsFilm.title }}</div>
                    <div>{{ detailsFilm.release_date }}</div>
                    <div>{{ detailsFilm.overview }}</div>

                </v-card-text>
                <v-card-actions>
                    <v-btn color="blue darken-1" text @click="closeDetail">Fermer</v-btn>

                </v-card-actions>
            </v-card>
        </v-dialog>
    </div>
</template>
  
<script>
import axios from 'axios';

export default {
    name: "MovieDetail",
    props: {
        movieId: {
            type: Number,
            required: true,
        },
    },
    data() {
        return {
            detailsFilm: null,
            dialog: true, // pour contrôler la visibilité de la boîte de dialogue
            baseUrlImage: "https://image.tmdb.org/t/p/w500",
        };
    },
    created() {
        // Récupérer les détails du film en utilisant l'idFilm
        this.fetchMovieDetails();
    },
    methods: {
        fetchMovieDetails() {
            const cleApi = "7185432bb4f3cd6339be3e7f9d3cd370";
            axios
                .get(
                    `https://api.themoviedb.org/3/movie/${this.movieId}?api_key=${cleApi}&language=fr-FR`
                )
                .then((res) => {
                    this.detailsFilm = res.data;
                })
                .catch((erreur) => {
                    console.error("Erreur lors de la récupération des détails du film : ", erreur);
                });
        },
        closeDetail() {
            this.$emit("close-detail");
        },
        getImageUrl(cheminAffiche) {
            return this.baseUrlImage + cheminAffiche;
        },
    },
};
</script>
  
<style scoped>
.movie-details {
    text-align: center;
}
</style>
