<template>
    <div>
        <input type="text" v-model="searchQuery" placeholder="Rechercher un film..." />
    </div>
</template>
  
<script>
import { debounce } from 'lodash'; 

export default {
    name: 'MovieSearch',
    data() {
        return {
            searchQuery: '',
        };
    },
    watch: {
        searchQuery: debounce(function (newVal) {
            this.$emit('searchMovieEmit', newVal);
        }, 300),
    },
};
</script>
  