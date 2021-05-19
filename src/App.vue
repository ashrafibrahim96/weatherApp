<template>
  <div id="app" :class="typeof weather.main != 'undefined' && weather.main.temp > 16 ? 'warm' : ''">
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
      <div class="weather-wrap" v-if="typeof weather.main != 'undefined'" 
>
        <div class="location-box">
          <div class="location">{{weather.name}}, {{weather.sys.country}}</div>
          <div class="date">{{date()}}</div>
        </div>
        <div class="weather-box">
          <div class="temp">{{Math.round(weather.main.temp)}}°C</div>
          <div class="weather">{{weather.weather[0].main}}</div>
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
      api_key:'c27338b8eb1fa589346d94f3ec8a018b',
      url_base:'https://api.openweathermap.org/data/2.5/',
      query: '',
      weather: {}
    }
  },
  methods: {
    fetchWeather (e){
      if (e.key=="Enter") {
        fetch(`${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`)
        .then(res=>{
          return res.json();
        }).then(this.setResults);
      }
    },
    setResults(results){
      this.weather=results;
    },
    date() {
      let d= new Date();
      let months=["January", "February", "March", "April", "May", "June" , "July", "August",
      "September", "October", "November", "December"];
      let days=["Monday", "Tuesday","Wednesday","Thursday","Friday","Saturday","Sunday"];
      let day=days[d.getDay()];
      let month=months[d.getMonth()];
      let date=d.getDate();
      let year=d.getFullYear();
      return `${day} ${date} ${month} ${year}`
    }
  }
}
</script>

<style>
*{
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
body{
  font-family:'montserat', sans-serif;
}
#app{
  background-image: url('./assets/coldimage.jpeg');
  background-size: cover;
  background-position: bottom;
  transition: 0.4s;
}
#app.warm{
    background-image: url('./assets/hotimage.jpeg');

}
main{
  min-height: 100vh;
  padding: 25px;
  background-image: linear-gradient(to bottom, rgba(0,0,0,0.25), rgba(0,0,0,0.75));
}
.search-box{
  width: 100%;
  margin-bottom: 30px;
}
.search-box .search-bar{
  display: block;
  width:100%;
  padding: 15px;
  color: #313131;
  font-size: 20px;
  appearance: none;
  border: none;
  outline: none;
  background: none ;
  box-shadow: 0px 0px 10px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.5);
  border-radius: 20px 0px 20px 0px;
  transition: 0.4s;
}
.search-box .search-bar:focus{
  box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.75);
  border-radius: 0px 20px 0px 20px;
}
.location-box .location{
  color: white;
  font-size: 30px;
  font-weight: 600;
  text-align: center;
  text-shadow: 2px 4px rgba(0, 0, 0, 0.25);
}
.location-box .date{
   color: white;
  font-size: 20px;
  font-weight: 350;
  font-style: italic;
  text-align: center;
}

.weather-box{
  text-align: center;
}
.weather-box .temp{
  display: inline-block;
  padding: 10px 25px;
  color: white;
  font-size: 100px;
  font-weight: 800;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.25);
  border-radius: 16px;
  margin: 30px 0px;
  box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}

.weather-box .weather{
  color:white;
  font-size: 48px;
  font-weight: 700;
  font-style: italic;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  }
</style>
