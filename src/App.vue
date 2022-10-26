<template>
  <div id="app" :class="weather.main  && weather.main.temp>18 ? 'warm' :''">
   <main>
    <div class="search-box">
      <input type="text" @change="fetchWeather()" v-model="query" class="search-bar" placeholder="Search...">
   
    </div>
    <div class="weather-wrap" v-if="weather.main">
      <div class="location-box">
        <div class="location">{{weather.name}}</div>
        <div class="date">{{dateBuilder()}}</div>
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
export default {
  name: 'app',
  data() {
    return {
      api_key: 'a96a214b366a57f2d63c2b4f1d68c00a',
      url_base: 'https://api.openweathermap.org/data/2.5/',
      query: '',
      weather: {}
    }
  },
  methods: {
    fetchWeather() {
      
      fetch(`${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`)
        .then(response => {
          return response.json();
        }).then(this.setResults);
      
    },
    setResults(results) {
      this.weather = results;
     
    },
    dateBuilder() {
      let d = new Date();
      let date = d.getDate();
      let days = ['Sunday','Monday','Tuesday','Wednesday','Thursday','Friday','Saturday']
      let months = ["January", "February", "March", "April", "May", "June", "July",
        "August", "September", "October", "November", "December"];
      let month = months[d.getMonth()];
      let day = days[d.getDay()];
      let year = d.getFullYear();
    return `${day} ${date} ${month} ${year}`;
    }
    
  }
}
</script>

<style  >
*{
  margin: 0;
  padding:0;
  box-sizing: border-box;
}
body{
  font-family: 'montserrat', sans-serif;}
#app{
  background-image: url('./assets/cold-bg.jpg');
  background-size:cover;
  background-position: bottom;
  transition: 0.4s;
}
#app.warm{
    background-image: url('./assets/warm-bg.jpg');
}
main{
  min-height: 100vh;
  padding: 15px;
  background-image: linear-gradient(to bottom,rgba(0,0,0,0.25),rgba(0, 0, 0, 0.75));
}
.search-box{
  width:100%;
  margin-bottom: 30px;
}
.search-box .search-bar{
  display:block;
  width:100%;
  padding:15px;
  color:#313131;
  font-size:20px;
  appearance: none;
  border:none;
  outline: none;
  background:none;
  background-color: rgba(255,255,255,0.5);
  border-radius:0 16px 0 16px;
  transition: 0.4s;
  box-shadow: 0 0 8px rgba(0,0,0,0.25);
}
.search-box .search-bar:focus{
    box-shadow: 0 0 16px rgba(0, 0, 0, 0.25);
    background-color: rgba(255, 255, 255, 0.75);
      border-radius:16px 0 16px 0;
}
.location-box .location{
  color:#fff;
  font-size: 32px;
  font-weight: 500;
  text-align: center;
  text-shadow:1px 3px rgba(0,0,0,0.25) ;
}
.location-box .date{
  color: #fff;
    font-size: 20px;
    font-weight: 300;
    font-style: italic;
    text-align: center;
}
.weather-box{
  text-align:center;
}
.weather-box .temp{
  display: inline-block;
  padding: 10px 15px;
  color:white;
  font-size: 102px;
  font-weight: 900;
  text-shadow: 3px 6px rgba(0,0,0,0.25);
  background-color: rgba(255, 255, 255, 0.25);
  border-radius:16px;
  margin:30px 0;
  box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}
.weather-box .weather{
  color:#fff;
  font-size:48px;
  font-weight:700;
  font-style:italic;
  text-shadow:3px 6px rgba(0,0,0,0.25);
}
</style>
