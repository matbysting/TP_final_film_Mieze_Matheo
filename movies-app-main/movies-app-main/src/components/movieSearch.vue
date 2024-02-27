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
            this.$emit('searchMovieEmit', newVal); 
        }, 300), 
    },
};
</script>
  