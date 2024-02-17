<script>
import { store } from '../store';

export default{
    props:{
        production: Object,
    },

    methods:{
         // generazioine path copertina film e serie tv
        pathImg(production){
        if(production.poster_path) {
            return `${store.api.imgPhat}${production.poster_path}`
        } else{
            return '/src/assets/imgDefault/default.png'
        } 
        },
        
        // bandiere lingua 
        iconsflag(langCode){
        if(langCode == 'en'){return '/src/assets/flags/en.png'}
        if(langCode == 'fr'){return '/src/assets/flags/fr.png'}
        if(langCode == 'it'){return '/src/assets/flags/it.png'}
        if(langCode == 'ja'){return '/src/assets/flags/ja.png'}
        if(langCode == 'de'){return '/src/assets/flags/de.png'}
        return '/src/assets/flags/generalFlag.png'
        }
    
    }
}
</script>

<template>
    <ul class="card">
        <div class="container-img">

            <img :src="pathImg(production)" alt="" class="img-production">

        
            <div class="info-production">
                <li><span class="title">Titolo:</span> {{ production.title }}</li>
                <li><span class="title">Titolo originale: </span>{{ production.original_title }}</li>
                <li class="img-flag">
                <span class="title">Lingua: </span>
                <img :src="iconsflag(production.language)" alt="">
                </li>
                <li><span class="title">Voto: </span>{{ production.vote }}
                    <span v-for="index in 5">
                        <i v-if="index <= production.vote" class="fa-solid fa-star star"></i>
                        <i v-else class="fa-regular fa-star star"></i>
                    </span>
                </li>
                <li><span class="title">overview: </span> {{ production.overview}}</li>
            </div>
        </div>
    </ul>
</template>

<style lang="scss" scoped>
    .img-flag{
        img{
            width: 30px;
            border: 1px solid white;
        }
    
    }

    .title{
        font-size: 25px;
        font-weight: bold;
    }

    .star{
    color: gold;
    }

    .card{
        .container-img {
            position: relative;
            height: 100%;
        }

        .info-production {
            color: white;
            position: absolute;
            border: 1px solid white;
            overflow: hidden;
            padding: 10px;
            top: 0;
            bottom: 0;
            left: 0;
            right: 0;
            opacity: 0;
        }
    }

    .container-img:hover .info-production {
        opacity: 1;
        height: 100%;
        display: flex;
        flex-direction: column;
        gap: 20px;
        overflow: scroll;
    }

    .container-img:hover .img-production {
        opacity: 0;
    }

    .img-production{
        width: 100%;
        height: 100%;
    }


</style>