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

    search(){
      this.fetchMovies(),
      this.fetchTvSeries()
    },

    iconsflag(movie){
      if(movie.original_language == 'en'){return './src/assets/flags/en.png'}
      if(movie.original_language == 'de'){return './src/assets/flags/de.png'}
      if(movie.original_language == 'fr'){return './src/assets/flags/fr.png'}
      if(movie.original_language == 'it'){return './src/assets/flags/it.png'}
      if(movie.original_language == 'ja'){return './src/assets/flags/ja.png'}
      else{return './src/assets/flags/generalFlag.png'}
    }
  },

  created(){
   this.search()
  }
}
</script>

<template>
  <h1>Hello BoolFlix</h1>

  <input type="text" v-model="inputValue" @keyup="search()">
  <button @click="search()">Cerca</button>

  <ul v-for="movie in movies">
    <li>Titolo film: {{ movie.title }}</li>
    <li>Titolo originale film: {{ movie.original_title }}</li>
    <li class="img-flag">
      <span>Lingua film: </span>
      <img :src="iconsflag(movie)" alt="">
    </li>
    <li>Voto film: {{ movie.vote_average }}</li>
  </ul>

  <ul v-for="tvSerie in tvSeries">
    <li>Nome serie tv: {{ tvSerie.name }}</li>
    <li>Nome originale serie tv: {{ tvSerie.original_name }}</li>
    <li class="img-flag">
      <span>Lingua serie tv: </span>
      <img :src="iconsflag(tvSerie)" alt="">
    </li>
    <li>Voto serie tv: {{ tvSerie.vote_average }}</li>
  </ul>

 
</template>

<style lang="scss">
.img-flag{
  img{
    max-width: 30px;
  }
 
}
</style>
