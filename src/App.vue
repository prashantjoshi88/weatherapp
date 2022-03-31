<template>
  <div id="app" :class="this.climate ?  this.weth.filter(e => climate.includes(e))   : ''  ">
    <div class="main">
      <div class="search-box">
        <input 
          type="text" 
          class="search-bar" 
          placeholder="Search..."
          v-model="query"
          @keypress="fetchWeather"
        />
      </div >
    <span class="err" v-if="this.message">{{message}}</span>

 

      <div class="location-box " v-if="weather.name">
      <div class="location" > {{ weather.name }}, {{ weather.country }} </div>
      <div class="date">{{ dateBuilder() }}</div>
    
      </div>
      <div class="weather-box" v-if="temp.temp_c">
         <div class="temp"> {{ (temp.temp_c) }}°c</div> <br>
         <div class="weather">Weather: {{ capitalizeFirstLetter(climate) }}</div>
         <div class="weather-sm"> {{ capitalizeFirstLetter(climate) }}</div>
        <div > <img :src="this.logo" alt=""> </div>

      </div>
      
      <div class="weather-box" v-else>
        <div class="temp-search">
          Seacrch for Weather
        </div>
        <div class="temp-search-sm">
          Seacrch for Weather
        </div>
      </div>
    </div>
  </div>
</template>


<script>


import axios from 'axios'
export default {
  name: 'app',
  data () {
    return {
      api_key: 'ce837410e43f4d7ab78101905221902',
      url_base: "http://api.weatherapi.com/v1/current.json",
      // weather_icon: "http://openweathermap.org/img/wn/",
      query: '',
      weather: {},
      temp:{},
      message : '',
      climate:'',
      weth:['mist' , 'fog' , 'sunny' , 'cold' , 'cloudy' , 'snow', 'rain' ,'overcast'],
      logo : null,
      lat : '',
      long : ''
      
    }
  },
  
  // beforeCreate(){ 
  //     axios.get(
  //             `http://api.weatherapi.com/v1/current.json?q=${'ahmedabad'}&key=ce837410e43f4d7ab78101905221902&units=metric`
  //         ) .then((response) => {
  //       this.weather = response.data.location;
  //       this.temp = response.data.current;
  //       this.climate = response.data.current.condition.text;
  //       this.logo = response.data.current.condition.icon
  //       this.climate = this.climate.toLowerCase() ;
  //       console.log(response.data);
  //       console.log(this.climate);
  //       this.message = null;
        
  //     })
  // },

  beforeCreate(){
    
      navigator.geolocation.getCurrentPosition(
      position => {
         console.log(position.coords.latitude);
         this.lat = position.coords.latitude
         console.log(position.coords.longitude);
         this.long = position.coords.longitude
         console.log(position);
         axios.get(
         "http://api.weatherapi.com/v1/current.json?q=" +
         this.lat +
         "," +
         this.long +
         "&key=ce837410e43f4d7ab78101905221902"
        ).then((response) =>{
           this.weather = response.data.location;
        this.temp = response.data.current;
        this.climate = response.data.current.condition.text;
        this.climate = this.climate.toLowerCase() ;
        console.log(response.data);
        console.log(this.climate);
        this.message = null;
        
        })
      },
      error => {
         console.log(error.message);
      },
    
   )
     
  },
  methods: {
  
    fetchWeather (e) {
      if (e.key == "Enter") {
          axios.get(
              `${this.url_base}?q=${this.query}&key=${this.api_key}&units=metric`
          )
          .then((response) => {
        this.weather = response.data.location;
        this.temp = response.data.current;
        this.climate = response.data.current.condition.text;
        this.climate = this.climate.toLowerCase() ;
        console.log(response.data);
        console.log(this.climate);
        this.message = null;
        
        
      }).then(this.setResults).catch(() =>{
      //  alert('please type a correct city name')
         this.message = 'Please type a Correct City Name'
     
      })
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
    },
     capitalizeFirstLetter(string) {
        return string.charAt(0).toUpperCase() + string.slice(1);
    },
    
    
  },
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
  background-repeat: no-repeat;
  background-size: cover;
  background-position: center;
  transition: 0.4s;
}
#app.cold{
   background-image: url('https://source.unsplash.com/random/1170x800/?cold-weather');
   /* background-image: url('./assets/cold.jpeg'); */
}
#app.snow{
   /* background-image: url('./assets/snow.jpg'); */
   background-image: url('https://source.unsplash.com/random/1170x800/?snow-weather');

}
#app.sunny{
   background-image: url('https://source.unsplash.com/random/1170x800/?sunny-weather');
  /* background-image: url('./assets/warm.jpg'); */
}
#app.rain{
   background-image: url('https://source.unsplash.com/random/1170x800/?rain-weather');
  /* background-image: url('./assets/rainy.jpg'); */
}
#app.fog{
  background-image: url('./assets/fog.jpeg');
}
#app.mist{
  background-image: url('https://source.unsplash.com/random/1170x800/?mist-weather');
  /* background-image: url('./assets/mist.jpeg'); */
}
#app.overcast{
  background-image: url('https://source.unsplash.com/random/1170x800/?overcast-weather');
  /* background-image: url('./assets/overcast.jpg'); */
}
#app.cloudy{
  background-image: url('https://source.unsplash.com/random/1170x800/?cloudy-weather');
  /* background-image: url('./assets/mist.jpeg'); */
}



