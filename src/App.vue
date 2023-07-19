<template>
  <div>
    <div class="weather-info">
      <h1>Pogoda🌤️</h1>
      <p v-if="weatherDataNow">
        Lokalizacja: {{ weatherDataNow.name }}<sup>{{ weatherDataNow.sys.country }}</sup>
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
      <h1>Pogoda najbliże 7dni🌤️</h1>
      <template v-if="weatherData && weatherData.length">
        <div v-for="(data, index) in weatherData" :key="index">
          <div class="weather-info">
            <h1>{{ weatherIcon[data.weather[0].main] }}</h1>
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
      axios.get('https://api.openweathermap.org/data/2.5/forecast?q=Wrocław,pl&appid=' + APIKey.key + '&cnt=7&units=metric')
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
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  margin-top: 60px;
}

h1 {
  font-size: 2em;
}

p {
  font-size: 1em;
}

.weather-info {
  border: 2px black solid;
  width: 10%;
  float: left;
  margin-left: 10px;
}

#week {
  width: 1200px;
}
</style>