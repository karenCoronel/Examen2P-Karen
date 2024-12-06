<script setup>
import { RouterLink, useRoute } from 'vue-router';
import axios from 'axios';
import { ref } from 'vue';

const route = useRoute();
const { id } = route.params;

const movie = ref({});
const cast = ref([]); 

const api_key = '1d0e6e1e3eaa2ea344d894e438f72919';
const movieDetailsURL = `https://api.themoviedb.org/3/movie/${id}?api_key=${api_key}`;
const creditsURL = `https://api.themoviedb.org/3/movie/${id}/credits?api_key=${api_key}`;

const getDetailsMovieById = async () => {
        const movieResp = await axios.get(movieDetailsURL);
        movie.value = movieResp.data;

        const creditsResp = await axios.get(creditsURL);
        cast.value = creditsResp.data.cast.slice(0, 10);
    
};

getDetailsMovieById();
</script>

<template>
    <div>
        <h3 class="text-primary">{{ movie.title }} - {{ movie.id }}</h3>
        <div class="movie-details">
            <img :src="`https://image.tmdb.org/t/p/w500${movie.poster_path}`"
                alt="Movie Poster"
                class="movie-poster"
            />
            <div>
                <hr>
                <p><strong>Description:</strong> {{ movie.overview }}</p>
                <p><strong>Date:</strong> {{ movie.release_date }}</p>
                <p><strong>Genre:</strong> {{ movie.genres?.map(genre => genre.name).join(', ') }} </p>
                <p><strong>Rating:</strong> {{ movie.vote_average }} / 10</p>
                <p><strong>Actors:</strong></p>
                <ul>
                    <li v-for="actor in cast" :key="actor.id">{{ actor.name }} as {{ actor.character }}</li>
                </ul>
            </div>
        </div>
        <RouterLink class="btn btn-success" to="/movie"> Back</RouterLink>
    </div>
</template>

<style scoped>
.movie-details {
    display: flex;
    gap: 20px;
    margin-top: 20px;
}
.movie-poster {
    width: 400px;
    border-radius: 10px;
}
.movie-description {
    text-align: justify;
}
.btn-success {
    margin-top: 20px;
}
</style>