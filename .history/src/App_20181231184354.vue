<template>
  <div class="root-container">
    <div class="bg" v-bind:style="{backgroundImage: `url(${bg})`}"></div>
    <div class="overlay"></div>
    <div class="title">
      <span>weather</span>
      <span>in your city</span>
    </div>
    <Login v-bind:isLoginError="isLoginError" v-if="!loginSuccess && !showRegistration" v-on:on-login="attemptLogin" v-on:open-registration="openRegistration" />
    <Registration v-if="showRegistration" v-on:on-success="showRegistration = false" />
    <div v-if="loginSuccess" class="search-container" v-bind:class="{resultActive: showResults}">
      <Search
        v-bind:message="message"
        v-bind:showLoading="isLoadingResults"
        v-bind:isError="isError"
        v-on:on-key-press="onClickSearch"
        v-on:on-change="onInputChange"
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
import Login from "./components/Login.vue";
import Registration from "./components/Registration.vue";
import axios from "axios";

export default {
  name: "app",
  components: {
    Search,
    WeatherDataBlock,
    Login,
    Registration
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
      weatherData: undefined,
      bg: "/images/bg.jpg",
      weatherBg: null,
      screenWidth: window.innerWidth,
      screenHeight: window.innerHeight,
      showRegistration: false,
      loginSuccess: false,
      isLoginError: false
    };
  },
  created: function() {
    if (!JSON.parse(window.localStorage.getItem("user"))) {
      this.showRegistration = true;
    }
  },
  mounted: function() {
    if (JSON.parse(window.localStorage.getItem("searches"))) {
      this.validateRecentSearches();
    }
  },
  watch: {
    async searchQuery(newQuery) {
      let storedWeatherData;
      this.showResults = false;

      // Check if anything is saved in localStorage
      if (window.localStorage.getItem("searches")) {
        // Now, check if the current query matches anything in localStorage
        storedWeatherData = this.getFromLocalStorage(newQuery);
      } else {
        // Create an empty object in localStorage
        window.localStorage.setItem("searches", null);
      }
      if (storedWeatherData) {
        this.isLoadingResults = true;
        // Translate localStorage data into the format needed to display the data
        setTimeout(
          () =>
            (this.weatherData = this.parseStoredWeatherData(
              storedWeatherData.result
            )),
          0.0001
        );
      } else {
        this.isLoadingResults = true;
        try {
          let weatherData = await this.fetchWeatherDataForCity(
            this.searchQuery
          );
          const formattedData = this.parseApiResponse(newQuery, weatherData);
          this.pushDataToLocalStorage(formattedData);
          this.weatherData = formattedData.result;
        } catch {
          this.isLoadingResults = false;
          this.isError = true;
        }
      }
    },
    weatherData() {
      this.isLoadingResults = false;
      this.showResults = true;
      this.changeBackgroundImage();
    }
  },
  methods: {
    async validateRecentSearches() {
      let storedData = window.localStorage.getItem("searches");
      let storedDataObject = JSON.parse(storedData);
      let promiseArray = [];
      let newDataObject = [];
      storedDataObject.forEach(({ result }) => {
        promiseArray.push(this.fetchWeatherDataForCity(result.name));
      });
      let responseArray = await Promise.all(promiseArray);
      responseArray.forEach((data, index) => {
        const formattedData = this.parseApiResponse(
          storedDataObject[index].query,
          data
        );
        newDataObject.push(formattedData);
      });
      window.localStorage.setItem("searches", JSON.stringify(newDataObject));
    },
    timeDifferenceBetwenNowAndDate(d) {
      const date = new Date(d * 1000);
      const now = new Date();
      return (now.getTime() - date.getTime()) / (1000 * 60);
    },
    attemptLogin(e) {
      const user = JSON.parse(window.localStorage.getItem("user"));
      if (user.email === e) {
        this.loginSuccess = true;
      } else {
        this.isLoginError = true;
      }
    },
    openRegistration() {
      this.showRegistration = true;
      this.isLoginError = false;
    },
    onInputChange() {
      if (this.isError) this.isError = false;
    },
    pushDataToLocalStorage(newDataItem) {
      let storedData = window.localStorage.getItem("searches");
      let storedDataObject = JSON.parse(storedData);
      let newDataObject = [];
      if (storedDataObject) {
        newDataObject = [...storedDataObject, newDataItem];
      } else {
        newDataObject.push(newDataItem);
      }
      window.localStorage.setItem("searches", JSON.stringify(newDataObject));
    },
    parseApiResponse(query, data) {
      return {
        query,
        result: {
          id: data.sys.id,
          temp: data.main.temp,
          name: data.name,
          country: data.sys.country,
          icon: data.weather[0].icon,
          description: data.weather[0].description,
          date: data.dt,
          humidity: data.wind.speed,
          wind: data.main.humidity
        }
      };
    },
    parseStoredWeatherData(data) {
      return {
        name: data.name,
        country: data.country,
        humidity: data.humidity,
        temp: data.temp,
        wind: data.wind,
        icon: data.icon,
        description: data.description,
        date: data.date
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
      this.searchQuery = e;
    },
    fetchWeatherDataForCity(city) {
      return new Promise((resolve, reject) => {
        axios
          .get(
            `http://api.openweathermap.org/data/2.5/weather?q=${city}&APPID=7c519880689160b16f5d233f16f2868d${
              this.units === "metric" ? "&units=metric" : ""
            }`
          )
          .then(response => {
            if (response.status === 404) {
              reject(response.err);
            } else {
              resolve(response.data);
            }
          })
          .catch(() => {
            this.isError = true;
            this.isLoadingResults = false;
            return;
          });
      });
    },
    changeBackgroundImage() {
      var newImg = new Image();
      newImg.onload = () => (this.bg = newImg.src);
      newImg.src = `https://loremflickr.com/1920/1080/${
        this.weatherData.description
      }`;
    }
  }
};
</script>

<style lang="scss" src="@/styles/app.scss">
</style>
