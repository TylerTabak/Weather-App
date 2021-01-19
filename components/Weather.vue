<template>
    <div class="weather-app-card" :class="[typeof weather.main != 'undefined' && weather.main.temp > 16 ? 'warm' : '', typeof weather.main != 'undefined' ? weather.weather[0].main : '']" >
      <main>
        <div class="search-box">
          <input 
            type="text" 
            class="search-bar" 
            placeholder="Search..."
            v-model="query"
            @keypress="fetchWeather"
          /> 
        </div>
        <div class="quick-search-box"><button class="quick-search" @click="getCoordinates()">Get Location</button></div>
        <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
          <div class="location-box">
            <div class="location">
              {{ weather.name }}
            </div>
            <div class="date">
              {{ dateBuilder() }}
            </div>
            <div class="weather-box">
              <div class="temp">{{ Math.round(weather.main.temp) }}°c</div>
              <div class="weather">{{ weather.weather[0].main }}</div>
            </div>
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
      api_key: "f8d14e577460a99cfffba3f8f2d1a1cb",
      url_base: "https://api.openweathermap.org/data/2.5/",
      query: '',
      weather: {}
    }
  },
  methods: {
    fetchWeather (e) {
      if (e.key == "Enter") {
        fetch(`${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`)
          .then(res => {
            return res.json();
          }).then(this.setResults);
      }
    },
    setResults (results) {
      this.weather = results;
    },
    dateBuilder() {
      let d = new Date();
      let months = ["January","February","March","April","May","June","July",
            "August","September","October","November","December"];
      let days = ['Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday', 'Sunday'];

      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();

      return `${day} ${date} ${month} ${year}`;
    },
    getCoordinates(){
      if (navigator.geolocation) {
        navigator.geolocation.getCurrentPosition(this.showPosition);
    } else {
    alert('Geolocation Not Supported');
    }
    },
    showPosition(position){
      var lat = position.coords.latitude;
      var long =position.coords.longitude;
      fetch(`${this.url_base}weather?lat=${lat}&lon=${long}&units=metric&APPID=${this.api_key}`).then(res => {
            return res.json();
          }).then(this.setResults);
    }
  },
  del (index) {
      // this.arr.splice(index, 1)
      this.$delete(this.arr, index)
    }
}
</script>

<style>
* {
  margin:0;
  padding:0;
  box-sizing: border-box;
}

body {
  font-family: "monsterrat", sans-serif;
}
.weather-app-card {
  background-image: url('../src/assets/cold-bg.jpg');
  background-size:contain;
  background-position: bottom center;
  background-repeat: no-repeat;
  transition: background-image 0.4s;
  min-width: 527px;
  margin-left:25px;
  margin-right:25px;
}

@media only screen and (max-width: 600px) {
  .weather-app-card{
    margin:0;
    min-width: unset;
    width:100%;
  }
  .component-container{
  width:100%;
}
}

.weather-app-card.warm{
  background-image: url('../src/assets/warm-bg.jpg');
}





.weather-app-card.Rain main{
  background-image: url("../src/assets/rain-300x300.png");
  animation: rain 0.4s linear infinite;
}
.weather-app-card.warm.Clear main{
  background-image: url("../src/assets/sunray.png");
  background-position: 50% 50%;
}

.weather-app-card.Clouds main{
  background-image: url("../src/assets/clouds.png");
  background-repeat: repeat-x;
  animation: float 15s linear infinite;
  background-position: 0px 0px;
  background-size: auto 30%;
}

.weather-app-card.Snow main{
  background-image: url("../src/assets/snow.png");
  animation: snowing  1s linear infinite;
  opacity: 1;
}


.weather-app.card.Mist::before{

}

@keyframes snowing { 
  from { background-position: 0 0; }
	to { background-position: 50% 375%; }
}

@keyframes float {
	from { background-position: 0 0; }
	to { background-position: 98% 0; }
}

@keyframes ray { 
  0% {
    opacity: 1;
  }

  25% {
    opacity: 0.5;
  }

  50% {
    opacity: 0.2;
  }

  75% {
    opacity: 1.5;
  }
  100% {
    opacity: 1;
  }
}

@keyframes rain { 
  0% {
    background-position: 0 0;
    opacity: 1;
  }

  100% {
    background-position: 10% 50%;
    opacity: 1;
  }
}

@keyframes color-change{
  0% {
    filter: hue-rotate(0deg);
  }

  50% {
    filter: hue-rotate(0deg);
  }
  100% {
    filter: hue-rotate(360deg);
  }
}

main {
  min-height: 100vh;
  padding: 25px;

  background-image: linear-gradient(180deg, rgba(0,0,0,0.05) 0%, rgba(0,0,0,0.25) 100%);
  
}

.search-box, .quick-search-box {
  width:100%;
  margin-bottom: 30px;
  display: flex;
  justify-content: center;
}

.search-box .search-bar{
  display: block;
  width: 100%;
  max-width: 360px;
  padding: 15px;
  color: #313131;
  font-size: 20px;
  
  appearance: none;
  border: none;
  outline: none;
  background: none;

  box-shadow: 0px 0px 8px rgba(0, 0, 0, 0.25);
  background-color: rgba(255,255,255,0.5);
  border-radius: 0 16px 0 16px;
  transition: 0.4s;
}

.quick-search{
  font-family: "monsterrat", sans-serif;
  background-color: #ffffff00;
  padding: 15px 25px;
  border-radius: 16px 0 16px 0;
  transition: 0.4s;
}
.quick-search:focus {
    outline: none;
}
.quick-search:hover{
  border-radius: 0 16px 0 16px;
}

.search-box .search-bar:focus {
  box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25);
  background-color: rgba(255,255,255,0.75);
  border-radius: 16px 0px 16px 0px;
}


.location-box .location {
  color:#fff;
  font-size: 32px;
  font-weight: 500;
  text-align: center;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
}

.location-box .date {
  color:#fff;
  font-size: 20px;
  font-weight: 300;
  font-style: italic;
  text-align: center;
}

.weather-box{
  text-align: center;
}

.weather-box .weather{
  color:#fff;
  font-size: 48px;
  font-weight: 700;
  font-style: italic;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}

.weather-box .temp {
  display: inline-block;
  padding: 10px 25px;
  color:#fff;
  font-size: 102px;
  font-weight: 900;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  background-color: rgba(255,255,255,0.25);
  border-radius: 16px;
  margin: 30px 0;

  box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}
</style>