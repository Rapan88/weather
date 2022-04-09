<template>
  <div class="container">
    <div class="row">
      <div class="col-1"></div>
      <div class="col-10">
        <div style="height: 75vh" class="d-flex align-items-center">
          <div
            id="main"
            style="height: 52vh; width: 105vh"
            class="rounded-3"
            v-if="forecast[0]"
          >
            <h1 id="h1">Weather in {{ forecast[0].cityName }} on today</h1>
            <h2 id="h2">
              Now in {{ forecast[0].cityName }}
              {{ forecast[0].time.slice(11, 16) }}
            </h2>
            <div id="currWeather">
              <div id="todayWeather">
                +{{ Math.trunc(forecast[0].temperature.currTemperature) }}°
              </div>
              <ul id="hourly">
                <li>
                  <span class="todayHourlyName">Night</span>
                  <i></i>
                  <span class="todayHourlyWeather"
                    >+{{
                      Math.trunc(forecast[0].temperature.currTemperature)
                    }}°</span
                  >
                  <span class="todayHourlyFeel">Chilly</span>
                </li>
                <li>
                  <span class="todayHourlyName">Morning</span>
                  <i></i>
                  <span class="todayHourlyWeather"
                    >+{{
                      Math.trunc(forecast[0].temperature.currTemperature)
                    }}°</span
                  >
                  <span class="todayHourlyFeel">Chilly</span>
                </li>
                <li>
                  <span class="todayHourlyName">Day</span>
                  <i></i>
                  <span class="todayHourlyWeather"
                    >+{{
                      Math.trunc(forecast[0].temperature.currTemperature)
                    }}°</span
                  >
                  <span class="todayHourlyFeel">Chilly</span>
                </li>
                <li>
                  <span class="todayHourlyName">Evening</span>
                  <i></i>
                  <span class="todayHourlyWeather"
                    >+{{
                      Math.trunc(forecast[0].temperature.currTemperature)
                    }}°</span
                  >
                  <span class="todayHourlyFeel">Chilly</span>
                </li>
              </ul>
            </div>
            <span id="spanUnderCurr"
              >Feels Like
              <b>
                +{{ Math.trunc(forecast[0].temperature.currTemperature) }}°</b
              ></span
            >
            <h3 id="h3">{{ forecast[0].weather }}</h3>
            <table id="todayTable">
              <tbody id="tbody">
                <tr>
                  <th>Chance of precipitation</th>
                  <td><b class="cpecB">50</b> %</td>
                </tr>
                <tr>
                  <th>Wind</th>
                  <td><b class="cpecB">6</b> mps</td>
                </tr>
                <tr>
                  <th>Preccure</th>
                  <td><b class="cpecB">738</b></td>
                </tr>
                <tr>
                  <th>UV Index</th>
                  <td><b class="cpecB">0/12</b></td>
                </tr>
                <tr>
                  <th>Humidility</th>
                  <td><b class="cpecB">{{forecast[0].humidity}}</b> %</td>
                </tr>
                <tr>
                  <th>Precipitation</th>
                  <td><b class="cpecB">0</b> mm</td>
                </tr>
              </tbody>
            </table>
          </div>
        </div>
      </div>
      <div class="col-1"></div>
    </div>
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
            currTemperature: x.the_temp,
          },
          visibility: x.visibility,
          humidity: x.humidity,
          windSpeed: x.wind_speed,
          weather: x.weather_state_name,
          cityName: res.title,
          sunRise: res.sun_rise,
          sunSet: res.sun_set,
          time: res.time,
          country: res.parent.title,
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

