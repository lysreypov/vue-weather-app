<template>
  <div class="days-tab text-center">
    <div v-if="loading" class="loading">Loading...</div>
    <ul class="p-0">
      <li v-for="day in forecast" :key="day.date" class="li_active">
        <div class="py-3"><img :src="day.iconUrl" /></div>
        <div class="py-3">{{ getDayName(day.date) }}</div>
        <div class="py-3">{{ day.temperature }}&deg;C</div>
      </li>
    </ul>
  </div>
</template>

<script>
import axios from "axios";
import moment from "moment";
export default {
  name: "DaysWeather",
  props: {
    cityname: String,
  },
  data() {
    return {
      forecast: [],
      loading: true,
      iconUrl: null,
    };
  },
  mounted() {
    this.fetchWeatherData();
  },
  methods: {
    async fetchWeatherData() {
      const apiKey = "3f4740f3d44cc357674689359cc8ae59";
      const apiURL = `https://api.openweathermap.org/data/2.5/forecast?q=${this.cityname}&units=metric&appid=${apiKey}`;

      await axios
        .get(apiURL)
        .then((response) => {
          const forecastData = response.data.list;
          const filterData = forecastData
            .map((item) => {
              return {
                date: moment(item.dt_txt.split(" ")[0]),
                temperature: Math.round(item.main.temp),
                description: item.weather[0].description,
                iconUrl: `https://api.openweathermap.org/img/w/${item.weather[0].icon}.png`,
              };
            })
            .reduce((acc, item) => {
              if (!acc.some((day) => day.date.isSame(item.date, "day"))) {
                acc.push(item);
              }
              return acc;
            }, [])
            .slice(1, 5);
          console.log("working", filterData);
          this.forecast = filterData;
          this.loading = false;
        })
        .catch((error) => {
          console.log("Error fetching weather data: ", error);
          this.loading = false;
        });
    },

    getDayName(date) {
      return date.format("ddd");
    },
  },
};
</script>

<style>
.days-tab {
  width: 90%;
  box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19);
  border-radius: 20px;
  width: 90%;
  margin: auto;
}
.loading {
  color: #fff;
}
ul {
  margin: 0;
}
li {
  display: inline-block;
  list-style: none;
  height: 100%;
  width: 21%;
  max-width: 21%;
  font-size: 1vm;
  line-height: 1.2;
}
span {
  display: block;
  margin-bottom: 5px;
  font: 100% sans-serif;
  height: 35px;
}
.li_active {
  background: #253d5c;
  color: #222831;
  border-radius: 10px;
  margin: 0.5rem;
  color: #fff;
}
.li_active:hover {
  transform: scale(1.2);
  transition: transform 0.1s ease;
}
.li_active_temp {
  display: inline-block;
  background-color: #222831;
  color: #ffffff;
  transition: background-color 0.5s;
  border-radius: 10px;
}
.li_active_temp:hover {
  transform: scale(1.2);
  transition: transform 0.1s ease;
  background: #fff;
  border-radius: 10px;
  color: #191a1f;
}
</style>
