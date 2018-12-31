<template>
  <div class="weather-data-block">
    <div class="time-city">
      <div class="time">{{getFormattedDate}}</div>
      <div class="city">{{`${this.weatherData.name}, ${this.weatherData.country}.`}}</div>
    </div>
    <div class="main-block">
      <img
        class="weather-icon"
        :src="`http://openweathermap.org/img/w/${this.weatherData.icon}.png`"
        alt="weather_icon"
      >
      <span class="weather-info">
        <div class="description">{{capitalizeFirstLetters(this.weatherData.description)}}</div>
        <span class="temperature-block">
          <span v-if="isCelcius">{{Math.floor(this.weatherData.temp)}}</span>
          <span v-else>{{celciusToFahrenheit(this.weatherData.temp)}}</span>
          <span class="active">{{`&deg;${isCelcius ? "C" : "F"}`}}</span>
          <i class="v-line"></i>
          <span v-on:click="toggleUnit()">{{`&deg;${isCelcius ? "F" : "C"}`}}</span>
        </span>
      </span>
    </div>
    <div class="other-block">
      <p>{{`Wind Speed: ${this.weatherData.wind}mps`}}</p>
      <p>{{`Humidity: ${this.weatherData.humidity}%`}}</p>
    </div>
  </div>
</template>
<script>
export default {
  name: "WeatherDataBlock",
  data() {
    return {
      isCelcius: true
    };
  },
  props: {
    weatherData: {
      type: Object,
      default: function() {
        return {
          main: {
            temp: null
          },
          weather: []
        };
      }
    }
  },
  computed: {
    getFormattedDate: function() {
      const dateObject = new Date(this.weatherData.date * 1000);
      const options = {
        weekday: "long",
        year: "numeric",
        month: "short",
        day: "numeric",
        hour: "2-digit",
        minute: "2-digit"
      };
      return dateObject.toLocaleString("en-US", options);
    }
  },
  methods: {
    celciusToFahrenheit(value) {
      return Math.floor((value * 9) / 5 + 32);
    },
    toggleUnit() {
      this.isCelcius = !this.isCelcius;
    },
    capitalizeFirstLetters(string) {
      return string
        .split(" ")
        .reduce(
          (string, item) =>
            string + " " + item.charAt(0).toUpperCase() + item.slice(1),
          ""
        );
    }
  }
};
</script>
<style lang="scss" src="@/styles/weatherDataBlock.scss">
</style>
