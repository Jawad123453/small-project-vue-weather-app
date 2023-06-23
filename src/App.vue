<script>
import axios from 'axios';
export default {
  data() {
    return {
      weatherData: "",
      mainwatherv: ""
    }
  },
  methods: {
    fetchWeatherData(e) {
      if (e.key == "Enter") {
        const apiKey = 'd7deecdcf36a1ead74f22104c211c629';
        const url = `https://api.openweathermap.org/data/2.5/weather?q=${this.mainwatherv}&appid=${apiKey}`;
        axios.get(url).then(response => {
          this.weatherData = response.data;
          console.log(this.weatherData);
        })
          .catch(error => {
            console.error(error);
          });
      }
    },
    dateBuilder() {
      const date = new Date();
      const months = [
        'January', 'February', 'March', 'April', 'May', 'June',
        'July', 'August', 'September', 'October', 'November', 'December'
      ];
      const days = [
        'Sunday', 'Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday'
      ];
      const day = days[date.getDay()];
      const month = months[date.getMonth()];
      const dayNumber = date.getDate();
      const year = date.getFullYear();

      return `${day}, ${month} ${dayNumber}, ${year}`;
    }
  }
}
</script>

<template>
  <main class="mainbg" :class="(typeof weatherData.main != 'undefined' && weatherData.main.temp - 274 > 16 ?'hot':'cold')">
    <div class="search-box">
      <input type="text" class="mainsearch" placeholder="Search" @keypress="fetchWeatherData" v-model="mainwatherv">
    </div>
    <div class="result-text" v-if="(typeof weatherData.main !='undefined')">
      <h1>{{ weatherData.name }}, {{ weatherData.sys.country }}</h1>
      <p>{{ dateBuilder() }}</p>
      <h1 class="degree">
        {{Math.round(weatherData.main.temp - 274)}}&deg;c
      </h1>
      <h3><i>{{ weatherData.weather[0].main}}</i></h3>
    </div>
  </main>
</template>

<style>
*{
  padding:0;
  margin:0;
  box-sizing: border-box;
  font-family:sans-serif;
}
.mainbg.cold{
  background-repeat: no-repeat;
  background-position: center;
  height:100vh;
  width:100%;
  background-size: cover;
  background: linear-gradient(to bottom, transparent, rgba(0, 0, 0, 0.8)),url("./assets/p3.jpg");
}
.mainbg.hot{
  background-repeat: no-repeat;
  background-position: center;
  height:100vh;
  width:100%;
  background-size: cover;
  background: linear-gradient(to bottom, transparent, rgba(0, 0, 0, 0.8)),url("./assets/p2.webp");
}
.search-box{
  padding: 20px;
}
.search-box input.mainsearch{
  width:100%;
  padding:15px ;
  border-radius: 20px;
  border-bottom-left-radius: 0;
  border-top-right-radius: 0;
  border: none;
  background-color: rgba(255, 255, 255, 0.8);
  font-weight: bold;
  outline:none;
}
.result-text {
  margin: 0 auto;
  width:fit-content;
  text-align: center;
}
.result-text h1{
  font-size: 30px;
  color:white;
}
.result-text p{
  color:white;
  opacity: 0.8;
}
.result-text h1.degree{
  font-size: 4rem;
  padding:10px;
  background-color: rgba(255, 255, 255, 0.4);
  margin:15px auto;
  border-radius: 10px;
  color:white;
  font-weight: bold;
  width:fit-content;
}
.result-text h3{
  font-size: 2.5rem;
  font-weight: bold;
  color:white;
}
</style>
