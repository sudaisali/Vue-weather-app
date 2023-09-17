<template>

 <div class="container p-0 ">
    <div class="d-flex flex-lg-nowrap flex-wrap ">
        <div class="card main-div  w-100  ">
            <div class="p-3">
                <div class="mb-1 day">Today</div>
                <div class="text-light date mb-0">{{date}}</div>
                <small>{{time}}</small>
                <h2 class="place"><i class="fa fa-location">{{ wname }}<small>{{country}}</small></i></h2>
                <div class="temp">
                    <h1 class="weather-temp">{{temperature}}&deg;</h1>
                    <div class="text-light">{{ description }} <img :src="iconUrl" alt=""></div>
                </div>
            </div>
        </div>

        
        <div class="card card-2 w-100">
            <table class="m-4">
                <tbody>
                    <tr>
                        <th>Pressure</th>
                         <td>{{ pressure }}</td>
                    </tr>
                    <tr>
                        <th>Humidity</th>
                         <td>{{ humidity }}</td>
                    </tr>
                    <tr>
                        <th>Wind Speed</th>
                         <td>{{ wind }}</td>
                    </tr>
                </tbody>
            </table>
            <DaysComp :city="city"/>
            <div class="div d-flex m-3 justify-content-center" id="div_From">
                <form action="">
                    <input type="button" value="Change Location" class="btn change-location btn btn-primary border-0">
                </form>
            </div>

        </div>
    </div>
   
 </div>

</template>

<script setup>
import DaysComp from './DaysComp.vue'
import {defineProps, ref} from 'vue'
import axios from 'axios'

const props = defineProps({
    city:String
})
  

 const temperature = ref('');
 const  description = ref('');
 const iconUrl = ref('');
 const date = ref('');
 const time = ref('');
 const wname = ref(''); 
 const pressure = ref(''); 
 const wind = ref(''); 
 const humidity = ref(''); 
 const country = ref(''); 
 const monthNames = ref(['January','Feburary','March','April','May','June','July','August','September'
,'October','November','December']);



async function created(city){
    const response = await axios.get(`https://api.openweathermap.org/data/2.5/weather?q=${city}&appid=14d4a99e342dffd6205f3947e06e165d`);
    const weatherData = response.data;
    temperature.value = Math.round(weatherData.main.temp);
    description.value = weatherData.weather[0].description;
    iconUrl.value = `https://api.openweathermap.org/img/w/${weatherData.weather[0].icon}.png`;
    wname.value = weatherData.name;
    pressure.value = weatherData.main.pressure;
    wind.value = weatherData.wind.speed;
    country.value = weatherData.sys.country;
    humidity.value = weatherData.main.humidity;
    const d = new Date();
    date.value = d.getDate()  + '-' + monthNames.value[d.getMonth()] +'-'+d.getFullYear();
    time.value = d.getHours() + '-' + d.getMinutes() + '-' + d.getSeconds();
}

created(props.city)



</script>

<style scoped>

body{
    background-color: #343d4b;
}
.weather-temp{
    margin: 0;
    font-weight: 700;
    font-size: 4em;
}
.h2.mb-1.day{
    font-size: 3rem;
    font-weight: 400;
}

.main-div{
    margin: 15px;
    border-radius: 20px;
    color: #fff;
    background-size: cover;
    background-position: center;
    background-blend-mode: overlay;
    background-color: rgba(0, 0, 0, 0.5);
    background-repeat: no-repeat;
    background-image: url(https://media.istockphoto.com/id/955765580/photo/palm-tree-in-beach-in-tropical-island-caribbean-guadalupe.jpg?s=612x612&w=0&k=20&c=IAh5qJ5T6JZUUebYwtAjt_ySYFQZ7otp70up_9MIE_o=);
}
.temp{
    position: absolute;
    bottom: 0;
}
.main-div:hover{
    transform: scale(1.1);
    transition: transform 0.5s ease;
    z-index: 1;
}
.card-2{
    background-color: #212730;
    border-radius: 20px;
    margin: 15px;
}
.card-details{
    margin-left: 19px;
}
.h1_left{
    position: absolute;
    bottom: 25px;
    left: 16px;
    font-size: 3vw;
    line-height: 1.2;
}
.h2_left{
    position: absolute;
    left: 16px;
    font-size: 2vw;
    line-height: 0.5;
}
.h3_left{
    position: absolute;
    left: 16px;
    font-size: 2vw;
    line-height: 0.5;
}
h2>i>small{
    margin-left: 15px;
}
.h3_left small{
    font-size: 1rem;
}
table{
    position: relative;
    left: 15px;
    border-collapse: separate;
    border-spacing: 15px;
    width: 85%;
    text-align: left;
    max-width: 600px;
    margin: 0 auto;
}
th,td{
    font-size: 18px;
    color: #fff;
}

td{
    text-align: right;
}

table, tr:hover{
    color: red;
}
.change-location{
    background-image: linear-gradient(135deg, #1900ff 0%, #16edfc 100%);
}
@media  screen and (max-width: 990px){
    .main-div{
        height: 400px;
    }
}
</style>