.anim{
  size: 12px;
}

.main{
   min-height: 100vh;
  padding: 25px;

  background-image: linear-gradient(to bottom, rgba(0, 0, 0, 0.25), rgba(0, 0, 0, 0.75));
}

.search-box {
  width: 100%;
  margin-bottom: 30px;
}

.err{
 display: inline-block;
  padding: 10px 25px;
  color: rgb(221, 40, 40);
  font-size: 24px;
  font-weight: 900;

  background-color:rgba(255, 255, 255, 0.25);
  border-radius: 16px;


  box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
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

  box-shadow: 0px 0px 8px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.5);
  border-radius: 0px 16px 0px 16px;
  transition: 0.4s;
}

.search-box .search-bar:focus {
  box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.75);
  border-radius: 16px 0px 16px 0px;
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
  display: inline-block;
  padding: 10px 25px;
  color: #FFF;
  font-size: 102px;
  font-weight: 900;

  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  background-color:rgba(255, 255, 255, 0.25);
  border-radius: 16px;
  margin: 30px 0px;

  box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}

.weather-box .temp-search{
  display: none;
  display: inline-block;
  padding: 10px 25px;
  color: #FFF;
  font-size: 102px;
  font-weight: 900;

  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  background-color:rgba(255, 255, 255, 0.25);
  border-radius: 16px;
  margin: 30px 0px;

  box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}

.weather-box .temp-search-sm{
  display: none;
   color: #FFF;
  font-size: 20px;
  font-weight: 300;
  font-style: italic;
  text-align: center;
}

.weather-box .weather {
  display: none;
  color: #FFF;
  font-size: 48px;
  font-weight: 700;
  font-style: italic;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}

.weather-sm{
  display: none;
   color: #FFF;
  font-size: 48px;
  font-weight: 700;
  font-style: italic;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}

@media (max-width:767px) {
  .weather-sm{
    display: block;
  }
  .weather-box .temp-search-sm{
    display: block;
  }
  .weather-box .temp-search{
    display: none;
  }
}
@media (min-width:768px) {
  .weather-box .weather {
    display: block;
  }
  .weather-box.temp-search{
    display: block;
  }
}
@media (min-width:768x) and (max-width:1200px){
  .weather-box .weather {
    display: block;
  }
  .weather-box.temp-search{
    display: block;
  }
}
</style>

