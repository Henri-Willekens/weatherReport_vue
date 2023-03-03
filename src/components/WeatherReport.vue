<template>
    <div class="container">
        <div class="inner-container">
            <h3>Today:</h3>
            <h2>{{ temperature }}<span>&#176;C</span></h2>
            <p>{{ weatherDescription }}</p>

            <div v-if="showCloudy" class="weather-condition">
                <img src="../assets/cloudy.png" alt="cloudy">
            </div>
            <div v-if="showRainy" class="weather-condition">
                <img src="../assets/rain.png" alt="rainy">
            </div>
            <div v-if="showStorm" class="weather-condition">
                <img src="../assets/thunderstorm.png" alt="cloudy">
            </div>
            <div v-if="showClear" class="weather-condition">
                <img src="../assets/clear.png" alt="Clear sky">
            </div>
            <div v-if="showDrizzle" class="weather-condition">
                <img src="../assets/drizzle.png" alt="Clear sky">
            </div>
            <div v-if="showSnow" class="weather-condition">
                <img src="../assets/snow.png" alt="Clear sky">
            </div>

            <div class="input-container">
                <label for="city">City: </label>
                <input type="text" name="city" v-model="city">
            </div>
            <button @click="getWeatherData(); getForecastData();">Get Data</button>
        </div>

    </div>
    <div class="container">
        <div class="inner-container">
            <h3>Weather:</h3>
            <table class="weather-condition">
                <thead>
                    <tr>
                        <th scope="col">City</th>
                        <th scope="col">Weather today</th>
                        <th scope="col">Temperature today</th>
                        <th scope="col">Weather 24 H</th>
                        <th scope="col">Temperature 24 H</th>
                    </tr>
                </thead>
                <tbody>
                    <tr>
                        <th scope="row"> </th>
                        <td> {{ Forcity }}</td>
                        <td> {{ weatherDescription }}</td>
                        <td> {{ temperature }}</td>
                        <td> {{ ForweatherDescription }}</td>
                        <td> {{ Fortemperature }}</td>
                    </tr>
                </tbody>
            </table>
        </div>

    </div>
</template>

<script>
import axios from 'axios'
export default {
    data() {
        return {
            weather: {},
            weatherDescription: '',
            temperature: '',
            showCloudy: false,
            showRainy: false,
            showStorm: false,
            showDrizzle: false,
            showClear: false,
            showSnow: false,
            weatherImage: '',
            ForweatherDescription: '',
            Fortemperature: '',
            city: 'Amsterdam',
            Forcity: ''
        }
    },
    methods: {
        // https://openweathermap.org/weather-conditions
        getWeatherData() {
            axios.get('https://api.openweathermap.org/data/2.5/weather?q=' + this.city + '&APPID=94e38cae9ff71a42769f2ff6499340d7&units=metric').then(
                response => {
                    // console.log(response.data.weather[0].main)
                    let mainDescription = response.data.weather[0].main;
                    let descriptionString = response.data.weather[0].description;
                    this.weatherDescription = descriptionString.charAt(0).toUpperCase() + descriptionString.slice(1);
                    this.temperature = response.data.main.temp.toFixed(1);

                    this.showCloudy = mainDescription == 'Clouds'
                    this.showRainy = mainDescription == 'Rain'
                    this.showClear = mainDescription == 'Clear'
                    this.showDrizzle = mainDescription == 'Mist'
                    this.showSnow = mainDescription == 'Snow'
                    this.showStorm = mainDescription == 'Thunderstorm'
                }
            ).catch(error => { console.log(error) })
        },
        getForecastData() {
            axios.get('https://api.openweathermap.org/data/2.5/forecast?q=' + this.city + '&APPID=94e38cae9ff71a42769f2ff6499340d7&units=metric').then(
                response => {
                    // console.log(response.data)
                    let FordescriptionString = response.data.list[8].weather[0].description;
                    this.ForweatherDescription = FordescriptionString.charAt(0).toUpperCase() + FordescriptionString.slice(1);
                    this.Fortemperature = response.data.list[8].main.temp;
                    this.Forcity = this.city;
                }
            ).catch(error => { console.log(error) })
        }
    },

    mounted() {
        this.getWeatherData();
        this.getForecastData();
    }
}
</script>

<style>
.container {
    width: 100%;
    display: flex;
    justify-content: center;
}

.inner-container {
    width: 50%;
    background-color: rgb(142, 142, 196);
}
</style>