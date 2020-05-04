<template>
    <main>
        <div class="weather-loading" v-if="typeof position != ''">
            <div class="weather-wrapper" v-if="typeof weather.city != 'undefined'">
                <div class="main-weather">
                    <div v-bind:class = "(weatherList[ targetId ].weather[0].main == 'Rain')?'main-weather-results-rain':'main-weather-results'">
                        <img v-if=" weatherList[ targetId ].weather[0].main == 'Rain' " class="rain-gif" src="../assets/images/rain.gif">
                        <div class="results-description">
                            <div class="location">{{ weather.city.name }}, {{ weather.city.country }}</div>
                            <div class="date">{{ dateFormat(new Date(weatherList[ targetId ].dt * 1000)) }}</div>
                            <div class="main-climate-condition">
                                <img :src="climateCondition(weatherList[targetId].weather[0].main)">
                            </div>
                            <div class="weather-description-box">
                                <div class="description">
                                    <p>Max Temp: {{ convertTemperature(weatherList[targetId].temp.max) }}&#8451;</p>
                                    <p>Min Temp: {{ convertTemperature(weatherList[targetId].temp.min) }}&#8451;</p>
                                    <p>Pressure: {{ weatherList[targetId].pressure }} Pa</p>
                                    <p>Humidity: {{ weatherList[targetId].humidity }} %</p>
                                    <p>Speed: {{ weatherList[targetId].speed }} km/hr</p>
                                    <p class="text">{{ weatherList[targetId].weather[0].description }}</p>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
                <div class="sub-weather-results">
                    <div id="0" v-on:click="select($event)" class="sub-weather-result-one sub-result">
                        <img class="climate-condition" :src="climateCondition(weatherList[0].weather[0].main)">
                        <p>{{ convertTemperature(weatherList[0].temp.max) }}&#8451; / {{ convertTemperature(weatherList[0].temp.min)}}&#8451;</p>
                        <p>{{  dateFormat(new Date(weatherList[0].dt * 1000)) }}</p>
                    </div>
                    <div id="1" v-on:click="select($event)" class="sub-weather-result-two sub-result">
                        <img class="climate-condition" :src="climateCondition(weatherList[1].weather[0].main)">
                        <p>{{ convertTemperature(weatherList[1].temp.max) }}&#8451; / {{ convertTemperature(weatherList[1].temp.min)}}&#8451;</p>
                        <p>{{  dateFormat(new Date(weatherList[1].dt * 1000)) }}</p>
                    </div>
                    <div id="2" v-on:click="select($event)" class="sub-weather-result-three sub-result">
                        <img class="climate-condition" :src="climateCondition(weatherList[2].weather[0].main)">
                        <p>{{ convertTemperature(weatherList[2].temp.max) }}&#8451; / {{ convertTemperature(weatherList[2].temp.min)}}&#8451;</p>
                        <p>{{  dateFormat(new Date(weatherList[2].dt * 1000)) }}</p>
                    </div>
                    <div id="3" v-on:click="select($event)" class="sub-weather-result-four sub-result">
                        <img class="climate-condition" :src="climateCondition(weatherList[3].weather[0].main)">
                        <p>{{ convertTemperature(weatherList[3].temp.max) }}&#8451; / {{ convertTemperature(weatherList[3].temp.min)}}&#8451;</p>
                        <p>{{  dateFormat(new Date(weatherList[3].dt * 1000)) }}</p>
                    </div>
                    <div id="4" v-on:click="select($event)" class="sub-weather-result-five sub-result">
                        <img class="climate-condition" :src="climateCondition(weatherList[4].weather[0].main)">
                        <p>{{ convertTemperature(weatherList[4].temp.max) }}&#8451; / {{ convertTemperature(weatherList[4].temp.min)}}&#8451;</p>
                        <p>{{  dateFormat(new Date(weatherList[4].dt * 1000)) }}</p>
                    </div>
                </div>
            </div>
            <div class="weather-loading" v-else>
                <div class="loading">
                    <img className="icon-load" alt="logo" src="../assets/images/loading.gif">
                    <h2>LOADING...</h2>
                    <p>A few moments, while we fetch your weather data based on your location</p>
                </div>
            </div>
        </div>
    </main>
</template>

<script>
    import moment from 'moment';
    import './style.css';
    import './responsive.css';
    export default {
        name: 'Weather',
        data() {
            return {
                api_key:'5f27e6de016a45e5d58cf3cf874bd3be',
                url_base:'https://api.openweathermap.org/data/2.5/',
                position:'',
                weather: {},
                weatherList:{},
                targetId: 0
            }
        },
        mounted: function() {
            if(navigator.geolocation){
                navigator.geolocation.getCurrentPosition(position => {
                    this.position = position.coords;
                    this.fetchWeather();
                })
            }
        },
        methods: {
            fetchWeather () {
                fetch(`${this.url_base}forecast/daily?cnt=5&lat=${this.position.latitude}&lon=${this.position.longitude}&appid=${this.api_key}`)
                .then(res => {
                    return res.json();
                }).then(this.setResults);
            },
            setResults (results) {
                this.weather = results;
                this.weatherList = results.list;
            },
            select: function(event) {
                this.targetId = event.currentTarget.id;
            },
            dateFormat: function (date) {
                return moment(date).format('dddd, MMMM Do YYYY');
            },
            convertTemperature: function (date) {
                return Math.round(date - 273.15);
            },
            climateCondition: function (condition) {
                if(condition == "Rain") {
                    return require('../assets/images/rainy.gif')
                }
                if(condition == "Clouds") {
                    return require('../assets/images/clouds.gif')
                }
                return require('../assets/images/sun.gif')
            }
        }
    }
</script>