<style>
#main {
  background-image: url("../imgs/cloud_evening.jpg");
  background-size: cover;
  font-size: 14px;
  font-weight: 400;
}
#h1 {
  color: #fff;
  display: block;
  font-family: "Roboto", sans-serif;
  font-size: 24px;
  font-weight: 400;
  letter-spacing: 0.18px;
  line-height: 28px;
  margin: 0 0 6px;
  padding: 20px 0 0 20px;
}
#h2 {
  color: #fff;
  display: block;
  font-size: 14px;
  font-weight: 400;
  letter-spacing: 0.25px;
  line-height: 20px;
  margin: 0;
  opacity: 0.7;
  padding-left: 20px;
}
#currWeather {
  -webkit-box-align: center;
  -ms-flex-align: center;
  -webkit-align-items: center;
  align-items: center;
  display: -webkit-box;
  display: -webkit-flex;
  display: -ms-flexbox;
  display: flex;
  padding-left: 30px;
}
#todayWeather {
  color: #fff;
  display: -webkit-box;
  display: -webkit-flex;
  display: -ms-flexbox;
  display: flex;
  font-size: 72px;
  font-weight: 400;
  letter-spacing: -0.5px;
  line-height: 72px;
  margin-top: 12px;
  min-width: 275px;
  position: relative;
}
#spanUnderCurr {
  -webkit-text-size-adjust: 100%;
  --swiper-theme-color: #007aff;
  --swiper-navigation-size: 44px;
  font-family: Roboto, sans-serif;
  box-sizing: border-box;
  color: hsla(0, 0%, 100%, 0.7);
  display: block;
  font-size: 16px;
  font-style: normal;
  font-weight: 400;
  letter-spacing: 0.25px;
  line-height: 24px;
  margin-top: 10px;
  padding: 0 20px;
}
#hourly {
  -webkit-text-size-adjust: 100%;
  line-height: 1.15;
  --swiper-theme-color: #007aff;
  --swiper-navigation-size: 44px;
  font-family: Roboto, sans-serif;
  font-size: 14px;
  font-weight: 400;
  box-sizing: border-box;
  list-style: none;
  -webkit-box-pack: justify;
  -webkit-box-align: center;
  align-items: center;
  display: flex;
  justify-content: space-between;
  margin: 0 45px 12px 24px;
  overflow-x: auto;
  overflow-y: hidden;
  padding: 16px 0 0;
  width: 100%;
}
b {
  color: #fff;
}
.todayHourlyName {
  color: #fff;
  display: block;
  font-size: 14px;
  letter-spacing: 0.25px;
  line-height: 20px;
  text-align: center;
}
.todayHourlyWeather {
  color: #fff;
  display: block;
  font-size: 22px;
  font-weight: 500;
  letter-spacing: 0.1px;
  line-height: 22px;
  text-align: center;
}
.todayHourlyFeel {
  color: #fff;
  display: block;
  font-size: 14px;
  font-weight: 500;
  line-height: 14px;
  margin-top: 8px;
  opacity: 0.7;
  text-align: center;
}
#h3 {
  color: #fff;
  font-size: 16px;
  font-weight: 500;
  letter-spacing: 0.15px;
  line-height: 24px;
  margin: 0;
  padding: 0 20px 24px;
}
#todayTable {
  -webkit-text-size-adjust: 100%;
  line-height: 1.15;
  --swiper-theme-color: #007aff;
  --swiper-navigation-size: 44px;
  font-family: Roboto, sans-serif;
  font-size: 14px;
  font-weight: 400;
  box-sizing: border-box;
  backdrop-filter: blur(12px);
  background: rgba(0, 0, 0, 0.08);
  border-radius: 8px;
  display: block;
  margin: 0 20px 20px;
  padding: 12px;
}
#tbody {
    -webkit-text-size-adjust: 100%;
    line-height: 1.15;
    --swiper-theme-color: #007aff;
    --swiper-navigation-size: 44px;
    font-family: Roboto,sans-serif;
    font-size: 14px;
    font-weight: 400;
    box-sizing: border-box;
    -webkit-box-pack: justify;
    display: flex;
    flex-wrap: wrap;
    justify-content: space-between;
}
tr {
  display: block;
  position: relative;
}
th {
  color: hsla(0, 0%, 100%, 0.7);
  display: block;
  font-size: 12px;
  font-weight: 400;
  letter-spacing: 0.25px;
  line-height: 14px;
  text-align: left;
  white-space: nowrap;
}
td {
  -webkit-box-align: center;
  -ms-flex-align: center;
  -webkit-align-items: center;
  align-items: center;
  color: #fff;
  display: -webkit-box;
  display: -webkit-flex;
  display: -ms-flexbox;
  display: flex;
  font-size: 12px;
  letter-spacing: 0.25px;
  line-height: 14px;
  padding-top: 6px;
  white-space: pre;
}
.cpecB {
  color: #fff;
  font-size: 20px;
  font-weight: 500;
  letter-spacing: 0.1px;
  line-height: 20px;
}
</style>