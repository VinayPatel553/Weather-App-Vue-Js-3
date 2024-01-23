<template>
    <div class="div">
    <div class="wrapper">
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
    </div>
    <br>
         <WeatherComponent :city="city" v-if="showWeather"/>

         <footer class="text-center fs-5 footer">
            <div class="text-center p-3">
                Made By:
                <a class="text-white" target="_blank" href="https://vinay-portfolio-pi.vercel.app/">Vinay Patel</a>
            </div>
        </footer>
    </div>
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
                if(this.city.length<=0){
                    document.addEventListener("click", function(){ alert("Please enter a city name"); });
                }
                else{
                    this.showWeather = false;
                    await this.$nextTick();
                    this.showWeather = true
                }
                
            }
        },
    }
</script>

<style scoped>
.div {
  display: flex;
  flex-direction: column;
  min-height: 100vh;
}
.wrapper {
  flex: 1; 
  display: flex;
  flex-direction: column;
}

.footer {
  background-color: #212730;
  margin-top: auto; 
  color: white;
}
</style>
