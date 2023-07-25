<template>
  <h1 class="white">Pogoda najbliże 7dni🌤️</h1>
  <div id="container">
    <div class="weather-info today">
      <h1>Dziś</h1>
      <p v-if="weatherDataNow">
        <b class="icon"> {{ weatherIcon[weatherDataNow.weather[0].main] }} </b><br>
        Lokalizacja: {{ weatherDataNow.name }}<sup>{{ weatherDataNow.sys.country }}</sup><br>
        Temperatura: {{ weatherDataNow.main.temp }}°C<br>
        Wilgotność: {{ weatherDataNow.main.humidity }}%<br>
        Ciśnienie: {{ weatherDataNow.main.pressure }}hPa<br>
        Wiatr: {{ weatherDataNow.wind.speed }}km/h
      </p>
      <p v-else>
        Ładowanie danych pogodowych...
      </p>
    </div>
    <div id="week">
      <template v-if="weatherData && weatherData.length">
        <div v-for="(data, index) in weatherData" :key="index">
          <div class="weather-info">
            <b class="icon">{{ weatherIcon[data.weather[0].main] }}</b>
            Temperatura: {{ data.main.temp }}°C<br>
            Wilgotność: {{ data.main.humidity }}%<br>
            Ciśnienie: {{ data.main.pressure }}hPa<br>
            Wiatr: {{ data.wind.speed }}km/h
          </div>
        </div>
      </template>
      <p v-else>
        Ładowanie danych pogodowych...
      </p>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
import APIKey from './api-key';

export default {
  name: 'App',
  data() {
    return {
      weatherIcon: {
        Clear: '☀️',
        Clouds: '🌥️',
        Rain: '🌧️',
        Thunderstorm: '⛈️',
        Snow: '🌨️'
      },
      weatherData: null,
      weatherDataNow: null,
    };
  },
  mounted() {
    this.getWeatherDataNow();
    this.getWeatherData();
  },
  methods: {
    getWeatherDataNow() {
      axios.get('https://api.openweathermap.org/data/2.5/weather?q=Wrocław,pl&appid=' + APIKey.key + '&units=metric')
        .then((response) => {
          this.weatherDataNow = response.data;
        })
        .catch((error) => {
          console.error(error);
        });
    },
    getWeatherData() {
      axios.get('https://api.openweathermap.org/data/2.5/forecast?q=Wrocław,pl&appid=' + APIKey.key + '&cnt=6&units=metric')
        .then((response) => {
          console.log(response.data)
          this.weatherData = response.data.list;
        })
        .catch((error) => {
          console.error(error);
        });
    },
  },
}
</script>

<style>
body {
  font-family: 'Verdana';
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  margin-top: 60px;
  background-image: url('assets/rain.jpg');
}

.white {
  color: white;
}

.icon {
  font-size: 4.5rem;
}

p {
  font-size: 1em;
}

.weather-info {

  grid-row: 2;
  grid-column: 1;
  float: left;
  transition: font-size 1s;
  color: white;
  grid-row: 1;
}

.today {
  grid-column: 1;
}

.weather-info:hover {
  cursor: pointer;
  background-color: rgba(255, 0, 0, 0.5);
  color: rgb(0, 255, 42);
}

#week {
  width: auto;
  display: grid;
  grid-template-columns: repeat(6, 1fr);
  grid-template-rows: 1fr;
}

#container {
  background-color: rgba(0, 0, 0, 0.3);
  height: 300px;
  width: 1200px;
  border-radius: 10px;
  border: solid black 2px;
  display: grid;
  grid-template-columns: 300px auto;
  grid-template-rows: 1fr;
  margin: auto;
}
</style>
