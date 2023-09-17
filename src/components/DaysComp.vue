<template>

    <div class="days-tab text-center">
        <div v-if="loading" class="loading p-3">Loading....</div>
        <ul v-else class="p-2">
            <li v-for="day in forecast" :key="day.date" class="li_active">
                <div class="py-3"><img :src="day.iconUrl" ></div>
                <div class="py-3">{{getDateName(day.date)}}</div>
                <div class="py-3">{{ day.temperature }}&deg;C</div>
            </li>
           
        </ul>
    </div>

</template>


<script setup>
import { defineProps , ref} from 'vue';
import axios from 'axios';
import moment from 'moment';

 const props = defineProps({
    city:String
 });

 const loading = ref(true);
 const forecast = ref([]);



async function fetchdata(){
          const API_KEY = '14d4a99e342dffd6205f3947e06e165d';
          const API_URL = `https://api.openweathermap.org/data/2.5/forecast?q=${props.city}&units=metric&appid=${API_KEY}`
          await axios.get(API_URL).then(Response=>{

            const forecastdata = Response.data.list;
            const filtereddata = forecastdata.map(item=>{
                return{
                    date : moment(item.dt_txt.split(' ')[0]),
                    temperature :Math.round(item.main.temp),
                    description:item.weather[0].description,
                    iconUrl : `https://api.openweathermap.org/img/w/${item.weather[0].icon}.png`,

                }; 
          }).reduce((acc,item)=>{
            if(!acc.some(day=>day.date.isSame(item.date,'day'))){
                acc.push(item);
            }
            return acc;
          },[]).slice(1,5);
          console.log(filtereddata)
          forecast.value = filtereddata;
          loading.value = false;
        }).catch(error =>{
            console.error("Error in fetching : " ,error)
            this.loading = false;
        })
          
       

}

function getDateName(date){
    return date.format('ddd')
}
fetchdata()
 

</script>


<style scoped>

.days-tab{
    width: 90%;
    box-shadow: 0 4px 8px 0 rgba(0,0,0,0.2), 0 6px 20px 0 rgba(0,0,0,0.19);
    border-radius: 20px;
    width: 90%;
    margin: auto;
}
.loading{
    color: #fff;
}
ul{
    margin: 0;
}
li{
    display: inline-block;
    list-style: none;
    height: 100%;
    width: 20%;
    font-size: 1vw;
    line-height: 1.2;
}
span{
    display: block;
    margin-bottom: 5px;
    font: 100% sans-serif;
    height: 35px;
}
.li_active{
    background: #253d5c;
    color: #222831;
    border-radius: 10px;
    margin: 0.5rem;
    color: #fff;
    font-weight: 600;

}
.li_active:hover{
    transform: scale(1.2);
    transition: transform 0.1s ease;
}
.li_active_temp{
    display: inline-block;
    background-color: #222831;
    color: #fff;
    transition: background-color 0.5s;
    border-radius: 10px;
}
.li_active_temp:hover{
    transform: scale(1.2);
    transition: transform 0.1s ease;
    background: #fff;
    border-radius: 10px;
    color: #191a1f;
}
@media screen and (max-width: 990px){
    li{
        width: 40%;
        font-size: 3vw;
    }
    
}

</style>