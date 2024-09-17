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
      },
      imageBaseUrl: 'https://image.tmdb.org/t/p/', // URL base per le immagini
      imageSize: 'w342' // Dimensione dell'immagine scelta
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
    },
    getPosterUrl(posterPath) {
      return `${this.imageBaseUrl}${this.imageSize}${posterPath}`; // Costruisce l'URL completo del poster
    },
    // Arrotonda il voto da 1 a 10 in un numero da 1 a 5

    // potevo usare anche: 
    // return Math.ceil(voteAverage / 2);

    getStarRating(voteAverage) {
      // Divide il voto per 2 e prende solo la parte intera
      return parseInt(voteAverage / 2);
    }
  }
}

</script>

<template>
  <div id="app">
    <h1>Cerca Film e Serie TV</h1>
    
    <!-- barra di ricerca -->

    <div>
      <input v-model="query" type="text" @keyup.enter="searchMovies"  placeholder="Inserisci il nome del film o della serie TV" />
      <button @click="searchMovies" >Cerca</button>
    </div>
    
    <!-- risultati dei Film -->

    <div v-if="movies.length > 0">
      <h2>Film trovati:</h2>
      <ul>
        <li v-for="movie in movies" :key="movie.id">
          <h3>{{ movie.title }}</h3>
          <p><strong>Titolo Originale:</strong> {{ movie.original_title }}</p>
          <p><strong>Lingua:</strong> 
            <img :src="getLanguageFlag(movie.original_language)" alt="lingua" style="width: 30px; height: 20px;"/>
          </p>
          <p><strong>Voto:</strong></p> 

          <!-- stelle -->
          <span v-for="star in getStarRating(movie.vote_average)" :key="star" class="star filled">★</span>
          <span v-for="emptyStar in 5 - getStarRating(movie.vote_average)" :key="emptyStar" class="star empty">★</span>
          
          <!-- mostra la copertina del film -->
            <div>
              <img class="copertina" v-if="movie.poster_path" :src="getPosterUrl(movie.poster_path)" alt="Poster del film" />
              <p v-else>Nessuna immagine disponibile</p>
            </div>


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
            <img :src="getLanguageFlag(show.original_language)" alt="lingua" style="width: 30px; height: 20px;" />
          </p>
          <p><strong>Voto:</strong></p>

          <!-- stelle -->
          <span v-for="star in getStarRating(show.vote_average)" :key="star" class="star filled">★</span>
          <span v-for="emptyStar in 5 - getStarRating(show.vote_average)" :key="emptyStar" class="star empty">★</span>
          
          <!-- visualizza la copertina della serie TV -->
          <div>
            <img class="copertina" v-if="show.poster_path" :src="getPosterUrl(show.poster_path)" alt="Poster della serie TV" />
            <p v-else>Nessuna immagine disponibile</p>
          </div>
          
          
        </li>
      </ul>
    </div>

    <!-- messaggio per nessun risultato -->
    <div v-else>
      <p>Nessun film o serie TV trovata.</p>
    </div>  
  </div>
</template>

<style scoped lang="scss">
.copertina {
  max-width: 100%;
  height: auto;
  display: block;
  margin-top: 10px;
}
  /* Stile generale per le stelle */
  .star {
    margin-right: 5px;
    font-size: 20px;
  }

  /* Stile per le stelle piene */
  .filled {
    color: gold;
    margin-right: 5px;
    font-size: 20px;
  }

  /* Stile per le stelle vuote */
  .empty {
    color: lightgray;
    margin-right: 5px;
    font-size: 20px;
  }
</style>