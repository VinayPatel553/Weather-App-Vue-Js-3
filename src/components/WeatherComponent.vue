<template>
    <div class="container p-0 mb-3">
        <div class="d-flex flex-column flex-md-row">
            <div class="card main-div w-100">
                <div class="p-3">
                    <h2 class="mb-1 day">{{ day }}</h2>
                    <p class="text-light date mb-0">{{date}}</p>
                    <p class="text-light date mb-0">{{time}}</p>
                    <h2 class="place">
                        <svg xmlns="http://www.w3.org/2000/svg" width="25" height="25" fill="currentColor" class="bi bi-geo-alt-fill" viewBox="0 0 20 20">
                        <path d="M8 16s6-5.686 6-10A6 6 0 0 0 2 6c0 4.314 6 10 6 10m0-7a3 3 0 1 1 0-6 3 3 0 0 1 0 6"/>
                        </svg>{{ name }},<small>{{ country }}</small>
                    </h2>
                        <div class="temp">
                            <h2 class="weather-temp">{{ temprature }}&deg;</h2>
                            <h2 class="text-light">{{ description }} <img :src="iconUrl"/></h2>
                        </div>
                </div>
            </div>
            <div class="card card-2 w-100">
            <table class="m-4">
                <tbody>
                    <tr>
                        <th>Wind</th>
                        <td>{{ wind }} km/h</td>
                    </tr>
                    <tr>
                        <th>Humidity</th>
                        <td>{{ humidity }}%</td>
                    </tr>
                    <tr>
                        <th>Visibility</th>
                        <td>{{ visibility }} m</td>
                    </tr>
                </tbody>
            </table>

            <DaysWeather :cityName="cityname"/>

            <div id="div_Form" class="d-flex m-3 justify-content-center">
                <form action="">
                    <input type="button" value="Change Location" v-on:click="changeLocation" class="btn change-btn btn-primary">
                </form>
            </div>
        </div>
        </div>
    </div>
</template>

<script>
import axios from 'axios';
import DaysWeather from '../components/DaysWeather.vue';
    export default {
        name:'WeatherComponent',
        components:{
            DaysWeather,
        },
        props:{
            city:String
        },
        data(){
            return{
                cityname:this.city,
                temprature:null,
                description:null,
                name:null,
                iconUrl:null,
                date:null,
                time:null,
                day:null,
                visibility:null,
                wind:null,
                humidity:null,
                country:null,
                monthNammes:["January","Feburary","March","April","May","June",
                            "July","August","September","October","November","December",]
            }
        },
        methods:{
            changeLocation(){
                window.location.reload();
            }
        },
        async created(){
            const response = await axios.get(`https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&APPID=2d904ac69cf40ef33bb83a76f4cae686`);
            const weatherData = response.data;
            this.temprature = Math.round(weatherData.main.temp);
            this.description = weatherData.weather[0].description;
            this.name = weatherData.name;
            this.wind = weatherData.wind.speed;
            this.humidity = weatherData.main.humidity;
            this.visibility = weatherData.visibility;
            this.country = weatherData.sys.country;
            this.iconUrl = `https://api.openweathermap.org/img/w/${weatherData.weather[0].icon}.png`;
            const d = new Date();
            this.date = d.getDate() + '-' + this.monthNammes[d.getMonth()]+ '-' +d.getFullYear();
            this.time = d.getHours() + ':' + d.getMinutes()+ ':' + d.getSeconds(); 
            const dayOfWeek = ['Sunday', 'Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday'][d.getDay()];
            this.day = dayOfWeek;

            console.log(weatherData);
        }
    }
</script>

<style scoped>
body{
    background-color: #343d4b;
}
.weather-temp{
    /* margin: 0; */
    font-weight: 600;
    font-size: 4em;
}
h2.mb-1.day{
    font-size: 3rem;
    font-weight: 400;
}
.main-div{
    border-radius: 20px;
    color: #fff;
    background-image: url('../assets/wback-img.jpeg');
    background-size: cover;
    background-position: center;
    background-color: rgba(0, 0, 0, 0.5);
    background-repeat: no-repeat;
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
.card {
  background-color: #29303b;
  border-radius: 20px;
  color: #fff;
  padding: 20px;
  box-shadow: 0 0 20px rgba(0, 0, 0, 0.3);
}
.day {
  font-size: 2rem;
  font-weight: 500;
}

.date {
  font-size: 1.1rem;
}

.place {
  font-size: 1.8rem;  
  margin-top: 5px;
  margin-bottom: 20px;
}
.temp {
  margin-bottom: 20px;
}

.weather-temp {
  font-size: 3rem;
  font-weight: 600;
}

.text-light {
  color: #ccc;
}
.card-2{
    background-color: #212730;
    border-radius: 20px;
}
h2,p{
    padding: 0 25px 0;
}
.card-details{
    margin-left: 19px;
}
.h1_Left{
    position: absolute;
    bottom: 25px;
    left: 16px;
    font-size: 3vw;
    line-height: 1.2;
}
.h2_Left{
    position: absolute;
    left: 16px;
    font-size: 2vw;
    line-height: 0.5;
}
.h2_Left small{
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
table tr:hover{
    color: red;
}
.change-btn{
  background-image: linear-gradient(to right, cyan, magenta);
}
</style>