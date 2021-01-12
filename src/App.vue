<template>
  <div id="app" :class="weather.main && weather.main.temp > 16 ? 'warm' : ''">
    <div class="container">
      <!-- search bar  -->
      <div class="search-box">
        <input
          type="text"
          placeholder="請輸入縣市名稱(英文)"
          class="search-bar"
          v-model="query"
          @keyup.enter="fetchWeather"
        />
      </div>
      <div class="weather-wrapper" v-if="weather.main">
        <!-- location & date info -->
        <div class="location-box">
          <div class="location">{{ weather.name }}</div>
          <div class="date">{{ currentDate }}</div>
        </div>
        <!-- weather info -->
        <div class="weather-box">
          <div class="temperature">{{ Math.round(weather.main.temp) }}°C</div>
          <div class="weather">{{ weather.weather[0].main }}</div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import dayjs from "dayjs";
import advancedFormat from "dayjs/plugin/advancedFormat";
dayjs.extend(advancedFormat);

export default {
  name: "app",
  data() {
    return {
      api_key: process.env.VUE_APP_WEATHER_KEY,
      base_url: "https://api.openweathermap.org/data/2.5/",
      query: "Taichung",
      weather: {},
      date: "",
    };
  },
  methods: {
    async fetchWeather() {
      const data = await fetch(
        `${this.base_url}weather?q=${this.query}&units=metric&appid=${this.api_key}`
      );
      // console.log(await data.json());
      this.weather = await data.json();
    },
  },
  computed: {
    currentDate() {
      return dayjs().format(`MMMM Do YYYY`);
    },
  },

  created() {
    this.fetchWeather();
  },
};
</script>

<style>
/* Reset */
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

#app {
  background-image: url("./assets/cold.jpg");
  background-size: cover;
  background-position: 50%;
}

#app.warm {
  background-image: url("./assets/warm.jpg");
}

.container {
  height: 100vh;
  padding: 25px;
}
.search-box {
  width: 100%;
  margin-bottom: 35px;
}

.search-box .search-bar {
  display: block;
  width: 100%;
  padding: 15px;
  border-radius: 0 10px 0 10px;
  font-size: 15px;
  border: none;
  outline: none;
}

.location-box .location {
  color: black;
  font-size: 30px;
  font-weight: bold;
  text-align: center;
  margin-bottom: 10px;
}
.location-box .date {
  color: black;
  font-size: 20px;
  font-weight: bold;
  text-align: center;
  font-style: italic;
}

.weather-box {
  text-align: center;
  margin-top: 15px;
}

.weather-box .temperature {
  display: inline-block;
  padding: 10px 20px;
  color: black;
  font-size: 80px;
  font-weight: bold;
  background-color: rgba(255, 255, 255, 0.25);
  border-radius: 15px;
  box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}

.weather-box .weather {
  margin-top: 10px;
  font-size: 40px;
  color: black;
  font-weight: bold;
  font-style: italic;
}
</style>