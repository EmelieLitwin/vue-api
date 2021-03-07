<template>
  <div class="main-container" v-if="typeof weather.main != 'undefined'">
    <table id="currentWeatherTable">
      <tr>
        <td colspan="2" class="date">{{ getDate() }}</td>
      </tr>
      <tr>
        <td colspan="2" class="location">
          {{ weather.name }}, {{ weather.sys.country }}
        </td>
      </tr>
      <tr>
        <img v-bind:src="iconSrc" />
        <td class="currentTemp">{{ Math.round(weather.main.temp) }}°C</td>
      </tr>
      <tr>
        <td colspan="2" class="currentDescription">
          {{ weather.weather[0].description }}
        </td>
      </tr>
    </table>
  </div>
</template>

<script>
export default {
  name: "Weather",
  data() {
    return {
      api_key: "3709f89e826c2838310e77a773533f2d",
      url: "https://api.openweathermap.org/data/2.5/",
      weather: "",
      iconSrc: "",
    };
  },
  mounted() {
    this.fetchWeather();
  },
  methods: {
    fetchWeather() {
      fetch(
        `${this.url}weather?q=stockholm&units=metric&APPID=${this.api_key}`
      )
        .then((res) => {
          return res.json();
        })
        .then(this.setCallback);
    },
    setCallback(results) {
      this.weather = results;
      this.iconSrc =
        "http://openweathermap.org/img/wn/" +
        results.weather[0].icon +
        "@2x.png";
    },
    getDate() {
      var options = { year: "numeric", month: "short", day: "numeric" };
      var today = new Date();
      return today.toLocaleDateString("en-GB", options);
    },
  },
};
</script>

<style>
body {
  font-family: Verdana, sans-serif;
  display: flex;
  align-items: center;
  flex-direction: column;
  font-size: 24px;
}
.currentDescription {
  text-transform: uppercase;
  font-size: 16px;
}
.main-container {
  color: rgb(233, 247, 252);
  background-color: rgb(0, 0, 94);
  border-radius: 20px;
  box-shadow: 0 27px 55px 0 rgba(0, 0, 0, 0.3),
    0 17px 17px 0 rgba(0, 0, 0, 0.15);
  padding: 30px;
}

.currentTemp,
.date {
  font-size: 40px;
}
</style>
