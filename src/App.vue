<template>
  <div :class="weather.cod == 200 && weather.main != undefined && weather.main.temp > 16 ? 'app sunny' : 'app' ">
    <main>
      <div class="search-box">
        <input
          type="text"
          class="search-bar"
          placeholder="Search...."
          v-model="search"
          @keyup.enter="getWeather"
        />
      </div>

      <div class="weather-wrap" v-if="weather.main != undefined">
        <div class="location-box">
          <div class="location">
            {{ weather.name }}, {{ weather.sys.country }}
          </div>
          <div class="date">{{ this.dateBuilder() }}</div>
        </div>
        <div class="weather-box">
          <div class="temperature">{{ Math.round(weather.main.temp) }}°c</div>
          <div class="weather">{{ weather.weather[0].main }}</div>
        </div>
      </div>
      <div class="error-box" v-else-if="weather.cod == 404">
        <h1 class="error">{{ weather.message }}</h1>
      </div>
      <div class="no-data-box" v-else>
        <h1 class="no-data">Search...</h1>
      </div>
    </main>
  </div>
</template>


<script>
import axios from "axios";
export default {
  name: "App",
  data() {
    return {
      api_key: "3be062aa03cf0d8643f783d875ff85e9",
      api_url: "https://api.openweathermap.org/data/2.5/",
      search: "",
      weather: {},
    };
  },
  methods: {
    getWeather() {
      fetch(`${this.api_url}weather?q=${this.search}&units=metric&appid=${this.api_key}`)
        .then((res) => {
          return res.json();
        })
        .then(this.setResults);
    },

    setResults(results) {
      this.weather = results;
    },

    dateBuilder() {
      let d = new Date();
      let months = [
        "January",
        "February",
        "March",
        "April",
        "May",
        "June",
        "July",
        "August",
        "September",
        "October",
        "November",
        "December",
      ];
      let days = [
        "Sunday",
        "Monday",
        "Tuesday",
        "Wednesday",
        "Thursday",
        "Friday",
        "Saturday",
      ];
      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();
      return `${day} ${date} ${month} ${year}`;
    },
  },
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: "montserrat", sans-serif;
}

.app {
  background-image: url("./assets/cold.jpg");
  background-size: cover;
  background-position: bottom;
  transition: 0.4s;
}

.app.sunny {
  background-image: url("./assets/sunny.jpg");
}

main {
  min-height: 100vh;
  padding: 30px;

  background-image: linear-gradient(
    to bottom,
    rgba(0, 0, 0, 0.25),
    rgba(0, 0, 0, 0.75)
  );
}

.search-box {
  width: 100%;
  margin-bottom: 30px;
}

.search-box .search-bar {
  display: block;
  width: 100%;
  padding: 15px;

  color: #130e0e;
  font-size: 20px;

  appearance: none;
  border: none;
  outline: none;
  background: none;

  box-shadow: 0px 0px 8px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.5);
  border-radius: 0px 20px 0px 20px;
  transition: 0.4s;
}

.search-box .search-bar:focus {
  box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.75);
  border-radius: 20px 0px 20px 0px;
}

.location-box .location {
  color: #ffffff;
  font-size: 32px;
  font-weight: 500;
  text-align: center;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
  margin-bottom: 10px;
}

.location-box .date {
  color: #ffffff;
  font-size: 20px;
  font-weight: 300;
  text-align: center;
  font-style: italic;
  margin-bottom: 5px;
}

.weather-box {
  text-align: center;
}

.weather-box .temperature {
  display: inline-block;
  padding: 10px 25px;
  color: #fff;
  font-size: 102px;
  font-weight: 300;

  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.3);
  border-radius: 10px 25px 10px 25px;
  margin: 30px 0px;

  box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.3);
}

.weather-box .weather {
  font-size: 48px;
  font-weight: 500;
  font-style: italic;
  color: #fff;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}

.no-data-box,
.error-box {
  text-align: center;
}

.no-data-box .no-data {
  font-size: 48px;
  font-weight: 500;
  color: #fff;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}

.error-box .error {
  display: inline-block;
  padding: 10px 25px;
  color: #fff;
  font-size: 50px;
  font-weight: 300;

  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  background-color: rgba(236, 22, 22, 0.767);
  border-radius: 10px 25px 10px 25px;
  margin: 30px 0px;

  box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.3);
}
</style>
