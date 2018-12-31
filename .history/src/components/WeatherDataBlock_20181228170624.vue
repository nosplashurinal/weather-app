<template>
  <div class="weather-data-block">
    <div class="time-city">
      <div class="time">{{getFormattedDate}}</div>
      <div class="city">{{`${this.weatherData.name}, ${this.weatherData.sys.country}.`}}</div>
    </div>
    <div class="main-block">
      <img class="weather-icon" :src="`http://openweathermap.org/img/w/${this.weatherData.weather[0].icon}.png`" alt="weather_icon" />
      <span class="weather-info">
        <div class="description">{{capitalizeFirstLetters(this.weatherData.weather[0].description)}}</div>
        <span class="temperature-block">
          <span>{{this.weatherData.main.temp}}</span>
          <span>&deg;C</span>
        </span>
      </span>
    </div>
    <div class="other-block">
      <p>{{`Wind Speed: ${this.weatherData.wind.speed}mps`}}</p>
      <p>{{`Precipitation: ${this.weatherData.precipitation.value}mm`}}</p>
      <p>{{`Humidity: ${this.weatherData.humidity.value}%`}}</p>
    </div>
  </div>
</template>
<script>
export default {
  name: "WeatherDataBlock",
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
      const dateObject = new Date(this.weatherData.dt * 1000);
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
