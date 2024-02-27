<template>
    <div>
        <input type="text" v-model="searchQuery" placeholder="Rechercher un film..." />
    </div>
</template>
  
<script>
import { debounce } from 'lodash'; // Importer la fonction debounce de lodash

export default {
    name: 'MovieSearch',
    data() {
        return {
            searchQuery: '', // Stocker la valeur de la recherche dans une data
        };
    },
    watch: {
        searchQuery: debounce(function (newVal) {
            // Utiliser debounce pour déclencher l'événement avec un léger délai après la saisie de l'utilisateur
            this.$emit('searchMovieEmit', newVal); // Émettre un événement searchMovieEmit avec la valeur de recherche
        }, 300), // Délai de 300 millisecondes après la fin de la saisie
    },
};
</script>
  