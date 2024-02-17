<script>
import { store } from '../store';
import AppCard from './AppCard.vue';

export default{
    data(){
        return{
            store,
        }
    },

    components:{ AppCard }
    
}
</script>

<template>

    <h2>FILM</h2>
    <app-card v-for="movie in store.movies" :production="movie"></app-card>

    <h2>SERIE TV</h2>
    <app-card v-for="tvSerie in store.tvSeries" :production="tvSerie"></app-card>
    
</template>

<style lang="scss" scoped>
    h2{
        font-size: 40px;
    }
</style>