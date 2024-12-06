<script setup>
    defineProps({
        id: Number,
        title: String,
        body:String
    })    
</script>

<template>
    <div>
        <hr>
        <div class="row">
            <div class="col">
                <h3 class="text-dark">{{ id }}</h3>
                <h3 class="text-info"> {{  title }}</h3>
                <p>{{ body }}</p>
            </div>
        </div>
    </div>
</template>
