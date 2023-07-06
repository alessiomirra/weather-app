<template>
    <div class="container main mt-2">
        
        <div class="text-center mt-5">
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
                <p>Check the city name</p>
            </div>
        </div>

        <div v-if="data && errors == false" class="mt-3 text-center">
            <h3>{{ data.name }}, {{ data.sys.country }}</h3>
            <img :src="iconUrl" alt="#">
            <h5>{{ data.weather[0].description }}</h5>
            <p class="text-muted">Temperature: {{ data.main.temp }}</p>
        </div>

    </div>
</template>

<script>
import axios from 'axios';

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
        }
    }, 
    methods: {
        getWeatherData(){
            this.loading = true;
            this.errors = false; 
            const API_KEY = 'e9557e52931333efd6702b6532134e35'; 
            const api_url = `https://api.openweathermap.org/data/2.5/weather?q=${this.city}&appid=${API_KEY}&units=metric`;

            axios 
                .get(api_url)
                .then(response => {
                    this.data = response.data; 
                    this.iconUrl = `https://openweathermap.org/img/wn/${response.data.weather[0].icon}@4x.png`; 
                    this.loading = false; 
                    this.showForm = false; 
                })
                .catch(error => {
                    this.errors = true; 
                    this.loading = false; 

                })
        }
    }, 
    created(){
        this.getWeatherData(); 
    }
})
</script>

<style scoped>
.main{
    width: 100%;
    height: 100vh;
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