<script>
import { store } from './store';
import axios from 'axios';

import AppHeader from './components/AppHeader.vue';
import AppMain from './components/AppMain.vue'

export default{
  data(){
    return {
      store,
    }
  },

  components:{ AppHeader, AppMain },

  methods:{
    // richiesta axois per film e serie tv
    fetchMovies(inputValue){
      axios.get(`${store.api.apiUri}/search/movie`, {
        params: {
          api_key: store.api.key,
          query: inputValue,
        },
      })
      .then((res) => {
      store.movies = res.data.results.map((movie) => {
        return{
          title: movie.title,
          original_title: movie.original_title,
          language: movie.original_language,
          vote: Math.floor(movie.vote_average / 2) + 1,
          poster_path: movie.poster_path
        }
      });
      })
    },

    fetchTvSeries(inputValue){
      axios.get(`${store.api.apiUri}/search/tv`, {
        params: {
          api_key: store.api.key,
          query: inputValue,
        },
      })
      .then((res) => {
      store.tvSeries = res.data.results.map((tvSerie) => {
        return{
          title: tvSerie.name,
          original_title: tvSerie.original_name,
          language: tvSerie.original_language,
          vote: Math.floor(tvSerie.vote_average / 2) + 1,
          poster_path: tvSerie.poster_path
        }
      });
      })
    },

    // generazioine copertina film e serie tv
    pathImg(movie){
     return `${store.api.imgPhat}${movie.poster_path}`
    },

    // funzione dell'input 
    performSearch(inputValue){
      this.fetchMovies(inputValue),
      this.fetchTvSeries(inputValue)
    },

    // bandiere lingua 
    iconsflag(movie){
      if(movie.language == 'en'){return './src/assets/flags/en.png'}
      if(movie.language == 'de'){return './src/assets/flags/de.png'}
      if(movie.language == 'fr'){return './src/assets/flags/fr.png'}
      if(movie.language == 'it'){return './src/assets/flags/it.png'}
      if(movie.language == 'ja'){return './src/assets/flags/ja.png'}
      else{return './src/assets/flags/generalFlag.png'}
    }
  },


}
</script>



<template>

  <app-header @search="performSearch"></app-header> 

  <!-- film section -->
  <ul v-for="movie in store.movies">
    <img :src="pathImg(movie)" alt="">
    <li>Titolo film: {{ movie.title }}</li>
    <li>Titolo originale film: {{ movie.original_title }}</li>
    <li class="img-flag">
      <span>Lingua film: </span>
      <img :src="iconsflag(movie)" alt="">
    </li>
    <li>Voto film: {{ movie.vote }}
      <span v-for="index in 5">
        <i v-if="index <= movie.vote" class="fa-solid fa-star star"></i>
        <i v-else class="fa-regular fa-star star"></i>
      </span>
    </li>
  </ul>

  <!-- serie section -->
  <ul v-for="tvSerie in store.tvSeries">
    <img :src="pathImg(tvSerie)" alt="" v-show="pathImg(tvSerie)">
    <li>Nome serie tv: {{ tvSerie.title }}</li>
    <li>Nome originale serie tv: {{ tvSerie.original_title }}</li>
    <li class="img-flag">
      <span>Lingua serie tv: </span>
      <img :src="iconsflag(tvSerie)" alt="">
    </li>
    <li>Voto serie tv: {{ tvSerie.vote }}
      <span v-for="index in 5">
        <i v-if="index <= tvSerie.vote" class="fa-solid fa-star star"></i>
        <i v-else class="fa-regular fa-star star"></i>
      </span>
    </li>
  </ul>

 
</template>

<style lang="scss">
@use './styles/general.scss';

.img-flag{
  img{
    max-width: 30px;
  }
 
}

.star{
  color: gold;
}
</style>
