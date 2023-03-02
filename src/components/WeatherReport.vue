<template>
    <div class="container">
        <div class="inner-container">
            <h2>{{ temperature }}<span>&#176;C</span></h2>
            <p>{{ weatherDescription }}</p>

            <div v-if="showCloudy" class="weather-condition">
                <img src="../assets/cloudy.png" alt="cloudy">
            </div>
            <div v-if="showRainy" class="weather-condition">
                <img src="../assets/cloudy.png" alt="cloudy">
            </div>
            <div v-if="showStorm" class="weather-condition">
                <img src="../assets/cloudy.png" alt="cloudy">
            </div>
            <div v-if="showClear" class="weather-condition">
                <img src="../assets/clear.png" alt="Clear sky">
            </div>

            <div class="input-container">
                <label for="city">City: </label>
                <input type="text" name="city" v-model="city">
            </div>
            <button @click="getWeatherData()">Get Data</button>



        </div>
        <h3>Cities:</h3>
        <table class="table">
            <thead>
                <tr>
                    <th scope="col">City</th>
                    <th scope="col">Weather</th>
                    <th scope="col">Temperature</th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="weather in weather" v-bind:key="weather.id">
                    <th scope="row">{{ city }}</th>
                    <td> {{ weather }}</td>
                    <td> {{ temperature }}</td>
                </tr>
            </tbody>
        </table>
    </div> 
</template>

<script>
import axios from 'axios'
export default {
    data(){
        return{
            weather: {},
            weatherDescription: '',
            showCloudy:false,
            showRainy:false,
            showStorm:false,
            showClear:false,
            temperature:'',
            weatherImage:'',
            city: 'Amsterdam'
        }
    }, 
    methods:{
        // https://openweathermap.org/weather-conditions
        getWeatherData(){
            axios.get('https://api.openweathermap.org/data/2.5/weather?q=' + this.city + '&APPID=94e38cae9ff71a42769f2ff6499340d7&units=metric').then(
            response => {
                console.log(response.data)
                let mainDescription = response.data.weather[0].main;
                let descriptionString = response.data.weather[0].description;
                this.weatherDescription = descriptionString.charAt(0).toUpperCase() + descriptionString.slice(1);
                this.temperature = response.data.main.temp.toFixed(1);

                if(mainDescription == 'Clouds'){
                this.showCloudy = true;
                }
                if (mainDescription == 'Rain'){
                this.showRainy = true;
                }
                if (mainDescription == 'Thunderstorm'){
                this.showStorm = true;
                }
                if (mainDescription == 'Clear'){
                this.showClear = true;
                }
            }
        ).catch(error => {console.log(error)})
        }
    },
    mounted(){
        this.getWeatherData();
    }
}
</script>

<style>
.container{
    width: 100%;
    display: flex;
    justify-content: center;
}
.inner-container{
    width: 50%;
    background-color: rgb(142, 142, 196);
}
</style>