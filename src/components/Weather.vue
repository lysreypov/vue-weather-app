<template>
  <div class="container p-0">
    <div class="d-flex gap-4">
      <div class="main-div w-100">
        <div class="p-3">
          <h2 class="mb-1 day">Today</h2>
          <p class="text-light date mb-0">{{ date }}</p>
          <small>{{ time }}</small>
          <h2 class="place">
            <i class="fa fa-location"
              >{{ name }} <small>{{ country }}</small></i
            >
          </h2>
          <div class="temp">
            <h2 class="weather-temp">{{ temperature }}&deg;</h2>
            <h2 class="text-light">{{ description }} <img :src="iconUrl" /></h2>
          </div>
        </div>
      </div>
      <div class="card card-2 w-100">
        <table class="m-4">
          <tbody>
            <tr>
              <th>Real Feel</th>
              <td>{{ feels_like }}&deg;</td>
            </tr>
            <tr>
              <th>Humidity</th>
              <td>{{ humidity }}</td>
            </tr>
            <tr>
              <th>Wind</th>
              <td>{{ wind }}</td>
            </tr>
          </tbody>
        </table>
        <DaysWeatherVue :cityname="cityname" />
        <div id="div_Form" class="d-flex m-3 justify-content-center">
          <form action="">
            <input
              type="button"
              value="Change Location"
              @click="changeLocation"
              class="btn change-btn btn-primary"
            />
          </form>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import DaysWeatherVue from "./DaysWeather.vue";

export default {
  name: "myWeather",
  components: {
    DaysWeatherVue,
  },
  props: {
    city: String,
  },
  data() {
    return {
      cityname: this.city,
      temperature: null,
      description: null,
      iconUrl: null,
      date: null,
      name: null,
      time: null,
      feels_like: null,
      wind: null,
      country: null,
      humidity: null,
      monthNames: [
        "January",
        "February",
        "March",
        "April",
        "May",
        "June",
        "July",
        "August",
        "September",
        "November",
        "December",
      ],
    };
  },
  async created() {
    const response = await axios.get(
      `https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&appid=3f4740f3d44cc357674689359cc8ae59`
    );
    const weatherData = response.data;
    this.temperature = Math.round(weatherData.main.temp);
    this.description = weatherData.weather[0].description;
    this.name = weatherData.name;
    this.wind = weatherData.wind.speed;
    this.feels_like = Math.round(weatherData.main.feels_like);
    this.country = weatherData.sys.country;
    this.humidity = weatherData.main.humidity;
    this.iconUrl = `https://api.openweathermap.org/img/w/${weatherData.weather[0].icon}.png`;
    const d = new Date();
    this.date =
      d.getDate() + "-" + this.monthNames[d.getMonth()] + "-" + d.getFullYear();
    this.time = d.getHours() + ":" + d.getMinutes() + ":" + d.getSeconds();
    console.log(weatherData);
  },
  methods: {
    changeLocation() {
      window.location.reload();
    },
  },
};
</script>

<style>
body {
  background-color: #343d4d;
}
.weather-app {
  margin: 0;
  font-weight: 700;
  font-size: 4em;
}
h2.mb-1.day {
  font-size: 3rem;
  font-weight: 400;
}

.weather-temp {
  font-size: 3rem;
  font-weight: 500;
}
.main-div {
  border-radius: 20px;
  color: #fff;
  background-image: url("../assets/view.jpg");
  background-size: cover;
  background-position: center;
  background-color: rgba(0, 0, 0, 0.5);
  background-blend-mode: overlay;
  background-repeat: no-repeat;
}

.temp {
  position: absolute;
  bottom: 0;
}

.main-div:hover {
  transform: scale(1.1);
  transition: transform 0.5s ease;
  z-index: 1;
}
.card-2 {
  background-color: #212730 !important;
  border-radius: 20px !important;
}

.card-details {
  margin-left: 19px;
}
.h1_left {
  position: absolute;
  bottom: 25px;
  left: 16px;
  font-size: 3vw;
  line-height: 1.2;
}
.h3_left {
  position: absolute;
  left: 16px;
  font-size: 2vw;
  line-height: 0.5;
}
.h3_left small {
  font-size: 1rem;
}
table {
  position: relative;
  left: 15px;
  border-collapse: separate;
  border-spacing: 15px;
  width: 85%;
  text-align: left;
  max-width: 600px;
  margin: 0 auto;
}

th,
td {
  font-size: 18px;
  color: #fff;
}
td {
  text-align: right;
}
table,
tr:hover {
  color: red;
}
/* } */
.change-btn:hover {
  transform: scale(1.1);
  transition: transform 0.5s ease;
}
</style>
