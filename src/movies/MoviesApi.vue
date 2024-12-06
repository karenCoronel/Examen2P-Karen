<script setup>

import { ref } from 'vue';
import { RouterLink } from 'vue-router';
import Loading from './components/Loading.vue';

const api_key = '1d0e6e1e3eaa2ea344d894e438f72919';
const base_URL = `https://api.themoviedb.org/3/movie/now_playing?api_key=${api_key}`;

const nowplaying = ref([])
const getNowplayingMovies = async () => {

    const resp = await fetch(base_URL);
    const data = await resp.json()
    nowplaying.value = data.results.map((movie) => {
        return {
            id: movie.id,
            title: movie.title,
            overview: movie.overview,
            poster_path: movie.poster_path
        }
    })


}
getNowplayingMovies()

</script>

<template>
    <h3 class="display-6 text-dark">List of Now Playing Movies</h3>
    <template v-for="m in nowplaying" :key="m.id"> 
        <RouterLink :to="{ name: 'movie-details', params: { id: m.id } }" class="text-primary">
            {{ m.title }}
        </RouterLink>
        <hr />
    </template>
</template>