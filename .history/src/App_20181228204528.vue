<template>
  <div class="root-container">
    <div class="overlay"></div>
    <div class="title">
      <span>weather</span>
      <span>in your city</span>
    </div>
    <div class="search-container">
      <Search
        v-bind:message="message"
        v-bind:showLoading="isLoadingResults"
        v-bind:isError="isError"
        v-on:on-key-press="onClickSearch"
      />
      <transition name="ease">
        <div class="results-container" v-if="showResults">
          <WeatherDataBlock v-bind:weatherData="weatherData"/>
        </div>
      </transition>
    </div>
  </div>
</template>

<script>
import Search from "./components/Search.vue";
import WeatherDataBlock from "./components/WeatherDataBlock.vue";
import axios from "axios";

export default {
  name: "app",
  components: {
    Search,
    WeatherDataBlock
  },
  data() {
    return {
      currentQueryValue: null,
      results: [],
      showResults: false,
      address: null,
      isError: false,
      message: null,
      searchQuery: null,
      isLoadingResults: false,
      units: "metric",
      weatherData: undefined
    };
  },
  watch: {
    searchQuery(newQuery) {
      let storedWeatherData;
      
      // Check if anything is saved in localStorage
      if (window.localStorage.getItem("searches")) {
        // Now, check if the current query matches anything in localStorage
        storedWeatherData = this.getFromLocalStorage(newQuery);
      } else {
        // Create an empty object in localStorage
        window.localStorage.setItem("searches", "");
      }
      if (storedWeatherData) {
        // Translate localStorage data into the format needed to display the data
        this.weatherData = this.parseStoredWeatherData(storedWeatherData);
      } else {
        // Nothing relevant in localStorage, so make a call to the API
        let weatherData = this.fetchWeatherDataForCity(this.searchQuery);
        if(weatherData) {
          this.storeDataInLocalStorage(newQuery, weatherData);
          this.weatherData = weatherData;
        } else {
          this.isError = true;
        }
      }
    },
    weatherData() {
      this.showResults = true;
    }
  },
  methods: {
    storeDataInLocalStorage(query, data) {
      let object = {
           query,
   result: {
      id: data.id
      temp: 
      name: data.name
      country: this.weatherData.sys.country,
      icon: this.weatherData.weather[0].icon,
      description: this.weatherData.weather[0].description,
      date: this.weatherData.dt
      humidity:
      wind:  
  }
      }
    },
    parseStoredWeatherData(data) {
      return {
        name: data.name,
        sys: {
          country: data.country
        },
        main: {
          humidity: data.humidity
        },
        wind: {
          speed: data.wind
        },
        weather: {
          icon: data.icon,
          description: data.description
        }
      };
    },
    getFromLocalStorage(i) {
      const localStorage = window.localStorage;
      const searchObject = JSON.parse(localStorage.getItem("searches"));
      if (searchObject) {
        return searchObject.find(item => item.query === i);
      } else {
        return null;
      }
    },
    onClickSearch(e) {
      if (e.key === "Enter") {
        this.isLoadingResults = true;
        this.searchQuery = e.target.value;
      }
    },
    fetchWeatherDataForCity(city) {
      axios
        .get(
          `http://api.openweathermap.org/data/2.5/weather?q=${city},in&APPID=7c519880689160b16f5d233f16f2868d${
            this.units === "metric" ? "&units=metric" : ""
          }`
        )
        .then(response => {
          this.isLoadingResults = false;
          if (response.status === 404) {
            return null;
          } else {
            return response.data;
          }
        });
    },
  }
};
</script>

<style lang="scss" src="@/styles/app.scss">
</style>
