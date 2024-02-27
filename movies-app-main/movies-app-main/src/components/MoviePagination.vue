<template>
    <div>
      
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
        currentPage: 1, 
      };
    },
    methods: {
      changePage(pageNumber) {
        this.currentPage = pageNumber;
       
        this.$emit('page-change', pageNumber);
      },
    },
  };
  </script>
  