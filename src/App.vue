<template>
  <div id="app" :class="typeof weather.main !== 'undefined' && weather.main.temp > 19 ? 'warm' : ''">
    <main>
      <div class="search-box">
        <input
          type="text"
          class="search-bar"
          placeholder="Search for a city..."
          v-model="query"
          @keypress="fetchWeather"
        >
      </div>

      <div class="weather-wrap" v-if="typeof weather.main !== 'undefined'">
        <div class="location-box">
          <div class="location">
            {{ weather.name }}, {{ weather.sys.country }}
          </div>
          <div class="date">
           {{ dateBuilder() }}
          </div>
        </div>

        <div class="weather-box">
          <div class="temp">
            <div class="temp-min"><div class="min-text">Min</div>{{ Math.round(weather.main.temp_min) }}°c</div>
            <div class="temp-current">{{ Math.round(weather.main.temp) }}°c</div>
            <div class="temp-max"><div class="max-text">Max</div>{{ Math.round(weather.main.temp_max) }}°c</div>
            </div>
          <div class="weather">{{ weather.weather[0].main }}</div>
          <div class="weather-sub">{{ weather.weather[0].description }}</div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>
export default {
  name: 'App',
  data () {
    return {
      api_key: 'c495f3c47c2ba1e45fdd9de6605343ed',
      url_base: 'https://api.openweathermap.org/data/2.5/',
      query: '',
      weather: {}
    }
  },
  methods: {
    fetchWeather (e) {
      if (e.key === "Enter") {
        fetch(`${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`)
        .then(res => {
          return res.json();
        }).then(this.setResults);
      }
    },
    setResults (results) {
      this.weather = results;
    },
    dateBuilder () {
      let d = new Date();
      let months = ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"];
      let days = ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"];
      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();
      return `${day} ${date} ${month} ${year}`;
    }
  }
}
</script>

<style>
  * {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
  }

  body {
    font-family: 'montserrat', sans-serif;
  }

  #app {
    background-image: url('./assets/cold-bg.jpg');
    background-size: cover;
    background-position: top;
    transition: 400ms;
  }

  #app.warm {
    background-image: url('./assets/warm-bg.png');
  }
  main {
    min-height: 100vh;
    padding: 25px;
    background-image: linear-gradient(to bottom, rgba(0, 0, 0, 0.25), rgba(0, 0, 0, 0.75));
  }
  .search-box {
    width: 100%;
    margin-bottom: 30px;
  }
  .search-box .search-bar {
    display: block;
    width: 100%;
    padding: 15px;

    color: #313131;
    font-size: 20px;
    appearance: none;
    border:none;
    outline: none;
    background: none;
    box-shadow: 0 0 8px rgba(0, 0, 0, 0.25);
    background-color: rgba(255, 255, 255, 0.5);
    transition: 0.4s;
  }
  .search-box .search-bar:focus {
    box-shadow: 0 0 16px rgba(0, 0, 0, 0.25);
    background-color: rgba(255, 255, 255, 0.75);
  }
  .weather-wrap {
    margin-top: 80px
  }
  .location-box .location {
    color: #FFF;
    font-size: 32px;
    font-weight: 500;
    text-align: center;
    text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
  }
  .location-box .date {
    color: #FFF;
    font-size: 20px;
    font-weight: 300;
    font-style: italic;
    text-align: center;
  }
  .weather-box {
    text-align: center;
  }
  .weather-box .temp {
    display: flex;
    justify-content: center;
    align-items: center;
  }
  .weather-box .temp-current {
    padding: 10px 25px;
    color: #FFF;
    font-size: 102px;
    font-weight: 800;
    text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
    background-color:rgba(255, 255, 255, 0.25);
    border-radius: 16px;
    margin: 30px;
    box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  }
  .weather-box .temp-min, .weather-box .temp-max {
    color: #FFF;
    font-size: 24px;
    font-weight: 800;
    text-shadow: 3px 3px rgba(0, 0, 0, 0.25);
  }
  .weather-box .min-text, .weather-box .max-text {
    font-size: 18px
  }
  .weather-box .weather {
    color: #FFF;
    font-size: 48px;
    font-weight: 700;
    font-style: italic;
    text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  }

  .weather-box .weather-sub {
    color: #FFF;
    font-size: 20px;
    font-weight: 300;
    font-style: italic;
    text-align: center;
  }

</style>
