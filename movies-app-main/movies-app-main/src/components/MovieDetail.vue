<template>
    <div v-if="detailsFilm" class="movie-details">
      <v-dialog v-model="dialog" max-width="1200"> 
        <v-card>
          <v-img :src="getImageUrl(detailsFilm.poster_path)" aspect-ratio="2.5" class="poster"></v-img> 
          <v-card-text>
            <div class="title">{{ detailsFilm.title }}</div>
            <br> 
            <div class="date">{{ detailsFilm.release_date }}</div>
            <br>
            <div class="genres">
                Genres:
                <template v-for="(genre, index) in detailsFilm.genres" :key="index">
                  <span class="genre">{{ genre.name }}</span>
                  <span v-if="index !== detailsFilm.genres.length - 1">, </span>
                </template>
              </div>
            <br> 
            <div class="description">{{ detailsFilm.overview }}</div>
            <br> 
            <div class="note">Note: {{ detailsFilm.vote_average }}</div>
            <br> 
            <div class="production">
              <br> 
              <div class="director">Réalisateur: {{ getDirector(detailsFilm.credits.crew) }}</div>
              <br> 
              <div class="production-companies">
                <div v-for="company in detailsFilm.production_companies" :key="company.id" class="production-company">
                  <img :src="getProductionCompanyLogoUrl(company.logo_path)" :alt="company.name" class="production-company-logo">
                </div>
              </div>
            </div>
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
        dialog: true,
        baseUrlImage: "https://image.tmdb.org/t/p/w500",
      };
    },
    created() {
      this.fetchMovieDetails();
    },
    methods: {
      fetchMovieDetails() {
        const apiKey = "7185432bb4f3cd6339be3e7f9d3cd370";
        axios
          .get(
            `https://api.themoviedb.org/3/movie/${this.movieId}?api_key=${apiKey}&language=fr-FR&append_to_response=credits`
          )
          .then((res) => {
            this.detailsFilm = res.data;
          })
          .catch((error) => {
            console.error("Erreur lors de la récupération des détails du film : ", error);
          });
      },
      closeDetail() {
        this.$emit("close-detail");
      },
      getImageUrl(posterPath) {
        return this.baseUrlImage + posterPath;
      },
      getDirector(crew) {
        const director = crew.find(member => member.job === 'Director');
        return director ? director.name : 'Inconnu';
      },
      getProductionCompanyLogoUrl(logoPath) {
        return logoPath ? `https://image.tmdb.org/t/p/w200${logoPath}` : '';
      },
    },
  };
  </script>
  
  <style scoped>
  .movie-details {
    text-align: center;
  }
  
  .poster {
    width: 100%; 
  }
  
  .title {
    font-size: 28px; 
    font-weight: bold;
  }
  
  .date {
    font-size: 18px;
  }
  
  .description {
    margin-top: 10px;
  }
  
  .note {
    margin-top: 10px;
  }
  
  .director {
    margin-top: 10px;
  }
  
  .genres {
    margin-top: 10px;
  }
  
  .genre {
    padding: 5px 10px; 
    border-radius: 20px; 
    background-color: green; 
    margin-right: 5px; 
  }

  .note{
    padding: 5px 10px; 
    border-radius: 20px; 
    background-color: #f0f0f0; 
    margin-right: 5px; 
  }
  
  .production-companies {
    margin-top: 10px;
    display: flex; 
  }
  
  .production-company {
    margin-right: 10px;
  }
  
  .production-company-logo {
    width: 100px; 
    height: auto;
  }
  </style>
  