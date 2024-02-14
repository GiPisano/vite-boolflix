<script>
import { store } from './store';
import axios from 'axios';

export default{
  data(){
    return {
      store,
      inputValue: '',
      films: '',
    }
  },

  methods:{
    search(){
      axios.get(`${store.apiUri}query=${this.inputValue}`).then((res) => {
      this.films = res.data.results;
    })
    }
  },

  created(){
   this.search()
  }
}
</script>

<template>
  <h1>Hello BoolFlix</h1>

  <input type="text" v-model="inputValue">
  <button @click="search()">Cerca</button>

  <ul v-for="film in films">
    <li>{{ film.title }}</li>
    <li>{{ film.original_title }}</li>
    <li>{{ film.original_language}}</li>
    <li>{{ film.vote_average }}</li>
  </ul>

 
</template>

<style>

</style>
