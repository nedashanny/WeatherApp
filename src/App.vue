<script>
  import axios from "axios";
  import HourlyWeather from "./components/HourlyWeather.vue";
  import DailyWeather from "./components/DailyWeather.vue";
  import CurrentWeather from "./components/CurrentWeather.vue";
  export default{
    data(){
      return{
        ipAdress:null,
        currentconditions:null,
        forecastsHourly:null,
        forecastsDaily:null,
        city:'',
        locations:null,
        showLocations:true,
      }
    },
    components:{
      HourlyWeather,DailyWeather,CurrentWeather
    },
    beforeMount(){
      // get user ip
      axios.get('https://api.ipify.org?format=json')
          .then(response=>{
            this.ipAdress=response.data.ip
          })
    },
    mounted(){
      // find user location
      axios.get('http://dataservice.accuweather.com/locations/v1/cities/ipaddress?q='+this.ipAdress+'&apikey=gnT2YKAwjOYiOGoR2cG0lLMXVyW2JUsA')
          .then(response=>{
                 this.displayWeather(response.data.Key)        
          })
    },
    methods:{
      displayWeather(location){
        axios.get('http://dataservice.accuweather.com/currentconditions/v1/'+location+'?apikey=gnT2YKAwjOYiOGoR2cG0lLMXVyW2JUsA')
              .then(response=>{
                  this.currentconditions=response
              })
        axios.get('http://dataservice.accuweather.com/forecasts/v1/hourly/12hour/'+location+'?apikey=gnT2YKAwjOYiOGoR2cG0lLMXVyW2JUsA')
              .then(response=>{
                  this.forecastsHourly=response.data
              })    
        axios.get('http://dataservice.accuweather.com/forecasts/v1/daily/5day/'+location+'?apikey=gnT2YKAwjOYiOGoR2cG0lLMXVyW2JUsA')
              .then(response=>{
                  this.forecastsDaily=response.data.DailyForecasts
              })  
        this.showLocations=true      
      },
      search(){
        axios.get('https://dataservice.accuweather.com/locations/v1/cities/search?q='+this.city+'&apikey=gnT2YKAwjOYiOGoR2cG0lLMXVyW2JUsA')
                .then( (response)=> {
                  this.locations=response.data
                })
        this.showLocations=false       
      },
      
      }
    }
</script>

<template>
  <nav class="navbar bg-body-tertiary">
    <div class="container">
      <a class="navbar-brand">Accu</a>
      <form @submit.prevent="search()" class="d-flex" role="search">
        <input v-model="city" class="form-control me-2" type="search" placeholder="Location" aria-label="Location">
        <button class="btn btn-outline-success" type="submit">Search</button>
      </form>
    </div>
  </nav>
  <div class="container">
    <!-- locations -->
    <ul class="list-group mt-1" :class="{'d-none':showLocations}">
      <li class="list-group-item" v-for="location in locations" :key="location" @click="displayWeather(location.Key)">{{ location.Country.LocalizedName+','+location.LocalizedName }}</li>
    </ul>
    <!-- current weather -->
    <div v-if="currentconditions!=null">
      <CurrentWeather title="current" :weather="currentconditions.data"/>
    </div>
    <!-- 12 hours forcast weather -->
    <div v-if="forecastsHourly!=null">
        <HourlyWeather title="hourly" :forecasts="forecastsHourly"/>    
    </div>
    <!-- 5days forcast weather -->
    <div v-if="forecastsDaily!=null">
        <DailyWeather title="daily" :forecasts="forecastsDaily"/>
    </div>
  </div>
  
</template>