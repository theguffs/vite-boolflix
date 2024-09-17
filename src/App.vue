<script>
import axios from 'axios';

export default {
  data() {
    return {
      query: '',        
      movies: [], // array dei  dei film
    };
  },
  methods: {
  async searchMovies() {
    const apiKey = '3a4db1cb907349cc970d7a7e93db75c6';
    const url = `https://api.themoviedb.org/3/search/movie?api_key=${apiKey}&query=${this.query}`;

    const response = await axios.get(url);
    this.movies = response.data.results;
    this.querySearched = true;
  }
}
  }
</script>

<template>
  <div id="app">
    <h1>Cerca un Film</h1>
    
    <!-- barra di ricerca -->

    <div>
      <input v-model="query" type="text" placeholder="Inserisci il nome del film" />
      <button @click="searchMovies">Cerca</button>
    </div>
    
    <!-- risultati -->

    <div v-if="movies.length > 0">
      <h2>Risultati della ricerca:</h2>
      <ul>
        <li v-for="movie in movies" :key="movie.id">
          <h3>{{ movie.title }}</h3>
          <p><strong>Titolo Originale:</strong> {{ movie.original_title }}</p>
          <p><strong>Lingua:</strong> {{ movie.original_language }}</p>
          <p><strong>Voto:</strong> {{ movie.vote_average }}</p>
        </li>
      </ul>
    </div>
    
  </div>
</template>

<style scoped>

</style>
