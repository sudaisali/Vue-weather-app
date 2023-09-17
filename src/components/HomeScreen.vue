<template>
 <div class="header container-fluid h-25 p-3 mb-1">
    <div class="d-md-flex  justify-content-lg-around">
        <h1 class="mb-2">Weather App</h1>
    <div class="d-flex justify-content-center h-100 w-60">
        <div class="searchbar w-60 mx-2">
            <input type="text" class="input form-control" placeholder="Enter a city" size="50" v-model="city">
        </div>
        <div class="btn-search btn btn-primary border-0" @click="searchWeather">Search</div>
    </div>
    </div>
  
 </div>
 <div v-show="show_weather">
    <WeatherScreen  :city="city"/>
</div>
 
</template>

<script setup>
import WeatherScreen  from './WeatherScreen.vue'

import {ref} from 'vue'
const city = ref('')
const show_weather = ref(false)

 function searchWeather(){
  
  show_weather.value = !show_weather.value
}
</script>


<style>


.header{
    background: linear-gradient(to top, #09203f 0%, #537895 100%);
    border-bottom-left-radius: 20px;
    border-bottom-right-radius: 20px;
    color: #fff;
    text-align: center;
    font-family: 'Franklin Gothic Medium', 'Arial Narrow', Arial, sans-serif;
    
}

.btn-search{
    
    background-image: linear-gradient(135deg, #1900ff 0%, #16edfc 100%);
}
</style>
