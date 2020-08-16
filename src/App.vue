<template>
  <div id="app" :class="typeof weather.main != 'undefined' && setBg(weather.main.temp) ">
    <main>
      <h1 class="header">Weather App</h1>
      <div class="container">
        <div class="search-box">
          <input
            type="text"
            class="search-bar"
            placeholder="Search City..."
            v-model="query"
            @keyup.enter="getWeather"
          />
        </div>

        <div class="weather-wrapper" v-if="typeof weather.main != 'undefined'">
          <div class="location-box">
            <div class="location">{{ weather.name }}</div>
            <div class="date">{{ setDate() }}</div>
          </div>

          <div class="weather-box">
            <div class="temp">{{ Math.round(weather.main.temp )}}°C</div>
            <div class="weather">{{ weather.weather[0].main }}</div>
          </div>
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
      base_url: process.env.VUE_APP_URL,
      api_key: process.env.VUE_APP_API,
      query: "",
      weather: {},
    };
  },
  methods: {
    getWeather() {
      fetch(
        `${this.base_url}weather?q=${this.query}&units=metric&APPID=${this.api_key}`
      )
        .then((res) => res.json())
        .then(this.setData);
    },
    setData(res) {
      this.weather = res;
    },
    setDate() {
      return moment().format("MMMM Do YYYY");
    },
    setBg(temp) {
      if (temp > 18 && temp < 30) {
        return "warm";
      } else if (temp > 30) {
        return "hot";
      } else {
        return;
      }
    },
  },
};
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Kanit:wght@400;700&display=swap');

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

#app {
  background-image: url("./assets/cold-bg.jpg");
  background-size: cover;
  background-position: bottom;
  transition: 0.4s;
}

#app.warm {
  background-image: url("./assets/warm-bg.jpg");
}

#app.hot {
  background-image: url("./assets/hot-bg.jpg");
}

main {
  min-height: 100vh;
  padding: 25px;

  background-image: linear-gradient(
    to bottom,
    rgba(0, 0, 0, 0.25),
    rgba(0, 0, 0, 0.75)
  );

  font-family: 'Kanit', sans-serif;
  text-align: center;
}

.container {
  display: flex;
  align-items: center;
  flex-direction: column;
}

.header {
  margin: 20px 0;

  color: #fff;
  font-size: 4rem;
  font-weight: 500;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
}

.search-box {
  width: 600px;
  margin: 30px 0;
}

.search-box .search-bar {
  width: 100%;
  padding: 15px;

  appearance: none;
  border: none;
  outline: none;
  background: none;

  box-shadow: 0 0 8px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.5);
  border-radius: 0 16px 0 16px;

  transition: 0.4s;
}

.search-box .search-bar:focus {
  box-shadow: 0 0 16px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.75);
  border-radius: 16px 0px 16px 0px;
}

.location-box .location {
  color: #fff;
  font-size: 3rem;
  font-weight: 500;
  text-align: center;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
}

.location-box .date {
  color: #fff;

  font-size: 2rem;
  font-weight: 200;
  font-style: italic;

  text-align: center;
}

.weather-box {
  text-align: center;
}

.weather-box .temp {
  display: inline-block;
  color: #fff;
  padding: 10px 25px;

  font-size: 5rem;
  font-weight: 500;

  text-align: center;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.25);
  border-radius: 20px;
  margin: 30px 0;

  box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}

.weather-box .weather {
  color: #fff;

  font-size: 2rem;
  font-weight: 200;
  font-style: italic;
  text-align: center;
}
</style>
