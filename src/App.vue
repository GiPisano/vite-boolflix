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
          poster_path: movie.poster_path,
          overview: movie.overview
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
          poster_path: tvSerie.poster_path,
          overview: tvSerie.overview
        }
      });
      })
    },

    // funzione dell'input Search
    performSearch(inputValue){
      this.fetchMovies(inputValue),
      this.fetchTvSeries(inputValue)
    },
  },


}
</script>



<template>
  <div class="container">
    <app-header @search="performSearch"></app-header> 

    <app-main></app-main>
  </div>
</template>

<style lang="scss">
@use './styles/general.scss';

.container{
  overflow: hidden;
}
</style>
