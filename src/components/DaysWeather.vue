<template>
    <div class="days-tab text-center">
        <div v-if="loading" class="loading">Loading...</div>
        <ul v-else class="p-0 d-flex flex-wrap">
            <li v-for="day in forecast" :key="day.data" class="li_active mb-3">
                <div class="py-3"><img :src="day.iconUrl"/></div>
                <div class="py-3">{{ getDayName(day.data) }}</div>
                <div class="py-3">{{ day.temprature }}&deg;C</div>
            </li>
        </ul>
    </div>
</template>

<script>
import axios from 'axios';
import moment from 'moment';
    export default {
        name:'DaysWeather',
        props:{
            cityName: String,
        },
        data(){
            return{
                forecast:[],
                loading:true,
                iconUrl:null,
            };
        },
        methods:{
            async fetchWeatherData(){
                const apiKey = '2d904ac69cf40ef33bb83a76f4cae686';
                const city = this.cityName;
                const apiUrl = `https://api.openweathermap.org/data/2.5/forecast?q=${city}&units=metric&APPID=${apiKey}`; 
                
                await axios.get(apiUrl).then(Response =>{
                    const forecastData = Response.data.list;
                    const filterData = forecastData.map(item =>{
                        return{
                            data: moment(item.dt_txt.split(' ')[0]),
                            temprature: Math.round(item.main.temp),
                            description:item.weather[0].description,
                            iconUrl: `https://api.openweathermap.org/img/w/${item.weather[0].icon}.png`,
                        }
                    }).reduce((acc,item)=>{
                        if(!acc.some(day=>day.data.isSame(item.data,'day'))){
                            acc.push(item);
                        }
                        return acc;
                    },[]).slice(1,5);
                    console.log(filterData,"working");
                    this.forecast = filterData;
                    this.loading = false;
                }).catch(error =>{
                    console.error('Error fetching weather data: ',error);
                    this.loading = false;
                });
            },
            getDayName(date){
                return date.format('ddd');
            }
        },
        mounted(){
            this.fetchWeatherData();
        },
    }
</script>

<style scoped>
.days-tab{
    width: 90%;
    box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2);
    border-radius: 20px;
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
    width: 21%;
    max-width: 21%;
    font-size: 1vw;
    line-height: 1.2;
}
@media (max-width: 767px) {
  li {
    width: 100%; 
    max-width: none;
  }
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
    color: #ffffff;
    transition: background-color 0.5s;
    border-radius: 10px;
}

</style>