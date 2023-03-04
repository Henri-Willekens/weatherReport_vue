<template>
    <div class="container">
        <div class="inner-container">
            <h3>Today:</h3>
            <h2>{{ temperature }} <label for="toggle_button">
                    <span>&#176;{{ isMetric ? "C" : "F" }}</span>

                    <input type="checkbox" id="toggle_button" v-model="checkedValue">
                </label></h2>
            <p>{{ weatherDescription }}</p>

            <div class="weather-condition">
                <img v-if="showCloudy" src="../assets/cloudy.png" alt="cloudy">
                <img v-else-if="showRainy" src="../assets/rain.png" alt="rain">
                <img v-else-if="showStorm" src="../assets/thunderstorm.png" alt="thunderstorm">
                <img v-else-if="showClear" src="../assets/clear.png" alt="clear">
                <img v-else-if="showSnow" src="../assets/snow.png" alt="snow">
                <img v-else-if="showDrizzle" src="../assets/drizzle.png" alt="drizzle">
            </div>

            <div class="input-container">
                <label for="city">City: </label>
                <input type="text" name="city" v-model="city">
            </div>
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
            weatherDescription: '',
            temperature: '',
            showCloudy: false,
            showRainy: false,
            showStorm: false,
            showDrizzle: false,
            showClear: false,
            showSnow: false,
            ForweatherDescription: '',
            Fortemperature: '',
            city: 'Amsterdam',
            Forcity: '',
            isMetric: true
        }
    },
    computed: {
        checkedValue: {
            get() {
                return this.isMetric
            },
            set(newValue) {
                this.isMetric = newValue
            }
        },
        unit: function () {
            return this.isMetric ? 'metric' : 'imperial'
        }
    },
    watch: {
        city() {
            this.getWeatherAndForecastData()
        },
        unit() {
            this.getWeatherAndForecastData()
        },
    },
    methods: {
        // https://openweathermap.org/weather-conditions
        getWeatherAndForecastData() {
            axios.get('https://api.openweathermap.org/data/2.5/weather?q=' + this.city + '&APPID=94e38cae9ff71a42769f2ff6499340d7&units=' + this.unit).then(
                response => {
                    console.log(this.unit)
                    // console.log(response.data.weather[0].main)
                    let mainDescription = response.data.weather[0].main;
                    let descriptionString = response.data.weather[0].description;

                    this.weatherDescription = descriptionString.charAt(0).toUpperCase() + descriptionString.slice(1);
                    this.temperature = response.data.main.temp.toFixed(1);

                    this.showClear = mainDescription == "Clear"
                    this.showCloudy = mainDescription == "Clouds"
                    this.showRainy = mainDescription == "Rain"
                    this.showDrizzle = mainDescription == "Mist"
                    this.showSnow = mainDescription == "Snow"
                    this.showStorm = mainDescription == "Thunderstorm"
                }
            ).catch(error => { console.log(error) })

            axios.get('https://api.openweathermap.org/data/2.5/forecast?q=' + this.city + '&APPID=94e38cae9ff71a42769f2ff6499340d7&units=' + this.unit).then(
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
        this.getWeatherAndForecastData()
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

.weather-condition {
    text-align: center;
    margin: auto;
}

#container {
    text-align: left;
    display: flex;
    flex-wrap: wrap;
}

.toggle__button {
    vertical-align: middle;
    user-select: none;
    cursor: pointer;
}

.toggle__button input[type="checkbox"] {
    opacity: 0;
    position: absolute;
    width: 1px;
    height: 1px;
}
</style>