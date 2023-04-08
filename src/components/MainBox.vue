<script setup>

import { defineProps } from 'vue';

const props = defineProps(["icon", "title", "data"])


</script>

<template>

    <div class="card-container">
        <div class="card-header">
            <div class="logo-container">
                <img alt="Vue logo" :src="require(`../assets/images/${props.icon}.png`)">
            </div>
            <h3>
                {{ props.title }}
            </h3>
        </div>
        <div class="card-data">
            {{ props.data }}
        </div>
    </div>

</template>

<style scoped>

.card-container{
    background-color: white;
    border: 1px solid #E5E7EB;
    border-radius: var(--st-border-radius);
    box-shadow: var(--hb-box-shadow);
    padding: .3rem .5rem .5rem .5rem;
    color: white;
}



.card-header{
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: flex-start;
    padding: 1rem;
    font-size: 1.6rem;
    text-transform: uppercase;
    
}

.logo-container{
    width: 5rem;
    height: 5rem;
    border: 2px solid white;
    border-radius: 50%;
    display: flex;
    justify-content: center;
    align-items: center;
    margin-bottom: 1rem;
}

.logo-container img{
    width: 3.4rem;
    object-fit: contain;
    padding: 1rem .4rem;
}

.card-data{
    font-size: 2.7rem;
    text-align: center;
    font-family: 'Bellota Text', sans-serif;
    font-weight: 700;
}


</style>