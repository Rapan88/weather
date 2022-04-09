<template>
  <div class="bg-primary border border-2 border-dark m-2">
    <h3 class="text-center">Current weather in you location</h3>
    <h4 class="mc-5"></h4>
  </div>
</template>
<script>
import axios from "axios";
export default {
  data() {
    return {
      httpRequest: "https://www.metaweather.com/api/location/",
      forecast: [],
      coords: "",
      woeid: 0,
    };
  },
  mounted() {
    this.getLocation();
  },
  methods: {
    getWeather(woeid) {
      axios
        .get(this.httpRequest + woeid)
        .then((res) => {
          console.log(res);
          this.refactor(res.data);
        })
        .catch((err) => console.log(err));
    },
    searchWoeid(newCoords) {
      axios
        .get(this.httpRequest + "search", {
          params: {
            lattlong: newCoords.lat + "," + newCoords.long,
          },
        })
        .then((elem) => {
          this.woeid = elem.data[0].woeid;
        })
        .catch((err) => alert(err));
    },
    getLocation() {
      navigator.geolocation.getCurrentPosition((position) => {
        this.coords = {
          lat: position.coords.latitude,
          long: position.coords.longitude,
        };
      });
    },
    refactor(res) {
      res.consolidated_weather.forEach((x) => {
        this.forecast.push({
          temperature: {
            minTemperature: x.min_temp,
            maxTemperature: x.max_temp,
            currTemperature: x.the_temp
          },
          visibility: x.visibility,
          humidity: x.humidity,
          windSpeed: x.wind_speed,
          weather: x.weather_state_name,
          cityName: res.title,
          sunRise: res.sun_rise,
          sunSet: res.sun_set
        });
      });
      console.log(this.forecast);
    },
  },
  watch: {
    coords(newCoords) {
      this.searchWoeid(newCoords);
    },
    woeid(newWoeid) {
      this.getWeather(newWoeid);
    },
  },
};
</script>
