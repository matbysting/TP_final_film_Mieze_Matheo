<template>
    <div>
      <!-- Afficher les boutons de pagination -->
      <button v-for="pageNumber in totalPages" :key="pageNumber" @click="changePage(pageNumber)">
        {{ pageNumber }}
      </button>
    </div>
  </template>
  
  <script>
  export default {
    name: 'MoviePagination',
    props: {
      totalPages: {
        type: Number,
        required: true,
      },
    },
    data() {
      return {
        currentPage: 1, // Initialisez la page actuelle à 1 par défaut
      };
    },
    methods: {
      changePage(pageNumber) {
        this.currentPage = pageNumber;
        // Émettre un événement pour informer le composant parent du changement de page
        this.$emit('page-change', pageNumber);
      },
    },
  };
  </script>
  