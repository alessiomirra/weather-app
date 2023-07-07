<template>
    <div class="container main">
        
        <div class="text-center mt-3">
            <button class="btn btn-danger" @click="showForm=!showForm">Change City</button>
        </div>
        <div v-if="showForm" class="form-container d-flex justify-content-center">
            <div class="border rounded-2">
                <form class="mt-4 d-flex p-2" @submit.prevent="getWeatherData">
                    <input type="text" name="city" id="city" v-model="city" class="form-control" placeholder="City">
                    <button type="submit" class="btn btn-danger ms-2">Submit</button>
                </form>
            </div>
        </div>

        <div v-if="loading" class="mt-3 text-center">
            <div class="spinner-border text-primary" role="status">
                <span class="visually-hidden">Loading...</span>
            </div>
        </div>

        <div v-if="errors == true" class="mt-3">
            <div class="alert alert-danger text-center" role="alert">
                <p>Network Errors</p>
                <p>Check city name</p>
            </div>
        </div>

        <div v-if="data && errors == false" class="mt-3 text-center">
            <h3>{{ data.name }}, {{ data.sys.country }}</h3>
            <img :src="iconUrl" alt="#">
            <h5 class="text-uppercase">{{ data.weather[0].description }}</h5>
            <p class="text-muted">Temperature: {{ data.main.temp }}°C</p>

            <!-- Table -->
            <div class="border border-secondary p-2 rounded-5">
                <div class="row mt-2">
                    <div class="col-md-4">
                        <h6>Humidity</h6>
                        <p>{{ data.main.humidity }}%</p>
                    </div>
                    <div class="col-md-4">
                        <h6>Temperature Max</h6>
                        <p>{{ data.main.temp_max }}°C</p>
                    </div>
                    <div class="col-md-4">
                        <h6>Temperature Min</h6>
                        <p>{{ data.main.temp_min }}°C</p>
                    </div>
                </div>
                <div class="row">
                    <div class="col-md-4">
                        <h6>Pressure</h6>
                        <p>{{ data.main.pressure }} mBar</p>
                    </div>
                    <div class="col-md-4">
                        <h6>Sunrise</h6>
                        <p>{{ sunrise }}</p>
                    </div>
                    <div class="col-md-4">
                        <h6>Sunset</h6>
                        <p>{{ sunset }}</p>
                    </div>
                </div>
            </div>
        </div>

    </div>
</template>

<script>
import axios from 'axios';
import {keys} from '@/keys.js'

export default({
    name: 'MainView', 
    data(){
        return{
            showForm: false, 
            loading: false, 
            errors: false, 
            city: 'Roma, IT', 
            data: '',
            iconUrl: '', 
            sunrise: '', 
            sunset: '', 
        }
    }, 
    methods: {
        getWeatherData(){
            this.loading = true;
            this.errors = false; 
            const API_KEY = keys.WEATHER_API_KEY;  
            const api_url = `https://api.openweathermap.org/data/2.5/weather?q=${this.city}&appid=${API_KEY}&units=metric`;

            axios 
                .get(api_url)
                .then(response => { 
                    this.data = response.data; 
                    this.iconUrl = `https://openweathermap.org/img/wn/${response.data.weather[0].icon}@4x.png`; 
                    this.sunrise = this.getTime(response.data.sys.sunrise);
                    this.sunset = this.getTime(response.data.sys.sunset);
                    this.loading = false; 
                    this.showForm = false; 
                })
                .catch(error => {
                    this.errors = true; 
                    this.loading = false; 

                })
        }, 
        getTime(timestamp){
            let date = new Date(timestamp * 1000); 

            let hours = date.getHours();
            let minutes = date.getMinutes();
            
            let formattedTime = hours.toString().padStart(2, '0') + ':' + minutes.toString().padStart(2, '0')

            return formattedTime
        }, 
    }, 
    created(){
        this.getWeatherData(); 
    }
})
</script>

<style scoped>
.main{
    width: 100%;
    min-height: 100vh;
}
.form-container{
    width: 100%;
    height: 100px;
}
.form-container div{
    width: 50%;
    height: 100%;
}
</style>