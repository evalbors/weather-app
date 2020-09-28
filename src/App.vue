<template>
  <div id="app" :class="typeof weather.main != 'undefined' && weather.main.temp > 15 ? 'warm' : '' ">
    <main>
      <h1>Weather app</h1>
      <p>
        This is my first project in Vue.js. Let's go, 
        <a href="https://media3.giphy.com/media/B0vFTrb0ZGDf2/giphy.gif?cid=ecf05e479754482bfb8ebc7a1892d8f9f0b5e73790b205fb&rid=giphy.gif" target="_blank" rel="noopener">girl!</a>
      </p>
    
      <div class="search-box">
        <input 
        type="text" 
        class="search-bar" 
        placeholder="Search..." 
        v-model="query" 
        @keypress="fetchWeather"
        />
      </div>

      <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
        <div class="location-box">
          <div class="location">{{ weather.name }}, {{ weather.sys.country }}</div>
          <div class="date">{{ dateBuilder() }}</div>
        </div>
        <div class="weather-box">
          <div class="temp">{{ Math.round(weather.main.temp) }}°C</div>
          <div class="weather">{{ weather.weather[0].main }}</div>
        </div>
      </div>
    </main>
    
  </div>
</template>

<script>

export default {
  name: 'App',
  data() {
    return{
      api_key: '97b03f929544b76c20acbe6cf5490119',
      url_base: 'https://api.openweathermap.org/data/2.5/',
      query: '',
      weather: {}
    }
  },
  methods: {
    fetchWeather (e){
      if (e.key == "Enter") {
        fetch(`${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`)
          .then(res => {
            return res.json();
          }).then(this.setResults)
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
    font-family: Avenir, Helvetica, Arial, sans-serif;
  }

  #app {
    text-align: center;
    color: #2c3e50;
    background-image: url('./assets/cold-bg.jpg');
    background-size: cover;
    background-position: bottom;
    transition: 0.4s;
  }

  #app.warm {
    background-image: url('./assets/warm-bg.jpg');
  }

  main{
    min-height: 100vh;
    background-image: linear-gradient(to bottom, rgb(0, 0, 0, 0.10), rgb(0, 0, 0, 0.5));   
  }

  a {
    color: rgb(255, 255, 255);
    font-weight: 600;
  }

  a:hover {
    color: rgb(255, 255, 255);
    font-weight: 600;
  }

  p {
    color: rgb(255, 255, 255);
    text-shadow: 0 0 16px rgba(0, 0, 0, 0.5);
  }

  h1 {
    padding-top: 150px;
    font-size: 40px;
    color: rgb(255, 255, 255);
    text-shadow: 0 0 16px rgba(0, 0, 0, 0.2);
  } 

  .search-box {
    padding: 50px;
    text-align: center;
    margin: auto;
    width: 50%;

  }

  @media (max-width: 600px) {
    .search-box{
      width: 100%;
      padding-top: 50px;
    }     
  }

  .search-box .search-bar {
    text-align: center;
    display: block;
    width: 100%;
    padding: 10px;

    color: #2c3e50;
    font-size: 18px;

    appearance: none;
    border: none;
    outline: none;
    background: none;

    box-shadow: 0 0 8px rgba(0, 0, 0, 0.2);
    background-color: rgba(255, 255, 255, 0.4);
    border-radius: 8px;
    transition: 0.4s;
  }

  .search-box .search-bar:focus{
    box-shadow: 0 0 16px rgba(0, 0, 0, 0.2);
    background-color: rgba(255, 255, 255, 0.8);
  }

  .location{
    color: rgba(255, 255, 255, 0.75);
    font-size: 24px;
    font-weight: 100;
    text-align: center;
  }

  .location-box .date {
    color: #ffffff;
    font-size: 18px;
    font-weight: 300;
    text-align: center;
  }

  .weather-box {
    text-align: center;
    color: rgb(255, 255, 255);

  }

  .temp{
    border-top: 1px dotted #ffffff;
    border-bottom: 1px dotted #ffffff;
  }

  .weather-box .temp {
    display: inline-block;
    padding: 10px 25px;
    margin: 15px;
    font-size: 92px;
    font-weight: 800;
    color: rgba(255, 255, 255, 0.75);

  }

  .weather-box .weather {
    font-size: 20px;
    color: rgba(255, 255, 255, 0.75);
    text-transform: uppercase;
    letter-spacing: 1;
    font-weight: 500;
  }

</style>
