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
      imageSize: 'w342', // Dimensione dell'immagine scelta
      hoverCard: null // Stato della card in hover
    };
  },

  methods: {
    async searchMovies() {
      const apiKey = ;
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
    <header>
      <h1>MyNetflix</h1>
      <div class="search-bar">
        <input v-model="query" type="text" @keyup.enter="searchMovies" placeholder="Cerca film o serie TV" />
        <button @click="searchMovies">Cerca</button>
      </div>
    </header>

    <!-- risultati dei Film -->
    <section v-if="movies.length > 0" class="section-results">
      <h2>Film trovati:</h2>
      <div class="card-container">
        <div
          v-for="movie in movies"
          :key="movie.id"
          class="card"
          @mouseover="hoverCard = movie.id"
          @mouseleave="hoverCard = null"
        >
          <!-- mostra la copertina del film -->
          <img class="poster" :src="getPosterUrl(movie.poster_path)" alt="Poster del film" v-if="movie.poster_path" />
          <div class="card-content" v-if="hoverCard === movie.id">
            <h3>{{ movie.title }}</h3>
            <p><strong>Titolo Originale:</strong> {{ movie.original_title }}</p>
            <p><strong>Lingua:</strong> 
              <img :src="getLanguageFlag(movie.original_language)" alt="lingua" style="width: 30px; height: 20px;" />
            </p>
            <p><strong>Voto:</strong></p>
            <!-- stelle -->
            <span v-for="star in getStarRating(movie.vote_average)" :key="star" class="star filled">★</span>
            <span v-for="emptyStar in 5 - getStarRating(movie.vote_average)" :key="emptyStar" class="star empty">★</span>
            <p><strong>Overview:</strong> {{ movie.overview }}</p>
          </div>
        </div>
      </div>
    </section>

    <!-- risultati delle Serie TV -->
    <section v-if="tvShows.length > 0" class="section-results">
      <h2>Serie TV trovate:</h2>
      <div class="card-container">
        <div
          v-for="show in tvShows"
          :key="show.id"
          class="card"
          @mouseover="hoverCard = show.id"
          @mouseleave="hoverCard = null"
        >
          <!-- visualizza la copertina della serie TV -->
          <img class="poster" :src="getPosterUrl(show.poster_path)" alt="Poster della serie TV" v-if="show.poster_path" />
          <div class="card-content" v-if="hoverCard === show.id">
            <h3>{{ show.name }}</h3>
            <p><strong>Titolo Originale:</strong> {{ show.original_name }}</p>
            <p><strong>Lingua:</strong> 
              <img :src="getLanguageFlag(show.original_language)" alt="lingua" style="width: 30px; height: 20px;" />
            </p>
            <p><strong>Voto:</strong></p>
            <!-- stelle -->
            <span v-for="star in getStarRating(show.vote_average)" :key="star" class="star filled">★</span>
            <span v-for="emptyStar in 5 - getStarRating(show.vote_average)" :key="emptyStar" class="star empty">★</span>
            <p><strong>Overview:</strong> {{ show.overview }}</p>
          </div>
        </div>
      </div>
    </section>

    <!-- messaggio per nessun risultato -->
    <section v-else>
      <p>Nessun film o serie TV trovata.</p>
    </section>  
  </div>
</template>

<style scoped lang="scss">

#app {
  font-family: 'Arial', sans-serif;
  text-align: center;
  background-color: #141414;
  color: white;
  padding: 20px;
}

header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 20px;
}

h1 {
  font-size: 2.5rem;
  color: red;
}

.search-bar input {
  padding: 10px;
  width: 300px;
  font-size: 16px;
}

.search-bar button {
  padding: 10px 20px;
  background-color: red;
  color: white;
  border: none;
  cursor: pointer;
}

/* Stile delle card */
.card-container {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
}

.card {
  position: relative;
  margin: 10px;
  width: 200px;
  height: 300px;
  background-color: #222;
  overflow: hidden;
  transition: transform 0.3s ease;
}

.card:hover {
  transform: scale(1.1);
}

.poster {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.card-content {
  position: absolute;
  bottom: 0;
  left: 0;
  right: 0;
  padding: 10px;
  background: rgba(0, 0, 0, 0.7);
  color: white;
  opacity: 0;
  transition: opacity 0.3s ease;
}

.card:hover .card-content {
  opacity: 1;
}

/* Stile delle stelle */
.star {
  margin-right: 5px;
  font-size: 20px;
}

.filled {
  color: gold;
}

.empty {
  color: lightgray;
}
</style> 