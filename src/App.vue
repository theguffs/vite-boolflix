<script>
import axios from 'axios';

export default {
  data() {
    return {
      query: '',        
      movies: [], // array dei  dei film

      tvShows: [], // Array delle serie TV
      languageFlags: {  
        en: 'https://flagcdn.com/w40/us.png',
        it: 'https://flagcdn.com/w40/it.png',
        fr: 'https://flagcdn.com/w40/fr.png',
        de: 'https://flagcdn.com/w40/de.png',
        es: 'https://flagcdn.com/w40/es.png',
        ar: '/img/imgyoussef.jpg',   // (immagine mia per la lingua araba)
      }
    };
  },

  methods: {
    async searchMovies() {
      const apiKey = '3a4db1cb907349cc970d7a7e93db75c6';
      const movieUrl = `https://api.themoviedb.org/3/search/movie?api_key=${apiKey}&query=${this.query}`;
      const tvUrl = `https://api.themoviedb.org/3/search/tv?api_key=${apiKey}&query=${this.query}`;

      // Richieste axioa per film e serie tv
      const movieResponse = await axios.get(movieUrl);
      const tvResponse = await axios.get(tvUrl);

      // Unisci i risultati
      this.movies = movieResponse.data.results;
      this.tvShows = tvResponse.data.results;
    },
    getLanguageFlag(languageCode) {
      return this.languageFlags[languageCode] || '/img/bandieracasual.png';  // bandiera messa a caso in caso se non c'è nell'array 
    }
  }
}

</script>

<template>
  <div id="app">
    <h1>Cerca Film e Serie TV</h1>
    
    <!-- barra di ricerca -->

    <div>
      <input v-model="query" type="text" placeholder="Inserisci il nome del film o della serie TV" />
      <button @click="searchMovies">Cerca</button>
    </div>
    
    <!-- risultati dei Film -->

    <div v-if="movies.length > 0">
      <h2>Film trovati:</h2>
      <ul>
        <li v-for="movie in movies" :key="movie.id">
          <h3>{{ movie.title }}</h3>
          <p><strong>Titolo Originale:</strong> {{ movie.original_title }}</p>
          <p><strong>Lingua:</strong> 
            <img :src="getLanguageFlag(movie.original_language)" alt="lingua" />
          </p>
          <p><strong>Voto:</strong> {{ movie.vote_average }}</p>
        </li>
      </ul>
    </div>

    <!-- risultati delle Serie TV -->
    <div v-if="tvShows.length > 0">
      <h2>Serie TV trovate:</h2>
      <ul>
        <li v-for="show in tvShows" :key="show.id">
          <h3>{{ show.name }}</h3>
          <p><strong>Titolo Originale:</strong> {{ show.original_name }}</p>
          <p><strong>Lingua:</strong> 
            <img :src="getLanguageFlag(show.original_language)" alt="lingua" />
          </p>
          <p><strong>Voto:</strong> {{ show.vote_average }}</p>
        </li>
      </ul>
    </div>

    <!-- messaggio per nessun risultato -->
    <div v-else>
      <p>Nessun film o serie TV trovata.</p>
    </div>  
  </div>
</template>

<style >

img{
  max-width: 50px;
  max-height: 50px;
}

</style>
