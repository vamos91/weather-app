<template>
  <div id="app" :class="weather.temp > 20 ? 'warm' : ''">
    <main>
      <div class="search-box">
        <input type="text" class="search-bar" placeholder="Search..." v-model="query" v-on:keyup.enter="getWeather">
      </div>
      <div class="weather-wrap">
        <div class="location-box">
          <div class="location">{{ query }}</div>
          <div class="date">{{ date }}</div>
        </div>
        <div class="weather-box">
          <div class="temp">{{ weather.temp }}</div>
          <div class="weather">{{ weather.weather }}</div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  name: 'App',
  data(){
    return{
      api_key: 'd578d6888b57e79f878b7d5bf8517065',
      url_base: 'api.openweathermap.org/data/2.5/',
      query: '',
      date: '',
      weather: {
        temp: '',
        weather: ''
      }
    }
  },
  methods:{
    getWeather: function(){
      axios.get(`https://${this.url_base}weather?q=${this.query}&units=metric&appid=${this.api_key}`)
      .then(response => (this.info = response))
      .then((data) => {
        if(data.data.cod === 200){
          this.query = data.data.name
          this.weather.temp = data.data.main.temp
          this.weather.weather = data.data.weather[0].main
          var t = new Date(1970, 0, 1);
          t.setSeconds(data.data.dt);
          this.date = t.toLocaleDateString()

        }else{
          this.query = 'Cette ville nexiste pas'
        }
      })
      .catch((err) => {

        this.query = "Cette ville n'existe pas"
        console.log(err)
      })
    }
  }
}
</script>

<style>
*{
  padding: 0;
  margin: 0;
  box-sizing: border-box;
}

body{
  font-family: 'montserra', sans-serif;
}

#app{
  background-image: url('./assets/cold-bg.jpg');
  background-size: cover;
  background-position: center;
}

main{
  height: 100vh;
  padding: 25px;
  background-image: linear-gradient(to bottom, rgba(0, 0, 0, 0.25), rgba(0, 0, 0, 0.75));
}

.search-box{
  width: 100%;
  margin-bottom: 100px;
}

.search-box .search-bar{
  display: block;
  width: 100%;
  padding: 15px;
  color: #313131;
  font-size: 20px;
  appearance: none;
  border: none;
  outline: none;
  background: none;
  color: gray;
  background-color: rgba(255, 255, 255, 0.15);
  border-radius: 0px 16px 0px 16px;
  box-shadow: 0px 0px 8px rgba(0, 0, 0, 0.25);
  transition: .4s;
}

.search-box .search-bar:focus{
  box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25);
  background-color: rgba(0, 0, 0, 0.15);
  color: white;
  border-radius: 16px 0px 16px 0px;

}

.location-box .location{
  color: white;
  font-size: 32px;
  font-weight: 500;
  text-align: center;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
}

.location-box .date{
  color: white;
  font-size: 20px;
  font-weight: 300;
  text-align: center;
  font-style: italic;
}

.weather-box{
  text-align: center;
}

.weather-box .temp{
  display: inline-block;
  padding: 10px 25px;
  font-size: 102px;
  color: white;
  font-weight: 900;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.25);
  border-radius: 16px;
  margin: 30px;
}

.weather-box .weather{
  color: white;
  font-size: 48px;
  font-weight: 700;
  font-style: italic;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}

#app.warm{
  background-image: url('./assets/warm-bg.jpg');
}
</style>
