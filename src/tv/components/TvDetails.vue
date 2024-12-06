<script setup>
import { RouterLink , useRoute } from 'vue-router';
import { ref, onMounted } from 'vue';
import axios from 'axios';

const route = useRoute();
const { id } = route.params;  

const tv = ref({});
const cast = ref([]); 

const api_key = '1d0e6e1e3eaa2ea344d894e438f72919';
const tvDetailsURL = `https://api.themoviedb.org/3/tv/${id}?api_key=${api_key}`;
const creditsURL = `https://api.themoviedb.org/3/tv/${id}/credits?api_key=${api_key}`;

const getDetails = async () => {

    const resp = await axios.get(tvDetailsURL);
    tv.value = resp.data;
    
    const creditsResp = await axios.get(creditsURL);
    cast.value = creditsResp.data.cast.slice(0, 10);

}

onMounted(() => {
  getDetails();
});
</script>

<template>
    <div>
        <h3 class="text-dark">{{ tv.name }} - {{ tv.id }}</h3>
        <div class="details">
            <img :src="`https://image.tmdb.org/t/p/w500${tv.poster_path}`"
                alt="Poster"
                class="poster"
            />
            <div>
                <hr>
                <p><strong>Description:</strong> {{ tv.overview }}</p>
                <p><strong>Genre:</strong> {{ tv.genres?.map(genre => genre.name).join(', ') }}</p>
                <p><strong>Rating:</strong> {{ tv.vote_average }} / 10</p>
                <p><strong>Actors:</strong></p>
                <ul>
                    <li v-for="actor in cast" :key="actor.id"> {{ actor.name }} como {{ actor.character }}</li>
                </ul>
            </div>
        </div>
        <RouterLink class="btn btn-success" to="/tv">Back</RouterLink>
    </div>
</template>

<style scoped>
.Tvs-description {
    text-align: justify;
}

.poster {
    width: 400px;
    border-radius: 10px;
}

.btn-success {
    margin-top: 20px;
}
</style>
