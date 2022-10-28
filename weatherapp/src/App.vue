<template>
  <div
    id="app"
    :class="
      typeof weather.main != 'undefined' && weather.main.temp > 16 ? 'warm' : ''
    "
  >
    <div class="container">
      <div class="search-box">
        <input
          class="search-bar"
          type="text"
          placeholder="Search..."
          @keyup.enter="axiosWeather"
          v-model="query"
        />
      </div>

      <!-- v-if="typeof weather.main != 'undefined' 判斷有weather.main才render，解決渲染錯誤Error in render: "TypeError: Cannot read property '0' of undefined-->
      <div class="weather-wrapper" v-if="typeof weather.main != 'undefined'">
        <div class="location-box">
          <div class="location">{{ weather.name }}</div>
          <div class="date">{{ currentDate }}</div>
        </div>

        <div class="weather-box">
          <div class="temperature">{{ Math.round(weather.main.temp) }} °C</div>
          <div class="weather">{{ weather.weather[0].main }}</div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
import dayjs from 'dayjs'
import advancedFormat from 'dayjs/plugin/advancedFormat'
dayjs.extend(advancedFormat)
export default {
  name: 'App',
  data () {
    return {
      api_key: '921d70f2858e825af1a3fcce6ed94460',
      base_url: 'https://api.openweathermap.org/data/2.5/',
      query: 'taipei',
      weather: {},
      date: ''
    }
  },
  computed: {
    currentDate () {
      return dayjs().format('MMMM DD YYYY')
    }
  },
  methods: {
    async axiosWeather () {
      await axios
        .get(
          `https://api.openweathermap.org/data/2.5/weather?q=${this.query}&units=metric&appid=${this.api_key}`
        )
        .then((res) => (this.weather = res.data))
    }
  },
  created () {
    this.axiosWeather()
  }
}
</script>

<style lang="scss">
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

html {
  height: 100vh;
  font-family: 'montserrat', sans-serif;
}

#app {
  background-image: url('./assets/cold-bg.jpg');
  background-size: 100% 100%;
}

#app.warm {
  background-image: url('./assets/warm-bg.jpg');
}

.container {
  min-height: 100vh;
  padding: 25px;
  background-image: linear-gradient(
    to bottom,
    rgba(0, 0, 0, 0.25),
    rgba(0, 0, 0, 0.75)
  );
}

.search-box {
  width: 100%;
  margin: 0 auto;
  margin-bottom: 30px;
}

.search-box .search-bar {
  display: block;
  width: 100%;
  padding: 15px;

  color: #313131;
  font-size: 20px;
  outline: none;
  border: none;
  border-radius: 0 16px 0 16px;
  box-shadow: 0px 0px 8px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.5);
}

.search-box .search-bar:focus {
  box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.75);
  border-radius: 16px 0 16px 0;
}

.location-box .location {
  font-size: 32px;
  font-weight: 500;
  color: #fff;
  text-align: center;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
}

.location-box .date {
  font-size: 20px;
  color: #fff;
  font-weight: 300;
  font-style: italic;
  text-align: center;
}

.weather-box {
  text-align: center;
}

.weather-box .temperature {
  display: inline-block;
  padding: 10px 25px;
  font-size: 100px;
  font-weight: 900;
  color: #fff;

  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.25);
  border-radius: 16px;
  margin: 30px 0px;
}

.weather-box .weather {
  color: #fff;
  font-size: 40px;
  font-weight: 700;
  font-style: italic;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}
</style>
