<template>
    <div class="header container h-100 p-4">
        <h1 class="mb-3"><img src="../assets/weather-logo.png" height="90px" width="150px"></h1>
        <div class="d-flex justify-content-center h-100">
            <div class="searchbar w-50 mx-2">
                <input type="text" class="input form-control" v-model="city" placeholder="Enter a City">
            </div>
            <button class="btn-search btn btn-primary" v-on:click="searchWeather">
                Search
                <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" fill="currentColor" class="bi bi-search" viewBox="0 0 20 20">
                <path d="M11.742 10.344a6.5 6.5 0 1 0-1.397 1.398h-.001q.044.06.098.115l3.85 3.85a1 1 0 0 0 1.415-1.414l-3.85-3.85a1 1 0 0 0-.115-.1zM12 6.5a5.5 5.5 0 1 1-11 0 5.5 5.5 0 0 1 11 0"/>
                </svg>
            </button>
        </div>
    </div>
    <br>
         <WeatherComponent :city="city" v-if="showWeather"/>
</template>

<script>
import WeatherComponent from '../components/WeatherComponent.vue';
    export default {
        name:'HomeComponent',
        components:{
            WeatherComponent,
        },
        data(){
            return{
                city:'',
                showWeather:false
            }
        },
        methods:{
            async searchWeather(){
                this.showWeather = false;
                await this.$nextTick();
                this.showWeather = true
            }
        },
    }
</script>

<style scoped>

</style>