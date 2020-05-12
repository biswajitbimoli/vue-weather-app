<template>
  <div id="app">
    <main>
      <div class="search-box">
        <input
          type="text"
          value="Tezpur"
          ref="groupId"
          placeholder="Search..."
          class="search-bar"
          @keypress="queryFunc"
        />
      </div>
      <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
        <div class="location-box">
          <div class="location">{{weather.name}}, {{weather.sys.country}}</div>
          <div class="date">{{date}}</div>
        </div>
        <div class="weather-box">
          <div class="temp">{{Math.round(weather.main.temp)}}°c</div>
          <div class="weather">{{weather.weather[0].main}}</div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "App",

  data() {
    return {
      api_key: "Your api key",
      url_base: "https://api.openweathermap.org/data/2.5/",
      query: "",
      weather: [],
      error: [],
      date: null,
    };
  },
  methods: {
    dateBuilder: function() {
      var d = new Date();
      var months = ['January', 'February', 'March', 'April', 'May', 'June', 'July', 'August', 'September', 'October', 'November', 'December'];
      var days = ['Sunday', 'Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday'];

      var day = days[d.getDay()];
      var date = d.getDate();
      var month = months[d.getMonth()];
      var year = d.getFullYear();
      return `${day} ${date} ${month} ${year}`;
      
    },
    queryFunc(e) {
      if (e.key == "Enter") {
        this.query = this.$refs.groupId.value;
        axios
        .get(
          `${this.url_base}weather?q=${this.query}&units=metric&APPID=4f2fe202f0535e3b4ae6ec1acffa4f42`
        )
        .then(res => {
          this.weather = res.data;
        })
        .cath(e => {
          this.error.push(e);
        });
      }
    }
  },
  created() {
    this.date = this.dateBuilder();
    
    axios
        .get(
          `${this.url_base}weather?q=Tezpur&units=metric&APPID=${this.query}`
        )
        .then(res => {
          this.weather = res.data;
        })
        .cath(e => {
          this.error.push(e);
        });
  },
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
body {
  font-family: "montserrat", sans-serif;
  background-image: linear-gradient(
    to bottom,
    rgba(0, 0, 0, 0.25),
    rgba(0, 0, 0, 0.25)
  );
}
#app {
  background-image: url("./assets/weather.png");
  background-size: cover;
  background-position: bottom;
  transition: 0.6s;
}

main {
  min-height: 70vh;
  padding: 25px;
  background-image: linear-gradient(
    to bottom,
    rgba(0, 0, 0, 0.25),
    rgba(0, 0, 0, 0.25)
  );
}

.search-box {
  margin-bottom: 30px;
  width: 100%;
}

.search-box .search-bar {
  display: block;
  width: 100%;
  padding: 15px;
  color: #313131;
  font-size: 20px;
  appearance: none;
  border: none;
  outline: none;
  background: none;

  box-shadow: 0px 0px 8px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.5);
  border-radius: 16px 0px 16px 0px;
}

.location-box .location {
  color: #fff;
  font-size: 32px;
  font-weight: 500;
  text-align: center;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
}
.location-box .date {
  color: #fff;
  font-size: 20px;
  font-weight: 300;
  font-style: italic;
  text-align: center;
}

.weather-box {
  text-align: center;
}

.weather-box .temp {
  display: inline-block;
  padding: 10px 25px;
  color: #fff;
  font-size: 102px;
  font-weight: 900;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  background-color: rgba(206, 28, 203, 0.25);
  border-radius: 16px;
  margin: 30px 0px;

  box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}
.weather-box .weather {
  color: #fff;
  font-size: 48px;
  font-weight: 700;
  font-style: italic;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}
</style>
