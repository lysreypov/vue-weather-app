<template>
  <div class="app">
    <div class="header p-5 h-100 container">
      <h2 class="mb-5">Weather App</h2>
      <div class="d-flex justify-content-center h-100">
        <div class="searchbar w-50 mx-2">
          <input
            type="text"
            class="input form-control py-2"
            v-model="city"
            placeholder="Enter a City"
          />
        </div>
        <button class="btn-search btn btn-primary" @click="searchWeather">
          Search <i class="fas fa-search"></i>
        </button>
      </div>
    </div>
    <br />
    <Weather :city="city" v-if="showWeather"></Weather>
  </div>
</template>

<script>
import Weather from "./components/Weather.vue";

export default {
  name: "App",
  components: {
    Weather,
  },
  data() {
    return {
      city: "",
      showWeather: false,
    };
  },
  methods: {
    async searchWeather() {
      this.showWeather = false;
      await this.$nextTick();
      this.showWeather = true;
    },
  },
};
</script>

<style>
body {
  background-color: #121212 !important;
}
.header {
  background-color: #212730;
  border-radius: 20px;
  color: #fff;
  font-family: "Franklin Gothic Medium", "Arial Narrow", Arial, sans-serif;
  margin-top: 5rem;
  text-align: center;
}
</style>
