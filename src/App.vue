<script>
import { store } from './store';
import axios from 'axios';

export default{
  data(){
    return {
      store,
      inputValue: '',
      movies: '',
      tvSeries: '',
    }
  },

  methods:{

    // richiesta axois per film e serie tv
    fetchMovies(){
      axios.get(`${store.uri.apiUri}search/movie?api_key=${store.uri.api_key}&query=${this.inputValue}`).then((res) => {
      this.movies = res.data.results;
      })
    },

    fetchTvSeries(){
      axios.get(`${store.uri.apiUri}search/tv?api_key=${store.uri.api_key}&query=${this.inputValue}`).then((res) => {
      this.tvSeries = res.data.results;
      })
    },

    // generazioine copertina film e serie tv
    pathImg(movie){
     return `${store.uri.imgPhat}${movie.poster_path}`
    },

    // funzione dell'input 
    search(){
      this.fetchMovies(),
      this.fetchTvSeries()
    },

    // genero stelle in base al voto
    stars(movie){
      return Math.floor(movie.vote_average / 2) + 1;
    },

    // bandiere lingua 
    iconsflag(movie){
      if(movie.original_language == 'en'){return './src/assets/flags/en.png'}
      if(movie.original_language == 'de'){return './src/assets/flags/de.png'}
      if(movie.original_language == 'fr'){return './src/assets/flags/fr.png'}
      if(movie.original_language == 'it'){return './src/assets/flags/it.png'}
      if(movie.original_language == 'ja'){return './src/assets/flags/ja.png'}
      else{return './src/assets/flags/generalFlag.png'}
    }
  },
}
</script>

<template>

  <!-- header -->
  <input type="text" v-model="inputValue" @keyup="search()">
  <button @click="search()">Cerca</button>

  <!-- film section -->
  <ul v-for="movie in movies">
    <img :src="pathImg(movie)" alt="">
    <li>Titolo film: {{ movie.title }}</li>
    <li>Titolo originale film: {{ movie.original_title }}</li>
    <li class="img-flag">
      <span>Lingua film: </span>
      <img :src="iconsflag(movie)" alt="">
    </li>
    <li>Voto film: {{ stars(movie) }}
      <span v-for="index in 5">
        <i v-if="index <= stars(movie)" class="fa-solid fa-star star"></i>
        <i v-else class="fa-regular fa-star star"></i>
      </span>
    </li>
  </ul>

  <!-- serie section -->
  <ul v-for="tvSerie in tvSeries">
    <img :src="pathImg(tvSerie)" alt="" v-show="pathImg(tvSerie)">
    <li>Nome serie tv: {{ tvSerie.name }}</li>
    <li>Nome originale serie tv: {{ tvSerie.original_name }}</li>
    <li class="img-flag">
      <span>Lingua serie tv: </span>
      <img :src="iconsflag(tvSerie)" alt="">
    </li>
    <li>Voto serie tv: {{ stars(tvSerie) }}
      <span v-for="index in 5">
        <i v-if="index <= stars(tvSerie)" class="fa-solid fa-star star"></i>
        <i v-else class="fa-regular fa-star star"></i>
      </span>
    </li>
  </ul>

 
</template>

<style lang="scss">
.img-flag{
  img{
    max-width: 30px;
  }
 
}

.star{
  color: gold;
}
</style>
