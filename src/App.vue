<template>
  <div id="app" v-bind:class="classObject">
    <main>
      <div class="search-box">
        <input type="text"
               class="search-bar"
               v-model="query"
               @keypress="fetchWeather"
               placeholder="Search..."/>
      </div>

      <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
        <div class="location-box">
          <div class="location">{{ weather.name}}, {{ weather.sys.country }}</div>
          <div class="date">{{ dataBuilder() }}</div>
        </div>

        <div class="weather-box">
          <div class="temp">{{ Math.round(weather.main.temp) }} &#8451;</div>
          <div class="weather">{{ weather.weather[0].main}}</div>
        </div>
      </div>

    </main>
  </div>
</template>

<script>

export default {
  name: 'App',
  data() {

    return {

      api_key: '95de62e9af14db081ecfe7eb32a7c722',
      url_base: 'https://api.openweathermap.org/data/2.5/',
      query: '',
      weather: {}
    }
  },

  methods: {
    
    fetchWeather: function (e)  {

      if (e.key === "Enter") {

        fetch(`${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`)
          .then((res) =>{
            return res.json();
          }).then(this.setResult);
      }
    },

    setResult: function (results) {

      this.weather = results;

    },

    dataBuilder () {

      let d = new Date();
      let months = ['January', 'February', 'March', 'April', 'May', 'June', 'July', 'August', 'September', 'October', 'November', 'December'];
      let days = ['Sunday', 'Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday'];

      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();

      return `${day} ${date} ${month} ${year}`;

    }
    
  },

  computed: {

    classObject: function () {

      if (typeof this.weather.main != 'undefined') {
        return this.weather.main.temp > 10 ? 'warm' : 'cold'
      }

      else {

        return 'no-active'

      }
    }

  }
}
</script>

<style lang="scss">

  * {

    margin: 0;
    padding: 0;
    box-sizing: border-box;

  }

  body {

    font-family: 'montserrat', sans-serif;
    width: 100%;
    max-width: 1024px;
    min-width: 320px;
    margin: 0 auto;

  }

  #app {

    background-image: url("./assets/cold-bg.jpg");
    background-size: cover;
    background-position: bottom;
    transition: 0.4s;

    &.warm {

      background-image: url("./assets/hot-bg.jpg");

    }

    &.cold {

      background-image: url("./assets/cold-bg.jpg");

    }

  }

  main {

    min-height: 100vh;
    padding: 25px;
    background-image: linear-gradient(to bottom, rgba(0,0,0,0.25), rgba(0,0,0,0.75));

  }

  .search-box {

    width: 100%;
    margin-bottom: 30px;

    .search-bar {

      display: block;
      width: 100%;
      padding: 15px;
      color: #313131;
      font-size: 20px;
      appearance: none;
      border: none;
      outline: none;
      background: none;
      background-color: rgba(255, 255, 255, 0.5);
      box-shadow: 0 0 8px rgba(0,0,0,0.25);
      border-radius: 0 16px 0 16px;
      transition: 0.4s;

      &:focus {

        background-color: rgba(255, 255, 255, 0.75);
        box-shadow: 0 0 16px rgba(0,0,0,0.25);
        border-radius: 16px 0 16px 0;

      }

    }

  }

  .location-box {

    .location {

      color: #fff;
      font-size: 32px;
      font-weight: 500;
      text-align: center;
      text-shadow: 1px 3px rgba(0, 0, 0 , 0.25);

    }

    .date {

      color: #fff;
      font-size: 20px;
      font-weight: 300;
      text-align: center;
      font-style: italic;
      text-shadow: 1px 3px rgba(0, 0, 0 , 0.25);

    }

  }

  .weather-box {

    text-align: center;

    .temp {

      display: inline-block;
      padding: 10px 25px;
      color: #fff;
      font-size: 102px;
      font-weight: 900;
      text-shadow: 3px 6px rgba(0,0,0, 0.25);
      background-color: rgba(255,255,255, 0.25);
      border-radius: 16px;
      margin: 30px 0;
      box-shadow: 3px 6px rgba(0,0,0, 0.25);

    }

    .weather {

        color: #fff;
        font-size: 48px;
        font-weight: 700;
        font-style: italic;
        text-shadow: 3px 6px rgba(0,0,0,0.25);

    }
  }
</style>
