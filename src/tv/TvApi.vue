<script setup>
import { ref } from 'vue';
import { RouterLink } from 'vue-router';

const api_key = '1d0e6e1e3eaa2ea344d894e438f72919';
const base_URL = `https://api.themoviedb.org/3/tv/airing_today?api_key=${api_key}`;

const airingToday = ref([]);
const getAiringTodayShows = async () => {

    const resp = await fetch(base_URL);
    const data = await resp.json();
    airingToday.value = data.results.map((Tvs) => {
      return {
        id: Tvs.id,
        name: Tvs.name,
        overview: Tvs.overview,
        poster_path: Tvs.poster_path,
      }
    })
}
getAiringTodayShows();
</script>

<template>
  <h3 class="display-6 text-dark">List of Airing Today TV Shows</h3>
  <template v-for="tv in airingToday" :key="tv.id">
    <RouterLink :to="{ name: 'tv-details', params: { id: tv.id } }" class="text-primary">
      {{ tv.name }}
    </RouterLink>
    <hr />
  </template>
</template>
