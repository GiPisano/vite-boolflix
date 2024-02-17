<script>
import { store } from '../store';

export default{
    props:{
        production: Object,
    },

    methods:{
         // generazioine path copertina film e serie tv
        pathImg(production){
        return `${store.api.imgPhat}${production.poster_path}`
        },
        
        // bandiere lingua 
        iconsflag(production){
        if(production.language == 'en'){return './src/assets/flags/en.png'}
        if(production.language == 'de'){return './src/assets/flags/de.png'}
        if(production.language == 'fr'){return './src/assets/flags/fr.png'}
        if(production.language == 'it'){return './src/assets/flags/it.png'}
        if(production.language == 'ja'){return './src/assets/flags/ja.png'}
        else{return './src/assets/flags/generalFlag.png'}
        }
    
    }
}
</script>

<template>
    <ul>
        <img :src="pathImg(production)" alt="">
        <li>Titolo: {{ production.title }}</li>
        <li>Titolo originale: {{ production.original_title }}</li>
        <li class="img-flag">
        <span>Lingua: </span>
        <img :src="iconsflag(production)" alt="">
        </li>
        <li>Voto: {{ production.vote }}
        <span v-for="index in 5">
            <i v-if="index <= production.vote" class="fa-solid fa-star star"></i>
            <i v-else class="fa-regular fa-star star"></i>
        </span>
        </li>
    </ul>
</template>

<style lang="scss" scoped>
    .img-flag{
    img{
        max-width: 30px;
    }
    
    }

    .star{
    color: gold;
    }
</style>