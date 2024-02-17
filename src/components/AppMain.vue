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
    <div class="container">
        <h2 v-show="store.movies.length > 0">FILM</h2>
        <div class="movie-section">
            <app-card v-for="movie in store.movies" :production="movie"></app-card>
        </div>

        <h2 v-show="store.tvSeries.length > 0">SERIE TV</h2>
        <div class="tvSerie-section">
            <app-card v-for="tvSerie in store.tvSeries" :production="tvSerie"></app-card>
        </div>
    </div>
</template>

<style lang="scss" scoped>
    .container{
        height: 100vh;
        overflow: scroll;
        background-color: rgb(21, 21, 21);
    }   
    h2{
        font-size: 40px;
        padding: 10px;
        text-align: center;
        padding: 50px;
        color: white;
    }

    h2:first-child{
        margin-top: 75px;
    }

    .movie-section,
    .tvSerie-section{
        display: flex;
        justify-content: center;
        flex-wrap: wrap;
        gap: 10px;
        padding: 30px 0;
    }

</style>