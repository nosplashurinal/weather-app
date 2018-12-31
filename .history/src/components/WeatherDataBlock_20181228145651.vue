<template>
  <div class="weather-data-block">
    <div class="time-city">
      <div class="time">{{getFormattedDate}}</div>
      <div class="city">{{`${this.weatherData.name}, ${this.weatherData.sys.country}.`}}</div>
    </div>
    <div class="main-block">
      
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
