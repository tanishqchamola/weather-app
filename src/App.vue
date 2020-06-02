<template>
  <div id="app" :class="typeof weather.main != 'undefined' && weather.main.temp > 25 ? 'warm' : ''">
    <main>
      <div class="search-box">
        <input
          type="text"
          class="search-bar"
          placeholder="Type a City"
          v-model="query"
          v-on:keypress="fetchWeather"
        />
      </div>

      <div class="weather-wrap" v-if="typeof weather.main !='undefined'">
        <div class="location-box">
          <div class="location">{{ weather.name }}, {{weather.sys.country}}</div>
          <div class="date">{{ date() }}</div>
        </div>

        <div class="weather-box">
          <div class="temp">{{ Math.round(weather.main.temp) }}°C</div>
          <div class="weather">{{ weather.weather[0].main }}</div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>
import moment from "moment";

export default {
  name: "App",
  data() {
    return {
      api_key: "73758b8a0ff57a773d57d698610a8eb5",
      url_base: "https://api.openweathermap.org/data/2.5",
      query: "",
      weather: {}
    };
  },
  methods: {
    fetchWeather(e) {
      if (e.key == "Enter") {
        fetch(
          `${this.url_base}/weather?q=${this.query}&units=metric&appid=${this.api_key}`
        )
          .then(res => {
            return res.json();
          })
          .then(this.setResults);
      }
    },
    setResults(results) {
      this.weather = results;
    },
    date() {
      return moment().format("dddd, MMMM DD YYYY");
    }
  }
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  max-width: 500px;
  margin: auto;
}

#app {
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  background-image: url("./assets/cold-bg.jpg");
  background-size: cover;
  background-position: bottom;
  transition: 1s;
}

#app.warm {
  background-image: url("./assets/warm-bg.jpg");
}

main {
  min-height: 100vh;
  padding: 25px;
  background-image: linear-gradient(
    to bottom,
    rgba(0, 0, 0, 0, 25),
    rgba(0, 0, 0, 0.75)
  );
}

.search-box {
  width: 95%;
  margin: auto;
  margin-bottom: 30px;
}

.search-box .search-bar {
  display: block;
  width: 100%;
  padding: 15px;

  color: #212121;
  font-size: 20px;
  text-align: center;

  appearance: none;
  border: none;
  outline: none;
  background: none;

  box-shadow: 0px 0px 8px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.5);
  border-radius: 8px;
  transition: 0.4s;
}

.search-box .search-bar:focus {
  background-color: rgba(255, 255, 255, 0.75);
}

.location-box .location {
  color: #343a40;
  font-size: 32px;
  font-weight: 500;
  text-align: center;
}

.location-box .date {
  color: #6c757d;
  margin-top: 15px;
  font-size: 20px;
  font-weight: 500;
  text-align: center;
}

.weather-box {
  text-align: center;
}

.weather-box .temp {
  display: inline-block;
  padding: 10px 25px;
  color: #fff;
  font-size: 102px;
  font-weight: 900;

  background-color: rgba(255, 255, 255, 0.25);
  backdrop-filter: blur(1px);
  border-radius: 10px;
  margin: 30px 0px;
}

.weather-box .weather {
  color: #fff;
  font-size: 48px;
  font-weight: 600;
}
</style>
